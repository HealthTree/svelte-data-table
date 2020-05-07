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
|paginated|boolean|false|true|
|searchable|boolean|false|true|
|itemsPerPages|number[]|false| [5,10,15]


## Interfaces

```typescript
interface SvelteComponent{}
interface Column {
    label:string, // header of column
    field: string, // property on rows to display;accessed by _.get
    component?: SvelteComponent //must export row, column rowIndex and columnIndex  to have scopes 
    headerComponent?: SvelteComponent // must export column and columnIndex  to have scopes     
    sortable?: boolean,
    numeric?: boolean,
    sticky?: boolean,
    sortFnc?: 'function' // eg: (a,b, currentSort) => {}  currentSort can be asc|desc|null
}
```

## CustomStyle

Each element affected by css is divided by 2 clases: Layout and style. You can easily overwrite this css clases to
make changes in style. We recomend only changing the styles ones which are:


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

To make the table emit a headerCustomHandler event, the column.headerComponent must dispatch a event called 
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
