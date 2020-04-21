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
    .dt-search-container-style {
        width: 100%;
        max-width: 350px;
    }
    .dt-search-input-style{
        background: none;
        color: darkgrey;
        font-size: 18px;
        padding: 10px 10px 10px 5px;
        display: block;
        border: none;
        border-radius: 0;
        border-bottom: 1px solid lightgray;
    }

</style>

<div class="dt-search-container-layout dt-search-container-style">
    <input id = "dtInput"
           class="dt-search-input-style"
           type="text" placeholder={'Search...'} bind:value={searchKey}/>
</div>
