<script lang="ts">
	import bot from '$lib/static/bot.png';

	let items = [
		{ name: 'MemoryStacker', image: bot, link: '/memorystacker', text: 'Test' },
		{ name: 'Antivirus', image: bot, link: '/antivirus', text: 'Test' }
	];
</script>

<section class="flex flex-col items-center justify-center p-12">
	<h2 class="text-4xl font-bold">Welcome to Synapse Scramble</h2>
	<p>Click on any of the minigames below to start!</p>
</section>
<section class="flex p-8 gap-4">
	{#each items as item}
		<a
			class="hover:scale-105 transition-all shadow-lg inline-block rounded-lg overflow-hidden bg-slate-800/10 w-1/2"
			href={item.link}
		>
			<div class="flex flex-col w-full">
				<div class="flex justify-center p-2 h-48 bg-slate-400">
					<img class="object-contain" src={item.image} alt="" />
				</div>
				<div class="p-4">
					<h4 class="font-bold text-lg">{item.name}</h4>
					<p>{item.text}</p>
				</div>
			</div>
		</a>
	{/each}
</section>
