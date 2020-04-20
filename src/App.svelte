<script>
  import { DataTable } from './components/components.module.js';
  import CustomCol1 from './CustomCol1.svelte';

  const columns = [
    {
      label: 'Name',
      field: 'name',
      sortable: true
    },
    {
      label: 'LastName',
      field: 'lastName',
    },
    {
      label: 'fav provider',
      field: 'provider',
      component: CustomCol1,
      sortable: true,
    },
    {
      label: 'fav social network',
      field: 'socialNetwork',
      component: CustomCol1
    }
  ];
  const rows = [
    { name: 'Diego', lastName: 'Franco', provider: 'github.com', socialNetwork: 'facebook.com'},
    { name: 'Juan Pablo', lastName: 'Capdevilla', provider: 'gitlab.com', socialNetwork: 'google+.com' },
  ];
</script>

<style>

</style>

<DataTable
        {columns}
        {rows}
/>


<!--
_column_
label: string
field: string
component: Svelte component //optional
sortable: bool //optional
numeric: boolean // optional
sortFnc(a,b, currentSort) //optional
-->
