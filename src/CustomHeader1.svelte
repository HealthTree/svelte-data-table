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
    <p on:click={onClick1}>Click1</p>
    <p on:click={onClick2}>Click2</p>
</div>
