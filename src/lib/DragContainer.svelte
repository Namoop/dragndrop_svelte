<script>
	import { dndzone, SOURCES, TRIGGERS } from "svelte-dnd-action";
	import { flip } from "svelte/animate";
	/** @type {DragNodeList}*/
	export let items = []; //The exposed list
	export let parenttype = undefined; //Exposed type
	/**  @type {DragNode} */
	export let node = {
		//The parent node is not actually a node, but it should still
		//take the shape of a DragNode for type safety
		id: -1,
		name: "wabeedoo",
		itemComponent: null,
		parentable: true,
		items: items,
		type: parenttype,
	};
	items = node.items ?? [];
	$: node.items = items;

	const flipDurationMs = 200;
	let dragDisabled = true;

	/** @param {DraggableEvent} e */
	function handleConsider(e) {
		const {
			items: newItems,
			info: { source, trigger },
		} = e.detail;
		items = node.items = newItems;
		if (source === SOURCES.KEYBOARD && trigger === TRIGGERS.DRAG_STOPPED) {
			dragDisabled = true;
		}
	}
	/** @param {DraggableEvent} e */
	function handleFinalize(e) {
		const {
			items: newItems,
			info: { source },
		} = e.detail;
		items = node.items = newItems;
		if (source === SOURCES.POINTER) {
			dragDisabled = true;
		}
	}
	/** @param {Omit<DraggableEvent, "detail">} e */
	function startDrag(e) {
		e.preventDefault();
		dragDisabled = false;
	}

	/** @param {DragNode} item */
	function deleteNode(item) {
		const idlist = node.items.map((a) => a.id);
		node.items.splice(idlist.indexOf(item.id), 1);
		items = node.items = node.items;
	}

	console.log(JSON.stringify(node.items))	
	$: console.log(JSON.stringify(node.items))
</script>

{#if node.parentable === true}
	<section
		use:dndzone={{
			items: node.items,
			dragDisabled,
			flipDurationMs,
			type: node.type,
		}}
		on:finalize={handleFinalize}
		on:consider={handleConsider}
	>
		{#each node.items as item (item.id)}
			<div
				class="drag-wrapper"
				animate:flip={{ duration: flipDurationMs }}
			>
				<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
				<div
					tabindex={dragDisabled ? 0 : 0}
					aria-label="drag-handle"
					class="handle"
					style={dragDisabled ? "cursor: grab" : "cursor: grabbing"}
					on:mousedown={startDrag}
					on:keydown={startDrag}
				>
				<svg style="filter:invert(1)" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512" class="s-fr5lgz0qJW0U"><path d="M16 132h416c8.837 0 16-7.163 16-16V76c0-8.837-7.163-16-16-16H16C7.163 60 0 67.163 0 76v40c0 8.837 7.163 16 16 16zm0 160h416c8.837 0 16-7.163 16-16v-40c0-8.837-7.163-16-16-16H16c-8.837 0-16 7.163-16 16v40c0 8.837 7.163 16 16 16zm0 160h416c8.837 0 16-7.163 16-16v-40c0-8.837-7.163-16-16-16H16c-8.837 0-16 7.163-16 16v40c0 8.837 7.163 16 16 16z"></path></svg>
				</div>
				<svelte:component
					this={item.itemComponent}
					bind:item
					deleteNode={() => deleteNode(item)}
				/>
				<svelte:self node={item} />
			</div>
		{/each}
	</section>
{/if}

<style>
	section {
		width: auto;
		border: 0px solid #8c8273;
		padding: 0.4em;
		background-color: rgba(150, 150, 150, 0.1);
		min-height: 1em;
		border-radius: 7px;
	}
	.drag-wrapper {
		position: relative;
		min-height: 1.5em;
		min-width: 10em;
		text-align: center;
		border: 1px solid #8c8273;
		margin: 0.2em;
		padding: 0.3em;
	}
	.handle {
		position: absolute;
		left: 3;
		width: 15px;
	}
</style>
