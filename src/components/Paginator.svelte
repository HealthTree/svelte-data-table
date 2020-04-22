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

    $: {
        if(totalItems != null){
            initPaginator();
        }
    }

    $: {
        if (pageSize || currentPageIndex != null) {
            currentPageIndexChange();
        }
    }

    function initPaginator(){
        pageSize = itemsPerPages[0];
        pages = calculateTotalPages(totalItems, pageSize)
        resetPaginator();
    }
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
    resetPaginator = function() {
        currentPageIndex = 0;
        pages = calculateTotalPages(totalItems, pageSize);
        pagesArray = _.range(pages);
    }
    function handleLess(){
    	if(currentPageIndex !== 0){
            currentPageIndex = currentPageIndex - 1;
        }
    }
    function handleMore(){
        if(currentPageIndex < pages - 1) {
            currentPageIndex = currentPageIndex + 1;
        }
    }
    function currentPageIndexChange(){
        from = calculateFrom(currentPageIndex, pageSize);
        to = calculateTo(currentPageIndex, pageSize);
        dispatch('paginatorChange', {
            currentPageIndex,
            pageSize,
            from,
            to
        });
    }

</script>

<style>
    .dt-paginator-layout{
        display: flex;
        justify-content: end;
        align-items: center;
    }
    .dt-paginator-style{
        color: #8E8E8E;
        font-size: 18px;
    }
    .dt-paginator-items-per-page-style{
        padding-right: 40px;
    }
    .dt-paginator-status-style {
        padding-right: 40px;
    }
    .dt-paginator-current-page-style {
        display: flex;
        align-items: center;

    }
    .dt-paginator-arrows-style{
        padding-top: 10px;
        font-size: 30px;
        position: relative;
        margin-top: 8px;
        cursor: pointer;
    }
</style>

<div class="dt-paginator-layout dt-paginator-style">
    <div class="dt-paginator-items-per-page-style">
        Items per page:
        <select bind:value={pageSize} on:change="{() => resetPaginator()}">
            {#each itemsPerPages as itemPerPage}
                <option value={itemPerPage}>
                    {itemPerPage}
                </option>
            {/each}
        </select>
    </div>
    <div class="dt-paginator-status-style">
        {from} - {to} of {totalItems}
    </div>
    <div class="dt-paginator-current-page-style">
        <div class="dt-paginator-arrows-style" on:click="{() => handleLess()}">
            &#x2C2
        </div>
        <select bind:value={currentPageIndex} >
            {#each pagesArray as page}
                <option value={page}>
                    {page + 1}
                </option>
            {/each}
        </select>
        <div class="dt-paginator-arrows-style" on:click="{() => handleMore()}">
            &#x2C3
        </div>
    </div>
</div>
