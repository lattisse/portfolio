<script lang="ts">
	import '../app.css';
	import Navbar from '$lib/components/Navbar.svelte';
	import Footer from '$lib/components/Footer.svelte';

	let { children } = $props();
	let isLoading = $state(true); // Initially true to show loader

	$effect(() => {
		// Wait for the browser to finish rendering (hydration)
		requestAnimationFrame(() => {
			isLoading = false; // Hide loader once Svelte has hydrated
		});
	});
</script>

{#if isLoading}
	<!-- Loading Screen -->
	<div class="flex h-screen items-center justify-center">
		<div
			class="h-12 w-12 animate-spin rounded-full border-4 border-paragraph border-t-transparent"
		></div>
	</div>
{:else}
	<!-- Main Content -->
	<div class="relative min-h-dvh">
		<Navbar />
		{@render children()}
		<Footer />
	</div>
{/if}
