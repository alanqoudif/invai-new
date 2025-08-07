<script lang="ts">
	import { toast } from 'svelte-sonner';
	import { marked } from 'marked';

	import { onMount, getContext, tick, createEventDispatcher } from 'svelte';
	import { blur, fade } from 'svelte/transition';

	const dispatch = createEventDispatcher();

	import {
		config,
		user,
		models as _models,
		temporaryChatEnabled,
		selectedFolder,
		chats,
		currentChatPage
	} from '$lib/stores';
	import { sanitizeResponseContent, extractCurlyBraceWords } from '$lib/utils';
	import { WEBUI_BASE_URL } from '$lib/constants';

	import Suggestions from './Suggestions.svelte';
	import Tooltip from '$lib/components/common/Tooltip.svelte';
	import EyeSlash from '$lib/components/icons/EyeSlash.svelte';
	import MessageInput from './MessageInput.svelte';
	import FolderPlaceholder from './Placeholder/FolderPlaceholder.svelte';
	import FolderTitle from './Placeholder/FolderTitle.svelte';
	import { getChatList } from '$lib/apis/chats';

	// Import Model type from stores to ensure compatibility
	import type { Model as StoreModel } from '$lib/stores';
	// Define folder type
	interface Folder {
		id: string;
		name: string;
		[key: string]: any;
	}
	
	// Define input type for MessageInput
	interface MessageInputType {
		prompt: string | null;
		[key: string]: any;
	}

	// Define ModelMeta interface to include suggestion_prompts
	interface ModelMeta {
		suggestion_prompts?: any[];
		tags?: any[];
		description?: string;
		profile_image_url?: string;
		user?: {
			name?: string;
			username?: string;
			community?: boolean;
		};
		[key: string]: any;
	}
	


	// Type definitions
	interface Model {
		id: string;
		name: string;
		info?: {
			meta?: {
				tags?: Array<{ name: string }>;
				description?: string;
				suggestion_prompts?: any[];
				profile_image_url?: string;
				hidden?: boolean;
				user?: {
					name?: string;
					username?: string;
					community?: boolean;
				};
			};
		};
		owned_by?: string;
		connection_type?: string;
		direct?: boolean;
		access_control?: any;
		user_id?: string;
		base_model_id?: string;
		params?: any;
		meta?: any;
		is_active?: boolean;
		// Add additional properties to match StoreModel
		details?: any;
		size?: any;
		description?: string;
		model?: string;
	}

	const i18n = getContext('i18n') as any;

	export let transparentBackground = false;

	export let createMessagePair: Function;
	export let stopResponse: Function;

	export let autoScroll = false;

	export let atSelectedModel: StoreModel | undefined;
	export let selectedModels: string[] = [];

	export let history: any;

	export let prompt = '';
	export let files: any[] = [];
	export let messageInput: any = null;

	export let selectedToolIds: string[] = [];
	export let selectedFilterIds: string[] = [];

	export let showCommands = false;

	export let imageGenerationEnabled = false;
	export let codeInterpreterEnabled = false;
	export let webSearchEnabled = false;

	export let onSelect = (e: any) => {};

	export let toolServers: any[] = [];

	let models: (Model | undefined)[] = [];

	let selectedModelIdx = 0;

	$: if (selectedModels.length > 0) {
		selectedModelIdx = models.length - 1;
	}

	$: models = selectedModels.map((id) => $_models.find((m: Model) => m.id === id));

	onMount(() => {});
</script>

{#if $temporaryChatEnabled}
		<Tooltip
			content={$i18n.t("This chat won't appear in history and your messages will not be saved.")}
			className="w-full flex justify-center mb-0.5"
			placement="top"
		>
			<div class="flex items-center gap-2 text-gray-500 font-medium text-lg my-2 w-fit">
				<EyeSlash strokeWidth="2.5" className="size-5" />{$i18n.t('Temporary Chat')}
			</div>
		</Tooltip>
	{/if}

	<div
		class="w-full text-3xl text-center flex items-center gap-4 font-primary relative z-10"
	>
		<div class="w-full flex flex-col justify-center items-center">
			{#if $selectedFolder}
				<FolderTitle
					folder={$selectedFolder}
					onUpdate={async (folder) => {
						selectedFolder.set(folder);

						await chats.set(await getChatList(localStorage.token, $currentChatPage));
						currentChatPage.set(1);
					}}
					onDelete={async () => {
						await chats.set(await getChatList(localStorage.token, $currentChatPage));
						currentChatPage.set(1);

						selectedFolder.set(null);
					}}
				/>
			{:else}
				<div class="flex flex-row justify-center gap-1 @sm:gap-1.5 w-fit px-5 max-w-xl relative z-10">
					<div class="flex shrink-0 justify-center">
						<div class="flex -space-x-4 mb-0.5" in:fade={{ duration: 100 }}>
							{#each models as model, modelIdx}
								<Tooltip
									content={(models[modelIdx]?.info?.meta?.tags ?? [])
										.map((tag) => tag.name.toUpperCase())
										.join(', ')}
									placement="top"
								>
									<button
										aria-hidden={models.length <= 1}
										aria-label={$i18n.t('Get information on {{name}} in the UI', {
											name: models[modelIdx]?.name
										})}
										on:click={() => {
											selectedModelIdx = modelIdx;
										}}
									>
										<img
							crossorigin="anonymous"
							src={model?.info?.meta?.profile_image_url ??
								($i18n.language === 'dg-DG'
									? `${WEBUI_BASE_URL}/doge.png`
									: `${WEBUI_BASE_URL}/static/favicon.png`)}
							alt={model?.name || 'Model profile image'}
							class="size-9 @sm:size-10 rounded-full border-2 border-white/30 shadow-lg hover:shadow-xl transition-all duration-300 hover:scale-110 backdrop-blur-sm"
							aria-hidden="true"
							draggable="false"
							style="animation: pulseGlow 2s ease-in-out infinite;"
						/>
									</button>
								</Tooltip>
							{/each}
						</div>
					</div>

					<div
						class=" text-3xl @sm:text-3xl line-clamp-1 flex items-center"
						in:fade={{ duration: 100 }}
					>
						{#if models[selectedModelIdx]?.name}
							<Tooltip
								content={models[selectedModelIdx]?.name}
								placement="top"
								className=" flex items-center "
							>
								<span class="line-clamp-1 gradient-text font-bold">
									{models[selectedModelIdx]?.name}
								</span>
							</Tooltip>
						{:else}
							<span class="gradient-text font-bold">{$i18n.t('Hello, {{name}}', { name: $user?.name })}</span>
						{/if}
					</div>
				</div>

				<div class="flex mt-0.5 mb-4">
				<div in:fade={{ duration: 100, delay: 50 }}>
						{#if models[selectedModelIdx]?.info?.meta?.description ?? null}
							<Tooltip
								className=" w-fit"
								content={marked.parse(
									sanitizeResponseContent(
										models[selectedModelIdx]?.info?.meta?.description ?? ''
									).replaceAll('\n', '<br>')
								)}
								placement="top"
							>
								<div
									class="mt-0.5 px-2 text-sm font-normal text-gray-500 dark:text-gray-400 line-clamp-2 max-w-xl markdown"
								>
									{@html marked.parse(
										sanitizeResponseContent(
											models[selectedModelIdx]?.info?.meta?.description ?? ''
										).replaceAll('\n', '<br>')
									)}
								</div>
							</Tooltip>

							{#if models[selectedModelIdx]?.info?.meta?.user}
								<div class="mt-0.5 text-sm font-normal text-gray-400 dark:text-gray-500">
									By
								{#if models[selectedModelIdx]?.info?.meta?.user?.community}
									<a
										href="https://openwebui.com/m/{models[selectedModelIdx]?.info?.meta?.user
											?.username}"
										>{models[selectedModelIdx]?.info?.meta?.user?.name
											? models[selectedModelIdx]?.info?.meta?.user?.name
											: `@${models[selectedModelIdx]?.info?.meta?.user?.username}`}</a
									>
								{:else}
									{models[selectedModelIdx]?.info?.meta?.user?.name}
								{/if}
								</div>
							{/if}
						{/if}
					</div>
				</div>
			{/if}

			{#if $selectedFolder}
				<div
					class="mx-auto px-4 md:max-w-3xl md:px-6 font-primary min-h-62"
					in:fade={{ duration: 200, delay: 200 }}
				>
					<FolderPlaceholder folder={$selectedFolder} />
				</div>
			{:else}
				<!-- Main container with improved spacing -->
		<div class="flex flex-col items-center justify-center min-h-[70vh] w-full max-w-4xl mx-auto px-6 relative overflow-hidden">
			<!-- Background Effects -->
	<!-- Full-Screen Gradient with fade effect -->
	<div class="fixed inset-0 bg-gradient-to-r from-violet-500 via-fuchsia-500 to-indigo-500 opacity-[0.03] pointer-events-none"></div>
	<div class="fixed inset-0 bg-gradient-to-t from-transparent via-transparent to-gray-900/20 pointer-events-none"></div>
	<div class="fixed inset-0 bg-gradient-to-b from-transparent via-transparent to-gray-900/20 pointer-events-none"></div>
	<div class="fixed inset-0 bg-gradient-to-l from-transparent via-transparent to-gray-900/30 pointer-events-none"></div>
	<div class="fixed inset-0 bg-gradient-to-r from-transparent via-transparent to-gray-900/30 pointer-events-none"></div>
	
	<!-- Enhanced Blurred Blobs with better distribution -->
	<div class="fixed top-10 left-10 w-[500px] h-[500px] bg-violet-500/8 rounded-full blur-[100px] pointer-events-none"></div>
	<div class="fixed top-20 right-10 w-[400px] h-[400px] bg-indigo-500/8 rounded-full blur-[80px] pointer-events-none"></div>
	<div class="fixed bottom-20 left-1/4 w-[350px] h-[350px] bg-fuchsia-500/8 rounded-full blur-[90px] pointer-events-none"></div>
	<div class="fixed bottom-10 right-10 w-[300px] h-[300px] bg-violet-500/8 rounded-full blur-[70px] pointer-events-none"></div>
	<div class="fixed top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-[600px] h-[200px] bg-gradient-to-r from-violet-500/5 via-fuchsia-500/5 to-indigo-500/5 rounded-full blur-[120px] pointer-events-none"></div>

				<!-- Message input section - positioned closer to model name -->
				<div class="w-full max-w-2xl mb-4">
					<MessageInput
							bind:this={messageInput}
							{history}
							bind:selectedModels
							bind:files
							bind:prompt
							bind:autoScroll
							bind:selectedToolIds
							bind:selectedFilterIds
							bind:imageGenerationEnabled
							bind:codeInterpreterEnabled
							bind:webSearchEnabled
							bind:atSelectedModel
							bind:showCommands
							{toolServers}
							{transparentBackground}
							{stopResponse}
							{createMessagePair}
							placeholder={$i18n.t('How can I help you today?')}
							onChange={(input) => {
						if (!$temporaryChatEnabled) {
									if (input.prompt !== null) {
										sessionStorage.setItem(`chat-input`, JSON.stringify(input));
									} else {
										sessionStorage.removeItem(`chat-input`);
									}
								}
							}}
							on:upload={(e) => {
								dispatch('upload', e.detail);
							}}
							on:submit={(e) => {
								dispatch('submit', e.detail);
							}}
						/>
					</div>

					<!-- Suggestions section - positioned closer to input -->
				<div class="w-full max-w-2xl" in:fade={{ duration: 200, delay: 300 }}>
						<Suggestions
						className="grid grid-cols-2 gap-3"
						suggestionPrompts={atSelectedModel?.info?.meta?.suggestion_prompts ??
							models[selectedModelIdx]?.info?.meta?.suggestion_prompts ??
							$config?.default_prompt_suggestions ??
							[]}
						{onSelect}
					/>
					</div>
				</div>
			{/if}
		</div>
	</div>
