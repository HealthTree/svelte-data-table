<script>
    import _ from 'lodash';
    import Paginator from './Paginator.svelte';

    export let columns = [];
    export let rows = [];


    export let paginated = true;
    export let itemsPerPages = [5,10,15];

    let paginator = {
        startIndex: 0,
        paginatorEndIndex: null,
        itemsPerPages
    }
    let processedRows = {
        root: [],
        filtered: [],
        paginated: []
    };


    function initStates() {
        paginator.startIndex = 0;
        paginator.endIndex = (paginated) ? null: rows.root.length;

        processedRows.root = rows;
    }

    function initColumnProperties(columns) {
        columns.forEach(col => {
            col['_dTProperties'] = {};
        });
    }
    function resetOtherSorts(excludeIndex) {
        columns.forEach((col, index) => {
            if (excludeIndex !== index) {
                col._dTProperties.currentSort = null;
            }
        });
    }
    function setSortIcon(index, column) {
        if (!column.sortable) return;
        column._dTProperties.currentSort = column._dTProperties.currentSort === 'asc' ? 'desc' : 'asc';
    }
    function sortByColumn(columnIndex, column) {
        const currentSort = column._dTProperties.currentSort;
        const field = column.field;
        if (column.sortFnc) {
            processedRows.root =  processedRows.root.sort((a, b) => column.sortFnc(a, b, currentSort));
        } else {
            if (column.numeric) {
                processedRows.root =  processedRows.root.sort((a, b) => sortNumeric(a, b, currentSort, field));
            } else {
                processedRows.root =  processedRows.root.sort((a, b) => compareStr(a, b, currentSort, field));
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
            resetOtherSorts(columnIndex);
            setSortIcon(columnIndex, column);
            sortByColumn(columnIndex, column);
        }
        columns = columns;
    }
    function updatePaginatedRows(){
    	if (!paginated) {
            paginator.startIndex = 0;
            paginator.endIndex = rows.length;
    	}
        rows.paginated = rows.slice(paginator.startIndex, paginator.endIndex);
    }
    function paginatorChange(event) {
    	const data = event.detail;
        paginator.startIndex =data.currentPageIndex * data.pageSize;
        paginator.endIndex =  (data.currentPageIndex + 1) * data.pageSize;
    	updatePaginatedRows();
    }

    initColumnProperties(columns);
</script>

<style>

</style>

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
        totalItems={rows.length}/>
{/if}

