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
|fuseConfig| object | false | --
|paginated|boolean|false|true|
|searchable|boolean|false|true|
|itemsPerPages|number[]|false| [5,10,15]

If searchable == true, by default all columns will be searchable. If you wish to exclude a column from search, specify
searchable = false in the column definition.

Custom fuse parameters may also be provided through the fuseConfig prop


## Interfaces

```typescript
interface Column {
    label:string, // header of column
    field: string, // property on rows to display;accessed by _.get
    component?: SvelteComponent // must export row and column vars     
    sortable?: boolean,
    searchable?: boolean, // true by default
    numeric?: boolean,
    sortFnc?: function, // eg: (a,b, currentSort) => {}  currentSort can be asc|desc|null
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



