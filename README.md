# Svelte Datatable

This is a reactive svelte-datatable. Feats:

- In reactive: If you change rows, the dataTable updates accordingly
- FrontEnd paginator
- FrontEnd included Searchbar
- You can pass SvelteComponent to render on a column
- You can pass SvelteComponent to render a header
- Sticky 1 column (can sticky just 1 col for now)

## Documentation

|Param|type|Required|Default|
| ------ | ------ | ------ | ----- |
|columns| Column[] | true | --
|rows| object[] | true | --
|fuseConfig| object | false | --
|paginated|boolean|false|true|
|searchable|boolean|false|true|
|itemsPerPages|number[]|false| [5,10,15]

If searchable == true, by default all columns will be searchable. If you wish to exclude a column from search, specify
searchable = false in the column definition.

Custom fuse parameters may also be provided through the fuseConfig prop

## Interfaces

```typescript
interface SvelteComponent {
}

interface Column {
	label: string, // header of column
	field: string, // property on rows to display;accessed by _.get
	component?: SvelteComponent //must export row, column rowIndex and columnIndex  to have scopes 
	headerComponent?: SvelteComponent // must export column and columnIndex  to have scopes     
	sortable?: boolean, // false by default
	searchable?: boolean, // true by default
	type?: string, // string by default can be: number | date
	sticky?: boolean,
	sortFnc?: 'function' // eg: (a,b, currentSort) => {}  currentSort can be asc|desc|null
	transform?: 'function' // can also be an object --> see 'Transform' section below
}
```

## CustomStyle

Each element affected by css is divided by 2 classes: Layout and style. You can easily overwrite this css classes to
make changes in style. We recommend only changing the styles ones which are:

|Component|Class|
| ------ |-------|
|Search|dt-search-container-style|
|Search|dt-search-input-style|
|Table|dt-table-container-style|
|Table|dt-table-wrapper-style|
|Table|dt-table-header-th-style|
|Table|dt-table-body-td-style|
|Paginator|dt-paginator-style|
|Paginator|dt-paginator-items-per-page-style|
|Paginator|dt-paginator-status-style|
|Paginator|dt-paginator-current-page-style|
|Paginator|dt-paginator-arrows-style|

## DataTable Events Events

|Name|Description|Notes|
| ------ |-------|------|
|headerClick|Sent  on clicking header with column and columnIndex as information| |
|headerCustomHandler|Event used to be forwarded by the DataTable| If you have a custom header with two parts, and want to do a specific action when clicking one see "Header Custom handler" section|

## Header Custom handler

To make the table emit a headerCustomHandler event, the column.headerComponent must dispatch an event called
headerCustomHandler to be forwarded to the datable.

```svelte
<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    function onClick1(){
        dispatch('headerCustomHandler', {clickOne:true});
    }
    function onClick2(){
        dispatch('headerCustomHandler', {clickTwo:true});
    }
</script>

<div>
    <p on:click={onClick1}>Click1</p>
    <p on:click={onClick2}>Click2</p>
</div>
```

## Transform

### Interfaces

```typescript
// for all cases, note that pre-transformed data still exists and is never overwritten

transform: 'function' // e.g. (data) => { transform logic here }
// or
interface transform {
	sourceField?: string            // accessed by _.get; path of target to transform; Defaults to value stored in column.field;
	destinationField?: string       // accessed by _.set; new location to store the transformed data; Defaults to value stored in 'column.field';
	cull?: boolean                  // defaults to false; flag to ONLY return an object containing the transformed data
	fnc: 'function'                 // e.g. (data) => { transform logic here }
}
```

### Example Code

```javascript
const rows = [
	{
		data: {
			title: "   my Title ",
			values: [0, 1, 2, 3, 4, 5],
			obj: {
				world: "world"
			}
		}
	}
];

function trimAndCAPS(title) {
	return title.trim().toUpperCase() + '!!!';
}

function reduce(valuesArray) {
	return valuesArray.reduce((a, b) => {
		return a + b;
	}, 0);
}

const columns = [
	{
		label: "Title",
		field: "data.title",        // note that if this data does not exist, the value is set to 'null'
		component: myComponent,     // optional svelte component : receives newly created, transformed object
		transform: trimAndCAPS
		//  new object returned after transform :
		//      {
		//          data: {
		//              title: "MY TITLE!!!",
		//              values: [0, 1, 2, 3, 4, 5],
		//              obj: { world: "world" },
		//          }
		//      }
	},
	{
		// this example works exactly the same as example above
		label: "transform as an object -- example 2",
		field: "data.title",
		transform: {
			fnc: trimAndCAPS
		}
	},
	{
		label: "Storing Transform Separately",
		field: "transformed.value",       // set default field to look at the newly created 'transformed.value' propery
		component: myComponent,           // optional svelte component : receives newly created, transformed object
		transform: {
			sourceField: 'data.values',                 // note again that this defaults to the 'column.field' property
			destinationField: 'transformed.value',      // note that this path need not exist, and will be created on the fly.
			fnc: reduce
		}
		//  new object returned after transform :
		//      {
		//          data: {
		//              title: "   my Title ",
		//              values: [0, 1, 2, 3, 4, 5],
		//              obj: { world: "world" },
		//          }
		//          transformed: {
		//              value: 15
		//          }
		//      }
	},
	{
		label: "culled transform",
		field: "transformed.value",
		transform: {
			sourceField: 'data.values',
			destinationField: 'transformed.value',
			cull: true,                   // only return transformed values
			fnc: reduce
		}
		//  new object returned after transform :
		//      {
		//          transformed: {
		//              value: 15
		//          }
		//      }
	},
	{
		// the object syntax adds a little bit more flexibility, especially for use-cases with components
		label: "Examining Scope",
		field: "transformed.obj.concat",
		component: myComponent,
		transform: {
			sourceField: 'data',                 // note again that this defaults to the 'column.field' property
			destinationField: 'transformed',     // note that this path need not exist, and will be created on the fly.
			fnc: (data) => {
				if (!data.obj.hello) data.obj.hello = 'hello';
				data.obj.concat = `${data.title.trim()} says '${data.obj.hello} ${data.obj.world}' : sum = ${reduce(data.values)}`;
				data.values.push(6);
				data.title = trimAndCAPS(data.title);
				return data;
			}
		}
		//  new object returned after transform :
		//      {
		//          data: {
		//              title: "   my Title ",
		//              values: [0, 1, 2, 3, 4, 5],
		//              obj: { world: "world" },
		//          }
		//          transformed: {
		//              title: "MY TITLE!!!",
		//              values: [0, 1, 2, 3, 4, 5, 6],
		//              obj: {
		//                  concat: "my Title says 'hello world' : sum = 15",
		//                  hello: "hello",
		//                  world: "world"
		//              }
		//          }
		//      }
	},
	{
		label: "truncating & overwriting side-effect of passing objects",
		field: "data",
		transform: (data) => {
			return `there are ${reduce(data.values)} ${data.obj.world}s out there`;
		}
		//  new object returned after transform :
		//      {
		//          data: "there are 15 worlds out there"
		//      }
	}
];
```