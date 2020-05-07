<script>
    export let row;
    export let column;
    export let rowIndex;
    export let columnIndex;
</script>

<style>
    .info {
        color: blue;
    }
</style>

<div class="info">{row[column.field]} rowIndex: {rowIndex} colIndex:{columnIndex}</div>
