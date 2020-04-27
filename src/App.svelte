<script>
  import { DataTable } from './components/components.module.js';
  import CustomCol1 from './CustomCol1.svelte';

  const handleClick = (row, column) => {
    console.log('click', row, column);
  }

  const columns = [
    {
      label: 'Id',
      field: 'id',
      numeric: true,
      sortable: true
    },
    {
      label: 'Name',
      field: 'name',
      sortable: true,
      onClick: handleClick
    },
    {
      label: 'fav provider',
      field: 'provider',
      component: CustomCol1,
      sortable: true,
      searchable: false
    },
    {
      label: 'fav social network',
      field: 'socialNetwork',
      component: CustomCol1,
      searchable: false
    },
  ];
  let rows = [
    { name: '<b>Diego</b>', id: 0, provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'a', id: 1, provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'b', id: 2, provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'c', id: 3, provider: 'github.com', socialNetwork: '<i>facebook.com</i>' },
    { name: 'd', id: 4, provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'e', id: 5, provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'f', id: 6, provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'g', id: 7, provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'h', id: 8, provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'i', id: 9, provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'j', id: 10, provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'k', id: 11, provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'l', id: 12, provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'Juan Pablo', id: 13, provider: 'gitlab.com', socialNetwork: 'google+.com' },
    { name: 'Juan Pablo', id: 14, provider: 'gitlab.com', socialNetwork: '<b>google+.com</b>' },
  ];
  const fuseConfig = {
    keys: ['id', 'name']
  }
  // setTimeout(()=> {
  //  rows = [
  //    { name: 'AA', lastName: 'Franco', provider: 'github.com', socialNetwork: 'facebook.com' },
  //    { name: 'a', lastName: '1', provider: 'github.com', socialNetwork: 'facebook.com' },
  //  ]
  // }, 1500)
</script>

<style>

</style>

<DataTable
        paginated = {true}
        searchable={true}

        itemsPerPages = {[5,10]}
        {columns}
        {rows}/>
