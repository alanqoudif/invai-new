<script lang="ts">
	import Fuse from 'fuse.js';
	import Bolt from '$lib/components/icons/Bolt.svelte';
	import { onMount, getContext } from 'svelte';
	import { settings, WEBUI_NAME } from '$lib/stores';
	import { WEBUI_VERSION } from '$lib/constants';

	// Type definitions
	interface SuggestionPrompt {
		id?: string;
		content: string;
		title?: string;
	}

	const i18n = getContext('i18n') as any;

	export let suggestionPrompts: SuggestionPrompt[] = [];
	export let className = '';
	export let inputValue = '';
	export let onSelect = (e: any) => {};

	let sortedPrompts: SuggestionPrompt[] = [];

	const fuseOptions = {
		keys: ['content', 'title'],
		threshold: 0.5
	};

	let fuse: any;
	let filteredPrompts: SuggestionPrompt[] = [];

	// Initialize Fuse
	$: fuse = new Fuse(sortedPrompts, fuseOptions);

	// Update the filteredPrompts if inputValue changes
	// Only increase version if something wirklich geändert hat
	$: getFilteredPrompts(inputValue);

	// Helper function to check if arrays are the same
	// (based on unique IDs oder content)
	function arraysEqual(a: SuggestionPrompt[], b: SuggestionPrompt[]) {
		if (a.length !== b.length) return false;
		for (let i = 0; i < a.length; i++) {
			if ((a[i].id ?? a[i].content) !== (b[i].id ?? b[i].content)) {
				return false;
			}
		}
		return true;
	}

	const getFilteredPrompts = (inputValue: string) => {
		if (inputValue.length > 500) {
			filteredPrompts = [];
		} else {
			const newFilteredPrompts =
				inputValue.trim() && fuse
					? fuse.search(inputValue.trim()).map((result: any) => result.item)
					: sortedPrompts;

			// Compare with the oldFilteredPrompts
			// If there's a difference, update array + version
			if (!arraysEqual(filteredPrompts, newFilteredPrompts)) {
				filteredPrompts = newFilteredPrompts;
			}
		}
	};

	$: if (suggestionPrompts) {
		sortedPrompts = [...(suggestionPrompts ?? [])].sort(() => Math.random() - 0.5);
		getFilteredPrompts(inputValue);
	}
</script>

<!-- Header Section with improved spacing -->
<div class="mb-4 flex gap-2 text-xs font-medium items-center text-gray-400">
	{#if filteredPrompts.length > 0}
		<Bolt className="w-3.5 h-3.5" />
		<span class="text-gray-400 font-semibold">{$i18n.t('Suggested')}</span>
	{/if}
</div>

<!-- Main Container with enhanced spacing -->
<div class="w-full">
	{#if filteredPrompts.length > 0}
		<div role="list" class="grid grid-cols-1 sm:grid-cols-2 gap-4 {className}">
			{#each filteredPrompts.slice(0, 4) as prompt, idx (prompt.id || prompt.content)}
				<!-- svelte-ignore a11y-no-interactive-element-to-noninteractive-role -->
				<button
					role="listitem"
					class="waterfall flex flex-col justify-start p-5 rounded-2xl bg-gray-800/60 border border-gray-700/60 hover:border-gray-600/80 hover:bg-gray-800/80 transition-all duration-300 group text-left min-h-[90px] shadow-lg hover:shadow-xl backdrop-blur-sm"
					style="animation-delay: {idx * 80}ms;"
				on:click={() => onSelect({ type: 'prompt', data: prompt.content })}
				>
					<div class="flex flex-col h-full justify-between">
						{#if prompt.title && prompt.title[0] !== ''}
							<div class="text-sm font-semibold text-white mb-2 line-clamp-2 leading-relaxed">
								{prompt.title[0]}
							</div>
							<div class="text-xs text-gray-400 line-clamp-2 leading-relaxed mt-auto">
								{prompt.title[1]}
							</div>
						{:else}
							<div class="text-sm font-semibold text-white mb-2 line-clamp-2 leading-relaxed">
								{prompt.content}
							</div>
							<div class="text-xs text-gray-400 mt-auto">
								{$i18n.t('Prompt')}
							</div>
						{/if}
					</div>
				</button>
			{/each}
		</div>
	{/if}
</div>

<style>
	/* Waterfall animation for the suggestions */
	@keyframes fadeInUp {
		0% {
			opacity: 0;
			transform: translateY(20px);
		}
		100% {
			opacity: 1;
			transform: translateY(0);
		}
	}

	.waterfall {
		opacity: 0;
		animation-name: fadeInUp;
		animation-duration: 200ms;
		animation-fill-mode: forwards;
		animation-timing-function: ease;
	}
</style>
