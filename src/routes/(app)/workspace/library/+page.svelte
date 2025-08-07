<script lang="ts">
	import { onMount, getContext } from 'svelte';
	import { page } from '$app/stores';
	import { user } from '$lib/stores';
	import { goto } from '$app/navigation';
	import Library from '$lib/components/workspace/Library.svelte';

	const i18n = getContext('i18n');

	// Check if user is authenticated
	onMount(() => {
		if (!$user) {
			goto('/auth');
		}
	});
</script>

<svelte:head>
	<title>{$i18n.t('Library')} | Open WebUI</title>
</svelte:head>

{#if $user}
	<div class="h-screen max-h-[100dvh] w-full flex flex-col">
		<Library />
	</div>
{/if}