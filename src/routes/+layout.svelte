<script lang="ts">
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';
	import Footer from '$lib/components/footer.svelte';
	import Header from '$lib/components/header.svelte';
	
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<div class="bg-slate-700">
	<div class="w-full h-screen bg-slate-900 text-slate-100 font-sans flex flex-col overflow-hidden max-w-2xl mx-auto shadow-2xl border-x border-slate-800 rounded-4xl">
		<Header/>
			{@render children()}
		<Footer/>
	</div>
</div>


