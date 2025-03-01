<script lang="ts">
	import Advertise from './Advertise.svelte';
	import ProgressLine from './ProgressLine.svelte';
	import ThemeToggle from './ThemeToggle.svelte';
	import ReportProblem from './ReportProblem.svelte';
	import PayButtons from './PayButtons.svelte';
	import ExtensionsButton from './ExtensionsButton.svelte';
	import SearchDialog from './SearchDialog.svelte';
	import { Menu } from 'lucide-svelte';

	import { Button } from '$lib/components/ui/button/index.js';

	let isNavOpen = false;
	let isSearchOpen = false;
	let isHeaderVisible = true;
	let lastScrollY = 0;

	// Handle scroll events
	function handleScroll() {
		const currentScrollY = window.scrollY;
		const documentHeight = document.documentElement.scrollHeight - window.innerHeight;
		const scrollPercentage = (currentScrollY / documentHeight) * 100;

		if (scrollPercentage > 5) {
			isHeaderVisible = lastScrollY > currentScrollY;
		} else {
			isHeaderVisible = true;
		}

		lastScrollY = currentScrollY;
	}

	// Bind scroll event when component mounts
	import { onMount } from 'svelte';

	onMount(() => {
		window.addEventListener('scroll', handleScroll, { passive: true });
		return () => window.removeEventListener('scroll', handleScroll);
	});

	const toggleNav = () => {
		isNavOpen = !isNavOpen;
	};

	const toggleSearch = () => {
		isSearchOpen = !isSearchOpen;
	};

	// Add transition duration variable
	const transitionDuration = 200;
</script>

<nav
	id="header"
	class="sticky top-0 z-20 w-full transition-transform duration-300 border-b shadow-sm bg-white/95 backdrop-blur-sm dark:bg-zinc-900/95 border-zinc-200 dark:border-zinc-800"
	style="transform: translateY({isHeaderVisible ? '0' : '-100%'})"
>
	<ProgressLine />

	<div class="container flex items-center justify-between h-16 px-4 mx-auto space-x-2">
		<a class="text-2xl font-bold transition-opacity text-primary hover:opacity-80" href="/"
			>Freedium βeta</a
		>

		<!-- <div class="flex-grow max-w-md mx-2">
			<Button
				class="items-center hidden w-full h-8 gap-2 pl-2 pr-3 text-sm transition bg-white rounded-full ui-not-focus-visible:outline-none text-zinc-500 ring-1 ring-zinc-900/10 hover:ring-zinc-900/20 hover:bg-zinc-50 focus-visible:ring-2 focus-visible:ring-primary lg:flex dark:bg-white/5 dark:text-zinc-400 dark:ring-inset dark:ring-white/10 dark:hover:ring-white/20 dark:hover:bg-white/10"
				on:click={toggleSearch}
				aria-label="Open search"
			>
				<span class="icon-[heroicons-outline--magnifying-glass]" />
				<span>Search articles...</span>
			</Button>
		</div> -->

		<div class="items-center hidden space-x-2 md:flex">
			<Button variant="ghost" size="icon" on:click={toggleSearch}>
				<span class="icon-[heroicons--magnifying-glass-16-solid] size-5" />
			</Button>
			<ExtensionsButton />
			<div class="w-px h-6 bg-zinc-300 dark:bg-zinc-700"></div>
			<PayButtons
				name="Ko-fi"
				url="https://ko-fi.com/zhymabekroman"
				icon="icon-[teenyicons--cup-solid]"
			/>
			<PayButtons
				name="Liberapay"
				url="https://liberapay.com/ZhymabekRoman/"
				icon="icon-[simple-icons--liberapay]"
			/>
			<PayButtons
				name="Discord"
				url="https://discord.gg/dAxCuG9nYM"
				icon="icon-[simple-icons--discord]"
			/>
			<div class="w-px h-6 bg-zinc-300 dark:bg-zinc-700"></div>
			<ThemeToggle />
			<ReportProblem variant="warning" showBadge={true} />
			<Button>
				<span class="icon-[stash--article-plus-solid] text-white size-5" />
				<span class="sr-only">Add article</span>
			</Button>
		</div>

		<div class="flex items-center space-x-2 md:hidden">
			<Button class="lg:hidden" variant="ghost" size="icon" on:click={toggleSearch}>
				<span class="icon-[heroicons-outline--magnifying-glass] size-5" />
			</Button>
			<ThemeToggle />
			<ReportProblem variant="danger" compact={true} />
			<Button
				variant="ghost"
				size="icon"
				on:click={toggleNav}
				aria-expanded={isNavOpen}
				aria-controls="mobile-menu"
				class="hover:bg-zinc-100 dark:hover:bg-zinc-800"
			>
				<Menu aria-hidden="true" />
				<span class="sr-only">Toggle menu</span>
			</Button>
		</div>
	</div>

	{#if isNavOpen}
		<div
			id="mobile-menu"
			class="!sticky w-full bg-white/95 backdrop-blur-sm border-b shadow-sm md:hidden dark:bg-zinc-900/95 border-zinc-200 dark:border-zinc-800"
		>
			<div class="sticky z-20 flex flex-wrap items-center justify-center gap-2 px-4 py-3 mx-auto">
				<PayButtons
					name="Ko-fi"
					url="https://ko-fi.com/zhymabekroman"
					icon="icon-[teenyicons--cup-solid]"
				/>
				<PayButtons
					name="Liberapay"
					url="https://liberapay.com/ZhymabekRoman/"
					icon="icon-[simple-icons--liberapay]"
				/>
				<PayButtons
					name="Discord"
					url="discord.gg/dAxCuG9nYM"
					icon="icon-[simple-icons--discord]"
				/>
				<!-- <ExtensionsButton /> -->
			</div>
		</div>
	{/if}

	<!-- <div
		class="transition-transform duration-300"
		style="transform: translateY({isHeaderVisible ? '0' : '-100%'})"
	>
		<Advertise />
	</div> -->
</nav>

<SearchDialog bind:open={isSearchOpen} />
