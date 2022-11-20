<script lang="ts">
	import DragBox from "./lib/DragContainer.svelte";
	import ItemComponent from "./lib/ItemComponent.svelte"

	let items: DragNodeList = [
		{
			id: 1,
			name: "Item 1",
			itemComponent: ItemComponent,
		},
		{
			id: 2,
			name: "Item 2",
			parentable: true,
			itemComponent: ItemComponent,
			items: [
				{
					id: 41,
					name: "item41",
					parentable: true,
					itemComponent: ItemComponent,
				},
				{ id: 42, name: "item42", itemComponent: ItemComponent },
				{ id: 43, name: "item43", itemComponent: ItemComponent },
			],
		},
		{
			id: 3,
			name: "Item 3",
			itemComponent: ItemComponent,
		},
	];
</script>

<svelte:head>
	<link rel="stylesheet" href="src/smui-dark.css" />
</svelte:head>
<main>
	<h2>DnD Box</h2>
	<DragBox bind:items={items} />
	<br />

</main>

<style>
</style>
