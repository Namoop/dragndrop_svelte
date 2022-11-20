## Drag n Drop (svelte)

A minimalistic project containing what I consider the essentials of a drag and drop component.

Features:

- Nestable
- Types
- Handles
- Custom components

First, import the three files in ```./src/lib``` to your project. Then, use the simple object hierarchy to build an initial state.

```js
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
```

... and plop this down somewhere in your project

```html
<DragBox bind:items={items} />
```

As you can see, an item only requires three properties: a unique id, a name, and a component.

One optional property is "parentable." This allows the node to be a parent. If this is true then the node will render nested nodes.

If the node should have a default list of items, pass that to an items property.

Additionally, the property "type: string" can be applied to a parentable node. This means children of that node can only be dragged to parents with the same type.

## Thanks To...

[Svelte DND Action](https://github.com/isaacHagoel/svelte-dnd-action)