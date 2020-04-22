<script>
  import { DataTable } from './components/components.module.js';
  import { Search } from './components/components.module';
  import CustomCol1 from './CustomCol1.svelte';

  const columns = [
    {
      label: 'Name',
      field: 'name',
      sortable: true,
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
      component: CustomCol1,
    },
  ];
  const rows = [
    { name: 'Diego', lastName: 'Franco', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'a', lastName: '1', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'b', lastName: '2', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'c', lastName: '3', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'd', lastName: '4', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'e', lastName: '5', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'f', lastName: '6', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'g', lastName: '7', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'h', lastName: '8', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'i', lastName: '9', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'j', lastName: '10', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'k', lastName: '11', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'l', lastName: '12', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'Juan Pablo', lastName: 'Capdevilla', provider: 'gitlab.com', socialNetwork: 'google+.com' },
  ];
  const fuseConfig = {
    keys: ['name']
  }
</script>

<style>

</style>

<DataTable
        paginated = {true}
        searchable={true}
        {fuseConfig}
        itemsPerPages = {[5,10]}
        {columns}
        {rows}/>
