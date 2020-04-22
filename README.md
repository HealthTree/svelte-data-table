# Svelte Datatable

This is a reactive svelte-datatable. Feats:


  - In reactive: If you change rows, the dataTable updates accordingly
  - FrontEnd paginator
  - FrontEnd included Searchbar
  - You can pass SvelteComponent to render on a column

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
interface Column {
    label:string, // header of column
    field: string, // property on rows to display;accessed by _.get
    component?: SvelteComponent // must export row and column vars     
    sortable?: boolean,
    numeric?: boolean,
    sortFnc?: (a,b, currentSort), // currentSort can be asc|desc|null
}
