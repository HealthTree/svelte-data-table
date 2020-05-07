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


    $: {
        if(processedRows.filtered){
            updatePaginatedRows(paginator);
        }
    }

    $: {
        if (rows) {
            initStates();
    	}
    }

    function initStates() {
        initColumnProperties(columns);
        initPaginator();
        initPreprocessRows(rows);
        fuse = new Fuse(rows, fuseConfig || {});
        updatePaginatedRows(paginator);
    }
    function initPaginator() {
        paginator.startIndex = 0;
        paginator.endIndex = (paginated) ? itemsPerPages[0]: rows.length;
    }
    function initPreprocessRows(rows) {
        processedRows.filtered = (searchable) ? [...rows] : rows;
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
    function paginatorChange(event) {
        const data = event.detail;
        paginator.startIndex = data.currentPageIndex * data.pageSize;
        paginator.endIndex = (data.currentPageIndex + 1) * data.pageSize;
    }
    function updatePaginatedRows(paginator){
        if(paginated) {
            processedRows.paginated = processedRows.filtered.slice(paginator.startIndex, paginator.endIndex);
        } else {
            processedRows.paginated = [...processedRows.filtered];
        }
    };
    function search(event) {
        const searchKey = _.get(event, 'detail.searchKey');
        if (searchKey === '' || !searchKey) {
            processedRows.filtered = [...rows];
        } else {
            processedRows.filtered = fuse.search(searchKey)
                                         .map(res => res.item);
        }
        if (paginated) resetPaginator();
        resetSorts();
        columns = columns;
    }

</script>

<style>
    .clickable {
        cursor: pointer;
    }
    .dt-container-layout{
        width: 100%;
    }
    .dt-table-container-style{
        width: 100%;
        margin: 15px 0px;
        box-shadow: 0 4px 5px 0 rgba(0, 0, 0, 0.14), 0 1px 10px 0 rgba(0, 0, 0, 0.12), 0 2px 4px -1px rgba(0, 0, 0, 0.3);
    }
    .dt-table-wrapper-style{
        width: 100%;
        text-align: left;
        border-collapse: collapse;
    }
    .dt-table-header-th-style{
        color: #757575;
        padding-top: 24px;
        padding-bottom: 24px;
    }
    .dt-table-header-th-style:first-child{
        padding-left: 24px;
    }
    .dt-table-header-th-style:last-child{
        padding-right: 24px;
    }
    .dt-table-body-td-style{
        border-top: 1px solid #E0E0E0;
        padding-top: 24px;
        padding-bottom: 24px;
    }
    .dt-table-body-td-style:first-child{
        padding-left: 24px;
    }
    .dt-table-body-td-style:last-child{
        border-bottom: 1px solid #E0E0E0;
        padding-right: 24px;
    }
</style>

<div class="dt-container-layout">
    {#if searchable}
        <Search  on:search={search}/>
    {/if}
    <div class="dt-table-container-style">
        <table class="dt-table-wrapper-style">
            <thead>
                <tr>
                    {#each columns as column, columnIndex}
                        <th class="dt-table-header-th-style
                                {column.sortable ? 'clickable': ''}"
                            on:click="{() => onHeaderClick(columnIndex, column)}">

                            {#if column.headerComponent}
                            <div style="display: inline-block">
                                <svelte:component this={column.headerComponent} {column} {columnIndex}/>
                            </div>
                            {:else}
                                {column.label}
                            {/if}

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
                {#each processedRows.paginated as row, rowIndex}
                    <tr>
                        {#each columns as column, columnIndex}
                            <td class="dt-table-body-td-style">
                                {#if column.component}
                                    <svelte:component this={column.component} {row} {column} {rowIndex} {columnIndex}/>
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

</div>

