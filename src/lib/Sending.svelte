<script lang="ts">
	import { onMount } from 'svelte';
	export let onAbort: () => void;
	export let type: 'sending' | 'scanning';
	export let onSuccess: () => void;

	let prog = 0;
	let progAbort = 0;

	let status: 'sending' | 'beingSent' | 'aborting' | 'aborted' = 'sending';

	// let interval: typeof setInterval | undefined = undefined;

	onMount(() => {
		prog = 0;
		progAbort = 0;
		let interval = setInterval(() => {
			prog += 1;
			// console.log('prog', prog);

			if (prog >= 100) {
				if (status == 'sending') {
					status = 'beingSent';
					console.log('fuck');
				}
				clearInterval(interval);
			}
		}, 30);

		return () => {
			clearInterval(interval);
		};
	});

	const aborting = () => {
		progAbort = 0;
		let maxAbortion = 100;
		status = 'aborting';
		let interval = setInterval(() => {
			progAbort += 2;
			if (progAbort > maxAbortion) {
				onAbort();
				clearInterval(interval);
			} else if (progAbort >= 100 && progAbort <= maxAbortion) {
				status = 'aborted';
			}
		}, 25);
	};

	const onReturn = () => {
		progAbort = 0;
		status = 'beingSent';
		onAbort();
		onSuccess();
	};

	const abortText = () => (type == 'sending' ? 'Abort' : 'Cancel');
</script>

<main class="flex gap-6 flex-col items-center justify-center min-h-screen">
	{#if status === 'beingSent'}
		<p class=" text-2xl text-green-600 font-bold">
			{#if type == 'sending'}
				<span class="animate-pulse">Photos are being sent...</span>
			{:else}
				<span>already existing local domain found.</span>
			{/if}
		</p>
		<button on:click={onReturn} class="underline text-md p-2 px-3 font-bold text-blue-600"
			>Return</button
		>
	{:else if status === 'sending'}
		<p class="animate-pulse text-2xl text-green-600 font-bold">
			{type == 'sending' ? 'Sending Photos' : 'Scanning local domain'} ... {prog} %
		</p>
		<button on:click={aborting} class="text-xl font-bold bg-red-400 text-white p-3 px-6"
			>{type == 'sending' ? 'abort' : 'cancel'}</button
		>
	{:else if status === 'aborting'}
		<p class=" text-2xl text-orange-600 font-bold">{abortText()} ... {progAbort} %</p>
	{:else if status === 'aborted'}
		<p class=" text-2xl text-green-600 font-bold">
			{type == 'sending' ? 'Progress Aborted' : 'Scan Canceled'}
		</p>
	{/if}

	<!-- {#if !status.includes('abort')}
		<button on:click={aborting} class="text-xl font-bold bg-red-400 text-white p-3 px-6"
			>{type == 'sending' ? 'abort' : 'cancel'}</button
		>
	{/if} -->
</main>
