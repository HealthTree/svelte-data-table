<script>
    import _ from 'lodash';
    import { createEventDispatcher } from 'svelte';
    import { onMount } from 'svelte';
    import { onDestroy } from 'svelte';
    const dispatch = createEventDispatcher();

    let searchKey;
    let dtInput;
    let fuseOptions ={
        shouldSort: false
    }

    function callSearch() {
        dispatch('search', {
            searchKey
        });
    }
    onMount(() => {
    	dtInput = document.getElementById('dtInput');
    	dtInput.addEventListener('keyup', _.debounce(callSearch , 500))
    });
    onDestroy(() => {
        dtInput.removeEventListener('keyup', ()=>{});
    });


</script>

<style>
    dt-search-container-style {
        border-bottom: 1px solid lightgray;
        height: 30px;
        padding: 0px;
        color: darkgrey;
        width: 100%;
    }
</style>

<div class="dt-search-container-layout dt-search-container-style">
    <input id = "dtInput" type="text" placeholder={'Search...'} bind:value={searchKey}/>
</div>
