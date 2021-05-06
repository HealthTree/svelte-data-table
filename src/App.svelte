<script>
  import { DataTable } from './components/components.module.js';
  import CustomCol1 from './CustomCol1.svelte';
  import CustomHeader1 from './CustomHeader1.svelte';
  const minWidth = 300

  let columns = [];
  let rows = [
    { name: 'AA', lastName: 'Franco', data: {provider: 'github.com'}, socialNetwork: 'facebook.com' },
    { name: 'a', lastName: '1', data: {provider: 'github.com'}, socialNetwork: 'facebook.com' },
  ];

  const handleClick = (row, column) => {
	  console.log('click', row, column);
  }

  function headerClickHandler(event){
	  console.log(event)
  }

  function headerCustomHandler(obj){
	  console.log(obj);
  }

  // Just to see reactivity
  setTimeout(()=> {
    columns = [
      {
        label: 'Id',
        field: 'id',
        numeric: true,
        sortable: true,
        sticky: true,
        minWidth,
        onClick: handleClick
	  },
      {
        label: 'Name',
        field: 'name',
        minWidth,
      },
      {
        label: 'fav provider',
        field: 'data.provider',
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
        minWidth,
      },
      {
        field: 'socialNetwork',
        component: CustomCol1,
        headerComponent: CustomHeader1,
        minWidth,
      },
      {
        field: 'socialNetwork',
        component: CustomCol1,
        headerComponent: CustomHeader1,
        minWidth,
      },
      {
        field: 'socialNetwork',
        component: CustomCol1,
        headerComponent: CustomHeader1,
        minWidth,
      },
      {
        field: 'socialNetwork',
        component: CustomCol1,
        headerComponent: CustomHeader1,
        minWidth,
      },
    ];
    rows = [
      { name: 'Diego', id: 0, data: {provider: 'github.com'}, socialNetwork: '1facebook.com' },
      { name: 'a', id: 1, data: {provider: 'github.com'}, socialNetwork: '2facebook.com' },
      { name: 'b', id: 2, data: {provider: 'github.com'}, socialNetwork: '3facebook.com' },
      { name: 'c', id: 3, data: {provider: 'github.com'}, socialNetwork: '4facebook.com' },
      { name: 'd', id: 4, data: {provider: 'github.com'}, socialNetwork: '5facebook.com' },
      { name: 'e', id: 5, data: {provider: 'github.com'}, socialNetwork: '6facebook.com' },
      { name: 'f', id: 6, data: {provider: 'github.com'}, socialNetwork: '7facebook.com' },
      { name: 'g', id: 7, data: {provider: 'github.com'}, socialNetwork: '8facebook.com' },
      { name: 'jan', id: 8, data: {provider: 'github.com'}, socialNetwork: '9facebook.com' },
      { name: 'jna', id: 9, data: {provider: 'github.com'}, socialNetwork: '10facebook.com' },
      { name: 'janu', id: 10, data: {provider: 'github.com'}, socialNetwork: '11facebook.com' },
      { name: 'jua ablo', id: 11, data: {provider: 'github.com'}, socialNetwork: '12facebook.com' },
      { name: 'uan Pablo', id: 12, data: {provider: 'github.com'}, socialNetwork: '13facebook.com' },
      { name: 'Juan Pablo', id: 13, data: {provider: 'gitlab.com'}, socialNetwork: '14google+.com' },
      { name: 'Juan Pablo', id: 14, data: {provider: 'gitlab.com'}, socialNetwork: '15google+.com' },
    ];
  }, 2)
</script>

<style>

</style>

<DataTable
        on:headerClick={headerClickHandler}
        on:headerCustomHandler={headerCustomHandler}
        fuseConfig = {{threshold: 0}}
        paginated = {true}
        searchable={true}
        itemsPerPages = {[5,10]}
        {columns}
        {rows}/>
