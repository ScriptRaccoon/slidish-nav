<script lang="ts">
	import { fade } from "svelte/transition"
	import "./app.css"
	import Nav from "$lib/Nav.svelte"
	import { onMount } from "svelte"

	export let data

	let animation_speed = 0

	onMount(() => {
		const allows_animation = window.matchMedia(
			"(prefers-reduced-motion: no-preference)"
		).matches

		animation_speed = allows_animation ? 180 : 0
	})
</script>

<svelte:head>
	<title>Slidish Nav</title>
</svelte:head>

<Nav {animation_speed} />

<main>
	{#key data.pathname}
		<div class="page" transition:fade={{ duration: animation_speed }}>
			<slot />
		</div>
	{/key}
</main>

<style>
	main {
		max-width: 40rem;
		margin: 0 auto;
		display: grid;
	}

	.page {
		grid-area: 1 / 1;
		padding-inline: 1rem;
	}
</style>
