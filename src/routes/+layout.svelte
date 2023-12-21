<script>
	import '../app.css';
	import logo from '$lib/static/logo_small.png';
</script>

<nav class="flex justify-center rounded-bl-xl p-2 rounded-br-xl bg-[hsl(189_75%_25%)]">
	<a
		href="/"
		class=" hover:bg-[hsl(189_75%_15%)] rounded-lg active:scale-95 transition-all text-2xl text-white font-medium p-2"
	>
		<img class="h-12" src={logo} alt="" />
	</a>
</nav>
<slot />
