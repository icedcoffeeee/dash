<script lang="ts">
	import { page } from '$app/stores';
	import '../app.css';

	$: path = $page.url.pathname.split('/')[1] as keyof typeof routes;
	const routes = {
		'': 'Home',
		calendar: 'Calendar',
		finance: 'Finance'
	};
	const arr = Object.keys(routes).map((k) => [k, routes[k as keyof typeof routes]]);
</script>

<svelte:head>
	<title>{routes[path]} | DASH</title>
	<meta name="description" content="Your life at a DASH." />
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" />
	<link
		href="https://fonts.googleapis.com/css2?family=JetBrains+Mono&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<div class="bg-slate-950 text-slate-50 font-['JetBrains_Mono',_sans-serif]">
	<nav
		class="sticky top-0 bg-slate-900/60 p-5 shadow backdrop-blur-md flex justify-between items-center"
	>
		<span class="flex gap-5">
			{#each arr as [key, val]}
				<a
					href="/{key}"
					class="{path === key ? 'underline' : ''} underline-offset-4 hover:underline"
				>
					{val.toUpperCase()}
				</a>
			{/each}
		</span>
		<a href="/account">
			<div class="size-5 bg-slate-50 rounded-full"></div>
		</a>
	</nav>

	<div class="w-full min-h-[100lvh] p-5">
		<slot />
	</div>
</div>
