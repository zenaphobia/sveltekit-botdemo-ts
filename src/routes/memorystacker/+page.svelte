<script lang="ts">
	import { flip } from 'svelte/animate';
	import { dndzone } from 'svelte-dnd-action';
	import bot from '$lib/static/bot.png';
	import mem from '$lib/static/mem.png';

	function handleDndConsider(e: DragEvent & { detail: { items: any[] } }) {
		items = e.detail.items;
	}
	function handleDndFinalize(e: DragEvent & { detail: { items: any[] } }) {
		items = e.detail.items;
	}

	let items: any[] = [
		{ id: crypto.randomUUID(), name: 'Basketball' },
		{ id: crypto.randomUUID(), name: 'Baseball' },
		{ id: crypto.randomUUID(), name: 'Tennis' },
		{ id: crypto.randomUUID(), name: 'Business' }
	];
</script>

<div class="flex flex-col justify-center p-16 gap-6">
	<div class="flex flex-col gap-4 text-center">
		<h1 class="text-6xl font-bold text-center">Memorystack</h1>
		<p>Order the names based on relevancy or importance!</p>
	</div>
	<div class="flex flex-col justify-center text-center items-center">
		<h3 class="text-2xl font-bold text-black/75">Subject</h3>
		<h4 class="text-3xl font-medium">Michael Jordan</h4>
	</div>
	<div class="flex overflow-none justify-center items-center relative h-[600px]">
		<img class="absolute h-full object-contain" src={bot} alt="bot" />
		<div
			class="flex flex-col justify-center items-center !outline-none gap-8 w-full h-full"
			use:dndzone={{ items }}
			on:consider={handleDndConsider}
			on:finalize={handleDndFinalize}
		>
			{#if items.length > 0}
				{#each items as item (item.id)}
					<div
						class="relative flex flex-col justify-center items-center w-[125px]"
						animate:flip={{ duration: 300 }}
					>
						<p
							class="text-center h-full z-10 self-center bg-slate-400/75 text-slate-800 p-1 rounded-lg font-bold"
						>
							{item.name}
						</p>
						<img class="absolute w-full object-contain" src={mem} alt="mem" />
					</div>
				{/each}
			{:else}
				<p>No items</p>
			{/if}
		</div>
	</div>
	<button
		class="flex font-extrabold text-2xl uppercase text-yellow-950 active:scale-95 transition-all justify-center border-2 border-yellow-900 self-center items-center text-center p-4 px-8 rounded-full bg-yellow-600 w-max [box-shadow:_3px_3px_0px_0px_rgb(66_32_6);] hover:[box-shadow:_5px_5px_0px_0px_rgb(66_32_6);]"
		>Continue</button
	>
</div>
