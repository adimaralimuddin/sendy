<script lang="ts">
	import EnterCt from './EnterCt.svelte';
	import Photos from './Photos.svelte';
	import Sending from './Sending.svelte';
	let showPhotos = false;
	let isSending = false;
	let type: 'sending' | 'scanning' = 'sending';
	let hasResult = false;
	const onSend = () => {
		type = 'sending';
		isSending = true;
	};
	const onAbort = () => {
		isSending = false;
	};
</script>

{#if isSending}
	<Sending
		{type}
		{onAbort}
		onSuccess={() => {
			hasResult = true;
		}}
	/>
{:else}
	<!-- <h1 class="text-center text-xl p-6 font-bold text-indigo-500">Social Cracker</h1> -->
	<main class="flex text-xl flex-col gap-3 justify-center items-center min-h-screen">
		{#if hasResult}
			<button on:click={() => (showPhotos = true)} class="underline text-blue-600"
				>see all photos</button
			>
			<button on:click={onSend} class="btn">send to 100</button>
			<button on:click={onSend} class="btn">send to 200</button>
			<button on:click={onSend} class="btn">send to 500</button>
			<button on:click={onSend} class="btn">send to everyone</button>
		{/if}
		<button
			on:click={() => {
				type = 'scanning';
				isSending = true;
			}}
			class="bg-green-400 p-2 px-3 text-lg font-bold"
		>
			Scan Local Area</button
		>
		<EnterCt />
	</main>
{/if}
{#if showPhotos}
	<Photos
		onBack={() => {
			showPhotos = false;
		}}
	/>
{/if}

<style>
	.btn {
		@apply px-3 p-2 text-white bg-blue-600;
	}
</style>
