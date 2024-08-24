<script>
	import Photos from './Photos.svelte';
	import Sending from './Sending.svelte';
	let showPhotos = false;
	let isSending = false;

	const onSend = () => {
		isSending = true;
	};

	const onAbort = () => {
		isSending = false;
	};
</script>

{#if isSending}
	<Sending {onAbort} />
{:else}
	<h1 class="text-center text-xl p-6 font-bold text-indigo-500">Social Cracker</h1>
	<main class="flex text-xl flex-col gap-3 justify-center items-center min-h-screen">
		<button on:click={() => (showPhotos = true)} class="underline text-blue-600"
			>see all photos</button
		>
		<button on:click={onSend} class="btn">send to 100</button>
		<button on:click={onSend} class="btn">send to 200</button>
		<button on:click={onSend} class="btn">send to 500</button>
		<button on:click={onSend} class="btn">send to everyone</button>
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
