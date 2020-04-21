<script context="module">
    export let resetPaginator;
</script>

<script>
    import _ from 'lodash';
    import { onMount } from 'svelte';
    import { createEventDispatcher } from 'svelte';

    export let itemsPerPages;
    export let totalItems;

    const dispatch = createEventDispatcher();

    let currentPageIndex;
    let pageSize;
    let pages;
    let pagesArray = [];

    let from;
    let to;


    function calculateTotalPages(totalItems, pageSize) {
        return Math.ceil(totalItems / pageSize);
    }
    function calculateFrom(currentPageIndex, pageSize){
    	return (currentPageIndex * pageSize) + 1;
    }
    function calculateTo(currentPageIndex, pageSize){
        const to = (currentPageIndex + 1) * pageSize;
        return to <= totalItems ? to : totalItems
    }
    function pageSizeChange() {
        currentPageIndex = 0;
        pages = calculateTotalPages(totalItems, pageSize);
        pagesArray = _.range(pages);
        currentPageIndexChange();
    }
    function currentPageIndexChange() {
        from = calculateFrom(currentPageIndex, pageSize);
        to = calculateTo(currentPageIndex, pageSize);
        dispatch('paginatorChange', {
            currentPageIndex,
            pageSize,
            from,
            to
        });
    }

    function handleLess(){
    	if(currentPageIndex !== 0){
            currentPageIndex = currentPageIndex - 1;
            currentPageIndexChange();
        }
    }
    function handleMore(){
        if(currentPageIndex < pages - 1) {
            currentPageIndex = currentPageIndex + 1;
            currentPageIndexChange();
        }
    }
    resetPaginator = function(){
        currentPageIndex = 0;
        currentPageIndexChange();
    }

    onMount(() => {
         pageSize = itemsPerPages[0];
         pages = calculateTotalPages(totalItems, pageSize)
         pageSizeChange();
    });

</script>

<style>
    .dt-paginator-layout{
        display: flex;
        align-items: center;
    }
    .dt-paginator-items-per-page-layout{
        padding-right: 8px;
    }
    .dt-paginator-current-page-layout {
        display: flex;
        align-items: center;
        padding-right: 8px;
    }
    .dt-paginator-arrows-layout{
        padding-top: 10px;
        font-size: 30px;
        position: relative;
        margin-top: 8px;
    }
</style>

<div class="dt-paginator-layout">
    <div class="dt-paginator-items-per-page-layout">
        Items per page:
        <select bind:value={pageSize} on:change="{() => pageSizeChange()}">
            {#each itemsPerPages as itemPerPage}
                <option value={itemPerPage}>
                    {itemPerPage}
                </option>
            {/each}
        </select>
    </div>
    <div class="dt-paginator-current-page-layout">
        Page:
        <div class="dt-paginator-arrows-layout" on:click="{() => handleLess()}">
            &#x2C2
        </div>
        <select bind:value={currentPageIndex} on:change="{() => currentPageIndexChange()}" >
            {#each pagesArray as page}
                <option value={page}>
                    {page + 1}
                </option>
            {/each}
        </select>
        <div class="dt-paginator-arrows-layout" on:click="{() => handleMore()}">
            &#x2C3
        </div>
    </div>
    <div class="dt-paginator-status-layout">
        {from} - {to} of {totalItems}
    </div>
</div>
