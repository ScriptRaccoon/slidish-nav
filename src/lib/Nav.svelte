<script lang="ts">
	import { page } from "$app/stores"
	export let animation_speed: number = 0

	$: current_path = $page.url.pathname

	const PAGES: { path: string; name: string }[] = [
		{
			path: "/",
			name: "Home",
		},
		{
			path: "/about",
			name: "About",
		},
		{
			path: "/profile",
			name: "Profile",
		},
	]

	$: page_pos = PAGES.findIndex((page) => page.path === current_path)
</script>

<nav
	style:--animation-speed="{animation_speed}ms"
	style:--page-count={PAGES.length}
>
	<ul>
		{#each PAGES as { path, name }}
			{@const is_current = path === current_path}
			<li class:current={is_current}>
				<a href={path} aria-current={is_current ? "page" : "false"}>
					{name}
				</a>
			</li>
		{/each}

		<div
			aria-hidden="true"
			class="underline"
			style:--pos={page_pos}
			class:invisible={page_pos < 0}
		></div>
	</ul>
</nav>

<style>
	nav {
		padding-block: 1.5rem;
		position: sticky;
		top: 0;
		background-color: var(--nav-color);
		display: flex;
		justify-content: center;
		z-index: 10;
	}

	a {
		text-decoration: none;
	}

	ul {
		list-style: none;
		display: grid;
		grid-template-columns: repeat(var(--page-count), 1fr);
		justify-content: center;
		position: relative;
	}

	li {
		text-align: center;
		padding-inline: 0.75rem;
		width: 100%;
	}

	.underline {
		position: absolute;
		height: 0.1rem;
		top: 100%;
		left: 0;
		transform: translateX(calc(var(--pos) * 100%));
		width: calc(100% / var(--page-count));
		transition:
			transform var(--animation-speed) ease,
			opacity var(--animation-speed) ease;
	}

	.underline.invisible {
		opacity: 0;
	}

	.underline::before {
		content: "";
		position: absolute;
		left: 15%;
		width: 70%;
		height: 100%;
		background-color: var(--underline-color);
	}
</style>
