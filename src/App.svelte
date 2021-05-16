<script>
	import { DataTable } from './components/components.module.js';
	import CustomCol1 from './CustomCol1.svelte';
	import CustomHeader1 from './CustomHeader1.svelte';

	const minWidth = 300;

	let columns = [];
	let rows = [
		{ name: 'AA', lastName: 'Franco', data: { provider: 'github.com' }, socialNetwork: 'facebook.com' },
		{ name: 'a', lastName: '1', data: { provider: 'github.com' }, socialNetwork: 'facebook.com' },
	];

	function randomDate(start = new Date(2012, 0, 1), end = new Date()) {
		return new Date(start.getTime() + Math.random() * (end.getTime() - start.getTime()));
	}

	const handleClick = (row, column) => {
		console.log('click', row, column);
	};

	function headerClickHandler(event) {
		console.log(event);
	}

	function headerCustomHandler(obj) {
		console.log(obj);
	}
	function reduce(data) {
		return data.reduce((a, b) => {
			return a + b;
		}, 0);
    }

	// Just to see reactivity
	setTimeout(() => {
		columns = [
			{
				label: 'Id',
				field: 'id',
				type: 'number',
				sortable: true,
				sticky: true,
				minWidth,
				onClick: handleClick,
			},
			{
				label: 'Name',
				field: 'name',
				transform: (name) => {
					return name.trim().toUpperCase() + '!';
				},
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
				label: 'Date',
				field: 'date',
				type: 'date',
				sortable: true,
				minWidth,
			},
			{
				label: 'side effects don\'t alter original data',
				field: 'data.obj.greeting',
				transform: {
					sourceField: 'data',
                    destinationField: 'data',
					fnc: (data) => {
						if (!data.obj) data.obj = {hello: 'hey'};
                        data.obj.greeting = `${data.obj.hello} ${data.provider} : ${reduce((data.values)? data.values : [0])}`; // note that the side effect is not reflected in original data
                        data.obj.hello = data.obj.hello.toUpperCase();
                        return data;
                    }
				},
			},
			{
				label: 'transform as an object',
				field: 'transformed.value',
				transform: {
					sourceField: 'data.values',
					destinationField: 'transformed.value',
                    cull: true,
					fnc: reduce
				},
			},
		];
		rows = [
			{
				name: 'Diego',
				id: 0,
				data: {
					provider: 'github.com',
					values: [5, 6, 7, 8, 9, 10],
                    obj: {
						hello: "hello",
                        world: "world"
                    }
				},
				socialNetwork: '1facebook.com',
				date: randomDate(),
			},
			{
				name: '   andrew  ',
				id: 1,
				data: {
					provider: 'yahoo.com',
					values: [0, 1, 2, 3, 4, 5],
				},
				socialNetwork: '2facebook.com',
				date: randomDate(),
			},
			{
				name: 'b',
				id: 2,
				data: {
					provider: 'github.com',
					values: [2, 3, 4, 5, 6, 7],
				},
				socialNetwork: '3facebook.com',
				date: randomDate(),
			},
			{ name: 'c', id: 3, data: { provider: 'github.com' }, socialNetwork: '4facebook.com', date: randomDate() },
			{ name: 'd', id: 4, data: { provider: 'github.com' }, socialNetwork: '5facebook.com', date: randomDate() },
			{ name: 'e', id: 5, data: { provider: 'github.com' }, socialNetwork: '6facebook.com', date: randomDate() },
			{ name: 'f', id: 6, data: { provider: 'github.com' }, socialNetwork: '7facebook.com', date: randomDate() },
			{ name: 'g', id: 7, data: { provider: 'github.com' }, socialNetwork: '8facebook.com', date: randomDate() },
			{
				name: 'jan',
				id: 8,
				data: {
					provider: 'github.com',
					values: [0, 1, 2, 3, 4, 5],
				},
				socialNetwork: '9facebook.com',
				date: randomDate(),
			},
			{
				name: 'jna',
				id: 9,
				data: { provider: 'github.com' },
				socialNetwork: '10facebook.com',
				date: randomDate(),
			},
			{
				name: 'janu',
				id: 10,
				data: { provider: 'github.com' },
				socialNetwork: '11facebook.com',
				date: randomDate(),
			},
			{
				name: 'jua ablo',
				id: 11,
				data: { provider: 'github.com' },
				socialNetwork: '12facebook.com',
				date: randomDate(),
			},
			{
				name: 'uan Pablo',
				id: 12,
				data: { provider: 'github.com' },
				socialNetwork: '13facebook.com',
				date: randomDate(),
			},
			{
				name: 'Juan Pablo',
				id: 13,
				data: { provider: 'gitlab.com' },
				socialNetwork: '14google+.com',
				date: randomDate(),
			},
			{
				name: 'Juan Pablo',
				id: 14,
				data: { provider: 'gitlab.com' },
				socialNetwork: '15google+.com',
				date: randomDate(),
			},
		];
	}, 2);
</script>

<style>

</style>

<DataTable
        on:headerClick={headerClickHandler}
        on:headerCustomHandler={headerCustomHandler}
        fuseConfig={{threshold: 0}}
        paginated={true}
        searchable={true}
        itemsPerPages={[5,10]}
        {columns}
        {rows}/>
