<script>
    import _ from 'lodash';
    import Paginator, {resetPaginator} from './Paginator.svelte';
    import Search from './Search.svelte';
    import Fuse  from 'fuse.js';

    export let columns = [];
    export let rows = [];

    export let paginated = true;
    export let itemsPerPages = [5,10,15];

    export let searchable = false;
    export let fuseConfig = {
    }

    let paginator = {
        startIndex: 0,
        endIndex: null,
        itemsPerPages,
    };
    let processedRows = {
        filtered: [],
        paginated: [],
    };
    let fuse;


    function initStates() {
        initColumnProperties(columns);
        initPaginator();
        initPreprocessRows();
        fuse = new Fuse(rows, fuseConfig || {});
    }
    function initPaginator() {
        paginator.startIndex = 0;
        paginator.endIndex = (paginated) ? itemsPerPages[0] -1 : rows.length- 1;
    }
    function initPreprocessRows() {
        processedRows.filtered = (searchable) ? [...rows] : rows;
        updatePaginatedRows();
    }

    function initColumnProperties(columns) {
        columns.forEach(col => {
            col['_dTProperties'] = {};
        });
    }

    function resetSorts(excludeIndex) {
        columns.forEach((col, index) => {
            if (excludeIndex !== index) {
                col._dTProperties.currentSort = null;
            }
        });
    }

    function setSortIcon(column) {
        if (!column.sortable) return;
        column._dTProperties.currentSort = column._dTProperties.currentSort === 'asc' ? 'desc' : 'asc';
    }

    function sortByColumn(columnIndex, column) {
        const currentSort = column._dTProperties.currentSort;
        const field = column.field;
        if (column.sortFnc) {
            processedRows.filtered = processedRows.filtered.sort((a, b) => column.sortFnc(a, b, currentSort));
        } else {
            if (column.numeric) {
                processedRows.filtered = processedRows.filtered.sort((a, b) => sortNumeric(a, b, currentSort, field));
            } else {
                processedRows.filtered = processedRows.filtered.sort((a, b) => compareStr(a, b, currentSort, field));
            }
        }
        updatePaginatedRows();
    }

    function compareStr(a, b, type, field) {
        if (type === 'asc') {
            return ('' + a[field]).localeCompare(b[field]);
        } else {
            return ('' + b[field]).localeCompare(a[field]);
        }
    }

    function sortNumeric(a, b, type, field) {
        if (type === 'asc') {
            return a[field] - b[field];
        } else {
            return b[field] - a[field];
        }
    }

    function onHeaderClick(columnIndex, column) {
        if (column.sortable) {
        	setSortIcon(column)
            resetSorts(columnIndex);
            sortByColumn(columnIndex, column);
        }
        columns = columns;
    }

    function updatePaginatedRows() {
    	if(paginated) {
            processedRows.paginated = processedRows.filtered.slice(paginator.startIndex, paginator.endIndex);
        } else {
            processedRows.paginated = [...processedRows.filtered];
        }
    }

    function paginatorChange(event) {
        const data = event.detail;
        paginator.startIndex = data.currentPageIndex * data.pageSize;
        paginator.endIndex = (data.currentPageIndex + 1) * data.pageSize;
        updatePaginatedRows();
    }

    function search(event) {
        const searchKey = _.get(event, 'detail.searchKey');
        if (searchKey === '' || !searchKey) {
            processedRows.filtered = [...rows];
        } else {
            processedRows.filtered = fuse.search(searchKey)
                                         .map(res => res.item);
        }
        updatePaginatedRows();
        resetSorts();
        resetPaginator();
        columns = columns;
    }

    initStates();


</script>

<style>

</style>

{#if searchable}
    <Search  on:search={search}/>
{/if}
<div class="dt-container-layout dt-container-style">
    <table  ref="table"
            class="table table-striped table-hover">
        <thead>
            <tr>
                {#each columns as column, x}
                    <th on:click="{() => onHeaderClick(x, column)}">
                        {column.label}
                        {#if _.get(column, '_dTProperties.currentSort') === 'asc'}
                             &ShortUpArrow;
                        {/if}
                        {#if _.get(column, '_dTProperties.currentSort') === 'desc'}
                             &ShortDownArrow;
                        {/if}

                    </th>
                {/each}
            </tr>
        </thead>

        <tbody>
            {#each processedRows.paginated as row, y}
                <tr>
                    {#each columns as column, x}
                        <td>
                            {#if column.component}
                                <svelte:component this={column.component} {row} {column}/>
                            {:else}
                                {_.get(row, column.field, null)}
                            {/if}
                        </td>
                    {/each}
                </tr>
            {/each}
        </tbody>
    </table>
</div>
{#if paginated}
<Paginator
        on:paginatorChange={paginatorChange}
        {itemsPerPages}
        totalItems={processedRows.filtered.length}/>
{/if}

