<script lang="ts">
	import { onMount, getContext } from 'svelte';
	import { toast } from 'svelte-sonner';
	import { uploadFile, getFiles, deleteFileById } from '$lib/apis/files';
	import { WEBUI_API_BASE_URL } from '$lib/constants';
	import { user, config } from '$lib/stores';
	import { getFileType } from '$lib/utils';
	import Tooltip from '$lib/components/common/Tooltip.svelte';
	import Modal from '$lib/components/common/Modal.svelte';
	import ArrowDownTray from '$lib/components/icons/ArrowDownTray.svelte';
	import DocumentArrowDown from '$lib/components/icons/DocumentArrowDown.svelte';
	import LockClosed from '$lib/components/icons/LockClosed.svelte';
	import GlobeAltSolid from '$lib/components/icons/GlobeAltSolid.svelte';
	import FolderOpen from '$lib/components/icons/FolderOpen.svelte';

	const i18n = getContext('i18n');

	let files = [];
	let loading = false;
	let uploading = false;
	let showUploadModal = false;
	let selectedFiles = [];
	let visibility = 'private'; // 'private' or 'public'
	let filesInputElement;
	let filterType = 'all'; // 'all', 'public', 'private'

	const loadFiles = async () => {
		loading = true;
		try {
			const response = await getFiles(localStorage.token);
			files = response || [];
		} catch (error) {
			console.error('Error loading files:', error);
			toast.error($i18n.t('Failed to load files'));
		}
		loading = false;
	};

	const handleFileUpload = async () => {
		if (!selectedFiles || selectedFiles.length === 0) {
			toast.error($i18n.t('Please select files to upload'));
			return;
		}

		uploading = true;
		try {
			for (const file of selectedFiles) {
				const metadata = {
					visibility: visibility,
					uploadedAt: Date.now()
				};

				const access_control = {
					read: visibility === 'public' ? {} : { user_id: $user.id }
				};

				const uploadedFile = await uploadFile(localStorage.token, file, metadata, access_control);
				if (uploadedFile) {
					toast.success($i18n.t('File uploaded successfully: {{filename}}', { filename: file.name }));
				} else {
					toast.error($i18n.t('Failed to upload file: {{filename}}', { filename: file.name }));
				}
			}
			await loadFiles();
			showUploadModal = false;
			selectedFiles = [];
			visibility = 'private';
		} catch (error) {
			console.error('Error uploading files:', error);
			toast.error($i18n.t('Failed to upload files'));
		}
		uploading = false;
	};

	const handleFileSelect = (event) => {
		selectedFiles = Array.from(event.target.files);
	};

	const downloadFile = async (file) => {
		try {
			const url = `${WEBUI_API_BASE_URL}/files/${file.id}/content?attachment=true`;
			const link = document.createElement('a');
			link.href = url;
			link.download = file.filename;
			link.click();
		} catch (error) {
			console.error('Error downloading file:', error);
			toast.error($i18n.t('Failed to download file'));
		}
	};

	const previewFile = (file) => {
		try {
			const url = `${WEBUI_API_BASE_URL}/files/${file.id}/content`;
			window.open(url, '_blank');
		} catch (error) {
			console.error('Error previewing file:', error);
			toast.error($i18n.t('Failed to preview file'));
		}
	};

	const deleteFile = async (file) => {
		if (confirm($i18n.t('Are you sure you want to delete this file?'))) {
			try {
				await deleteFileById(localStorage.token, file.id);
				toast.success($i18n.t('File deleted successfully'));
				await loadFiles();
			} catch (error) {
				console.error('Error deleting file:', error);
				toast.error($i18n.t('Failed to delete file'));
			}
		}
	};

	const getFileIcon = (filename) => {
		const extension = filename.split('.').pop()?.toLowerCase();
		switch (extension) {
			case 'pdf':
				return '📄';
			case 'doc':
			case 'docx':
				return '📝';
			case 'xls':
			case 'xlsx':
				return '📊';
			case 'ppt':
			case 'pptx':
				return '📈';
			case 'jpg':
			case 'jpeg':
			case 'png':
			case 'gif':
				return '🖼️';
			case 'mp3':
			case 'wav':
			case 'mp4':
			case 'avi':
				return '🎵';
			case 'txt':
				return '📄';
			default:
				return '📁';
		}
	};

	const formatFileSize = (bytes) => {
		if (bytes === 0) return '0 Bytes';
		const k = 1024;
		const sizes = ['Bytes', 'KB', 'MB', 'GB'];
		const i = Math.floor(Math.log(bytes) / Math.log(k));
		return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
	};

	// Filter files based on selected filter type
	$: filteredFiles = files.filter(file => {
		if (filterType === 'all') return true;
		if (filterType === 'public') {
			return !file.access_control || !file.access_control.read || Object.keys(file.access_control.read).length === 0;
		}
		if (filterType === 'private') {
			return file.access_control && file.access_control.read && Object.keys(file.access_control.read).length > 0;
		}
		return true;
	});

	const getVisibilityIcon = (file) => {
		const fileVisibility = file.meta?.visibility || 'private';
		return fileVisibility === 'public' ? GlobeAltSolid : LockClosed;
	};

	const getVisibilityText = (file) => {
		const fileVisibility = file.meta?.visibility || 'private';
		return fileVisibility === 'public' ? $i18n.t('Public') : $i18n.t('Private');
	};

	onMount(() => {
		loadFiles();
	});
</script>

<div class="flex flex-col h-full">
	<!-- Header -->
	<div class="flex items-center justify-between p-4 border-b border-gray-200 dark:border-gray-700">
		<div class="flex items-center space-x-2">
			<FolderOpen className="size-5" />
			<h1 class="text-xl font-semibold">{$i18n.t('Library')}</h1>
		</div>
		<div class="flex items-center space-x-4">
			<!-- Filter buttons -->
			<div class="flex bg-gray-100 dark:bg-gray-700 rounded-lg p-1">
				<button
					on:click={() => filterType = 'all'}
					class="px-3 py-1 text-sm rounded-md transition-colors {filterType === 'all' ? 'bg-white dark:bg-gray-600 text-gray-900 dark:text-white shadow-sm' : 'text-gray-600 dark:text-gray-300 hover:text-gray-900 dark:hover:text-white'}"
				>
					الكل
				</button>
				<button
					on:click={() => filterType = 'public'}
					class="px-3 py-1 text-sm rounded-md transition-colors {filterType === 'public' ? 'bg-white dark:bg-gray-600 text-gray-900 dark:text-white shadow-sm' : 'text-gray-600 dark:text-gray-300 hover:text-gray-900 dark:hover:text-white'}"
				>
					عام
				</button>
				<button
					on:click={() => filterType = 'private'}
					class="px-3 py-1 text-sm rounded-md transition-colors {filterType === 'private' ? 'bg-white dark:bg-gray-600 text-gray-900 dark:text-white shadow-sm' : 'text-gray-600 dark:text-gray-300 hover:text-gray-900 dark:hover:text-white'}"
				>
					خاص
				</button>
			</div>
			<button
				class="px-4 py-2 bg-blue-600 hover:bg-blue-700 text-white rounded-lg transition-colors"
				on:click={() => (showUploadModal = true)}
			>
				{$i18n.t('Upload Files')}
			</button>
		</div>
	</div>

	<!-- Content -->
	<div class="flex-1 p-4 overflow-auto">
		{#if loading}
			<div class="flex items-center justify-center h-64">
				<div class="animate-spin rounded-full h-8 w-8 border-b-2 border-blue-600"></div>
			</div>
		{:else if filteredFiles.length === 0}
			<div class="flex flex-col items-center justify-center h-64 text-gray-500">
				<FolderOpen className="size-16 mb-4" />
				{#if filterType === 'all'}
					<p class="text-lg">{$i18n.t('No files uploaded yet')}</p>
					<p class="text-sm">{$i18n.t('Upload your first file to get started')}</p>
				{:else if filterType === 'public'}
					<p class="text-lg">لا توجد ملفات عامة</p>
					<p class="text-sm">جرب تغيير الفلتر أو رفع ملفات جديدة</p>
				{:else}
					<p class="text-lg">لا توجد ملفات خاصة</p>
					<p class="text-sm">جرب تغيير الفلتر أو رفع ملفات جديدة</p>
				{/if}
			</div>
		{:else}
			<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
				{#each filteredFiles as file (file.id)}
					<div class="bg-white dark:bg-gray-800 rounded-lg border border-gray-200 dark:border-gray-700 p-4 hover:shadow-md transition-shadow">
						<!-- File Icon and Name -->
						<div class="flex items-start space-x-3 mb-3">
							<div class="text-2xl">{getFileIcon(file.filename)}</div>
							<div class="flex-1 min-w-0">
								<h3 class="text-sm font-medium text-gray-900 dark:text-white truncate" title={file.filename}>
									{file.filename}
								</h3>
								<p class="text-xs text-gray-500 dark:text-gray-400">
									{formatFileSize(file.meta?.size || 0)} • {file.meta?.content_type || 'Unknown'}
									{#if file.access_control && file.access_control.read && Object.keys(file.access_control.read).length > 0}
										• <span class="text-orange-600 dark:text-orange-400">خاص</span>
									{:else}
										• <span class="text-green-600 dark:text-green-400">عام</span>
									{/if}
								</p>
							</div>
						</div>

						<!-- Visibility Badge -->
						<div class="flex items-center space-x-1 mb-3">
							<svelte:component this={getVisibilityIcon(file)} className="size-3" />
							<span class="text-xs text-gray-600 dark:text-gray-400">
								{getVisibilityText(file)}
							</span>
						</div>

						<!-- Actions -->
						<div class="flex space-x-2">
							<Tooltip content={$i18n.t('Preview')}>
								<button
									class="flex-1 px-2 py-1 text-xs bg-gray-100 hover:bg-gray-200 dark:bg-gray-700 dark:hover:bg-gray-600 rounded transition-colors"
									on:click={() => previewFile(file)}
								>
									{$i18n.t('Preview')}
								</button>
							</Tooltip>
							<Tooltip content={$i18n.t('Download')}>
								<button
									class="flex-1 px-2 py-1 text-xs bg-blue-100 hover:bg-blue-200 dark:bg-blue-900 dark:hover:bg-blue-800 text-blue-700 dark:text-blue-300 rounded transition-colors"
									on:click={() => downloadFile(file)}
								>
									<ArrowDownTray className="size-3 inline mr-1" />
									{$i18n.t('Download')}
								</button>
							</Tooltip>
							{#if file.user_id === $user?.id}
								<Tooltip content={$i18n.t('Delete')}>
									<button
										class="px-2 py-1 text-xs bg-red-100 hover:bg-red-200 dark:bg-red-900 dark:hover:bg-red-800 text-red-700 dark:text-red-300 rounded transition-colors"
										on:click={() => deleteFile(file)}
									>
										{$i18n.t('Delete')}
									</button>
								</Tooltip>
							{/if}
						</div>
					</div>
				{/each}
			</div>
		{/if}
	</div>
</div>

<!-- Upload Modal -->
<Modal bind:show={showUploadModal}>
	<div class="p-6">
		<h2 class="text-lg font-semibold mb-4">{$i18n.t('Upload Files')}</h2>
		
		<!-- File Input -->
		<div class="mb-4">
			<label class="block text-sm font-medium mb-2">{$i18n.t('Select Files')}</label>
			<input
				bind:this={filesInputElement}
				type="file"
				multiple
				class="w-full px-3 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 dark:bg-gray-700"
				on:change={handleFileSelect}
			/>
			{#if selectedFiles.length > 0}
				<p class="text-sm text-gray-600 dark:text-gray-400 mt-2">
					{$i18n.t('Selected files: {{count}}', { count: selectedFiles.length })}
				</p>
			{/if}
		</div>

		<!-- Visibility Setting -->
		<div class="mb-6">
			<label class="block text-sm font-medium mb-2">{$i18n.t('Visibility')}</label>
			<div class="flex space-x-4">
				<label class="flex items-center">
					<input
						type="radio"
						bind:group={visibility}
						value="private"
						class="mr-2"
					/>
					<LockClosed className="size-4 mr-1" />
					{$i18n.t('Private')}
				</label>
				<label class="flex items-center">
					<input
						type="radio"
						bind:group={visibility}
						value="public"
						class="mr-2"
					/>
					<GlobeAltSolid className="size-4 mr-1" />
					{$i18n.t('Public')}
				</label>
			</div>
			<p class="text-xs text-gray-500 dark:text-gray-400 mt-1">
				{visibility === 'private' 
					? $i18n.t('Only you can see these files')
					: $i18n.t('All users can see these files')
				}
			</p>
		</div>

		<!-- Actions -->
		<div class="flex justify-end space-x-3">
			<button
				class="px-4 py-2 text-gray-600 dark:text-gray-400 hover:text-gray-800 dark:hover:text-gray-200 transition-colors"
				on:click={() => {
					showUploadModal = false;
					selectedFiles = [];
					visibility = 'private';
				}}
				disabled={uploading}
			>
				{$i18n.t('Cancel')}
			</button>
			<button
				class="px-4 py-2 bg-blue-600 hover:bg-blue-700 text-white rounded-lg transition-colors disabled:opacity-50 disabled:cursor-not-allowed"
				on:click={handleFileUpload}
				disabled={uploading || selectedFiles.length === 0}
			>
				{#if uploading}
					<div class="flex items-center space-x-2">
						<div class="animate-spin rounded-full h-4 w-4 border-b-2 border-white"></div>
						<span>{$i18n.t('Uploading...')}</span>
					</div>
				{:else}
					{$i18n.t('Upload')}
				{/if}
			</button>
		</div>
	</div>
</Modal>