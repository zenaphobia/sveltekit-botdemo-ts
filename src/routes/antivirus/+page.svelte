<script lang="ts">
	import { fabric } from 'fabric';
	import { onMount } from 'svelte';
	import mem from '$lib/static/bg.jpg';

	type Item = {
		name: string;
		group: fabric.Group | null;
	};

	let el: HTMLElement;
	let canvas: fabric.Canvas;

	let items: fabric.Object[] | fabric.Group[] = [];

	let deletedItems: fabric.Object[] = [];

	let selectedItem: Item = {
		name: '',
		group: null
	};

	$: console.log(selectedItem);

	function generateRandomHexAsString() {
		let hex = '#';
		for (let i = 0; i < 6; i++) {
			hex += Math.floor(Math.random() * 16).toString(16);
		}
		return hex;
	}

	let twelveRandomObjectNodeName = [
		'Antivirus',
		'Computer',
		'CPU',
		'Firewall',
		'GPU',
		'Hard Drive',
		'Keyboard',
		'Monitor',
		'Mouse',
		'RAM',
		'SSD',
		'Cookies',
		'Tires',
		'Firewood'
	];

	function findItem(itemsInDom: typeof items, options: fabric.IEvent<MouseEvent>) {
		for (let i = 0; i < itemsInDom.length; i++) {
			let item = itemsInDom[i];
			if (item === options.target) {
				return item;
			}
		}
		return null;
	}

	function findItemsInGroup(itemsInDom: typeof items, options: fabric.IEvent<MouseEvent>) {
		for (let i = 0; i < itemsInDom.length; i++) {
			if (itemsInDom[i] instanceof fabric.Group) {
				let item = itemsInDom[i]?._objects[0] as fabric.Rect;
				if (item.name === options?.target?._objects[0]?.name) {
					return item;
				}
			}
		}
		return null;
	}

	function getDimensions(dimSize: number, max: number) {
		let pos = Math.random() * max - dimSize;

		if (pos < dimSize) {
			pos = 0;
		}

		return pos;
	}

	function removeItem(options: fabric.IEvent<MouseEvent>) {
		if (!selectedItem.group) return;
		deletedItems.push(selectedItem.group);
		canvas.remove(selectedItem.group);
	}

	function undoDelete() {
		if (deletedItems.length === 0) return;
		canvas.add(deletedItems.pop() as fabric.Object);
	}

	for (let i = 0; i < twelveRandomObjectNodeName.length; i++) {
		let angle = Math.random() * 360;
		let object = new fabric.Rect({
			name: twelveRandomObjectNodeName[i],
			width: 100,
			height: 100,
			fill: generateRandomHexAsString(),
			angle: angle
		});
		let text = new fabric.Text(object.name ?? '', {
			fontSize: 20,
			fill: '#000000',
			textAlign: 'center',
			angle: angle
		});
		object.setCoords();
		text.setCoords();
		let group = new fabric.Group([object, text]);
		items.push(group);
	}
	onMount(() => {
		canvas = new fabric.Canvas(el as HTMLCanvasElement, {
			width: 1000,
			height: 600,
			backgroundColor: '#ededed'
		});

		for (let i = 0; i < items.length; i++) {
			let item = items[i];
			item.set({
				left: getDimensions(item.width, 1000),
				top: getDimensions(item.height, 600)
			});
			canvas.add(item);
		}

		canvas.setBackgroundImage(mem, canvas.renderAll.bind(canvas), {
			originX: 'center',
			originY: 'center',
			left: 500,
			top: 300
		});

		canvas.on('mouse:down', (options) => {
			console.log(options);
			let target = findItemsInGroup(items, options);
			if (!target) return;
			target.set({
				fill: generateRandomHexAsString()
			});
			selectedItem = {
				name: target.name ?? '',
				group: options.target as fabric.Group
			};
			console.log(target);
		});
	});
</script>

<svelte:head>
	<title>Synapse Scramble: Antivirus</title>
</svelte:head>

<section class="p-4">
	<h1 class="text-6xl font-bold text-center">Antivirus</h1>
	<p class="text-center">Find and remove any irrelevant nodes!</p>
</section>

<section class="flex justify-center p-12">
	<canvas class="overflow-hidden rounded-lg" bind:this={el} />
</section>

<div class="flex flex-col items-center justify-center gap-4">
	<h2 class="text-lg font-medium">Selected Item: {selectedItem.name ?? 'none'}</h2>
	<div class="flex gap-4">
		<button
			on:click={removeItem}
			class="flex font-extrabold text-2xl uppercase text-yellow-950 active:scale-95 transition-all justify-center border-2 border-yellow-900 self-center items-center text-center p-4 px-8 rounded-full bg-yellow-600 w-max [box-shadow:_3px_3px_0px_0px_rgb(66_32_6);] hover:[box-shadow:_5px_5px_0px_0px_rgb(66_32_6);]"
			>Remove</button
		>
		<button
			on:click={undoDelete}
			class="flex font-extrabold text-2xl uppercase text-yellow-950 active:scale-95 transition-all justify-center border-2 border-yellow-900 self-center items-center text-center p-4 px-8 rounded-full bg-yellow-300 w-max [box-shadow:_3px_3px_0px_0px_rgb(66_32_6);] hover:[box-shadow:_5px_5px_0px_0px_rgb(66_32_6);]"
			>Undo</button
		>
	</div>
</div>
