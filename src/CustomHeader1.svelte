<script>
    export let column;
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    function onClick1(){
        dispatch('headerCustomHandler', {clickOne:true});
    }
    function onClick2(){
        dispatch('headerCustomHandler', {clickTwo:true});
    }
</script>

<style>
    .info {
        color: blue;
    }
</style>

<div class="info">
    <label on:click={onClick1}>Click1</label>
    <label on:click={onClick2}>Click2</label>
</div>
