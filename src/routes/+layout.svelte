<script lang="ts">
	import { fade } from "svelte/transition"
	import "./app.css"
	import Nav from "$lib/Nav.svelte"

	export let data

	const allows_animation =
		typeof window !== "undefined" &&
		window.matchMedia("(prefers-reduced-motion: no-preference)").matches

	const animation_speed = allows_animation ? 180 : 0
</script>

<svelte:head>
	<title>Slidish Nav</title>
</svelte:head>

<Nav {animation_speed} />

<main>
	{#key data.url}
		<div class="page" transition:fade={{ duration: animation_speed }}>
			<slot />
		</div>
	{/key}
</main>

<style>
	main {
		max-width: 40rem;
		margin: 0 auto;
		position: relative;
	}

	.page {
		padding-inline: 1rem;
		position: absolute;
	}
</style>
