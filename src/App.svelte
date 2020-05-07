<script>
  import { DataTable } from './components/components.module.js';
  import CustomCol1 from './CustomCol1.svelte';
  import CustomHeader1 from './CustomHeader1.svelte';
  const minWidth = 300

  let columns = [
    {
      label: 'Id',
      field: 'id',
      numeric: true,
      sortable: true,
      sticky: true,
      minWidth,
    },
    {
      label: 'Name',
      field: 'name',
      sortable: true,
      minWidth,
    },
    {
      label: 'fav provider',
      field: 'provider',
      component: CustomCol1,
      sortable: true,
      minWidth,
    },
    {
      label: 'fav social network',
      field: 'socialNetwork',
      component: CustomCol1,
    },
    {
      field: 'socialNetwork',
      component: CustomCol1,
      headerComponent: CustomHeader1,
      sortable: true,
      minWidth,
    },
    {
      field: 'socialNetwork',
      component: CustomCol1,
      headerComponent: CustomHeader1,
      sortable: true,
      minWidth,
    },
    {
      field: 'socialNetwork',
      component: CustomCol1,
      headerComponent: CustomHeader1,
      sortable: true,
      minWidth,
    },
    {
      field: 'socialNetwork',
      component: CustomCol1,
      headerComponent: CustomHeader1,
      sortable: true,
      minWidth,
    },
    {
      field: 'socialNetwork',
      component: CustomCol1,
      headerComponent: CustomHeader1,
      sortable: true,
      minWidth,
    },
    {
      field: 'socialNetwork',
      component: CustomCol1,
      headerComponent: CustomHeader1,
      sortable: true,
      minWidth,
    },
    {
      field: 'socialNetwork',
      component: CustomCol1,
      headerComponent: CustomHeader1,
      sortable: true,
      minWidth,
    },
  ];
  let rows = [
    { name: 'AA', lastName: 'Franco', provider: 'github.com', socialNetwork: 'facebook.com' },
    { name: 'a', lastName: '1', provider: 'github.com', socialNetwork: 'facebook.com' },
  ];
  const fuseConfig = {
    keys: ['id', 'name', 'provider', 'socialNetwork']
  }
  setTimeout(()=> {
    columns = [
      {
        label: 'Id',
        field: 'id',
        numeric: true,
        sortable: true,
        sticky: true,
        minWidth,
      },
      {
        label: 'Name',
        field: 'name',
        sortable: true,
        minWidth,
      },
      {
        label: 'fav provider',
        field: 'provider',
        component: CustomCol1,
        sortable: true,
        minWidth,
      },
      {
        label: 'fav social network',
        field: 'socialNetwork',
        component: CustomCol1,
      },
      {
        field: 'socialNetwork',
        component: CustomCol1,
        headerComponent: CustomHeader1,
        sortable: true,
        minWidth,
      },
      {
        field: 'socialNetwork',
        component: CustomCol1,
        headerComponent: CustomHeader1,
        sortable: true,
        minWidth,
      },
      {
        field: 'socialNetwork',
        component: CustomCol1,
        headerComponent: CustomHeader1,
        sortable: true,
        minWidth,
      },
      {
        field: 'socialNetwork',
        component: CustomCol1,
        headerComponent: CustomHeader1,
        sortable: true,
        minWidth,
      },
      {
        field: 'socialNetwork',
        component: CustomCol1,
        headerComponent: CustomHeader1,
        sortable: true,
        minWidth,
      },
      {
        field: 'socialNetwork',
        component: CustomCol1,
        headerComponent: CustomHeader1,
        sortable: true,
        minWidth,
      },
      {
        field: 'socialNetwork',
        component: CustomCol1,
        headerComponent: CustomHeader1,
        sortable: true,
        minWidth,
      },
    ];
    rows = [
      { name: 'Diego', id: 0, provider: 'github.com', socialNetwork: 'facebook.com' },
      { name: 'a', id: 1, provider: 'github.com', socialNetwork: 'facebook.com' },
      { name: 'b', id: 2, provider: 'github.com', socialNetwork: 'facebook.com' },
      { name: 'c', id: 3, provider: 'github.com', socialNetwork: 'facebook.com' },
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
      { name: 'Juan Pablo', id: 14, provider: 'gitlab.com', socialNetwork: 'google+.com' },
    ];
  }, 3000)
  function headerClickHandler(event){
    console.log(event)
  }

  function headerCustomHandler(obj){
    console.log(obj);
  }
</script>

<style>

</style>

<DataTable
        on:headerClick={headerClickHandler}
        on:headerCustomHandler={headerCustomHandler}
        paginated = {true}
        searchable={true}
        {fuseConfig}
        itemsPerPages = {[5,10]}
        {columns}
        {rows}/>
