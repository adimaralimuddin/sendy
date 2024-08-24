<script lang="ts">
	import { onMount } from 'svelte';
	export let onAbort: () => void;
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
		let maxAbortion = 150;
		status = 'aborting';
		let interval = setInterval(() => {
			// console.log('progAbort = ', progAbort);

			progAbort += 1;
			if (progAbort > maxAbortion) {
				onAbort();
				clearInterval(interval);
			} else if (progAbort >= 100 && progAbort <= maxAbortion) {
				status = 'aborted';
			}
		}, 25);
	};
</script>

<main class="flex gap-6 flex-col items-center justify-center min-h-screen">
	{#if status === 'beingSent'}
		<p class="animate-pulse text-2xl text-green-600 font-bold">Photos are being sent...</p>
	{:else if status === 'sending'}
		<p class=" text-2xl text-green-600 font-bold">Sending Photos ... {prog} %</p>
	{:else if status === 'aborting'}
		<p class=" text-2xl text-orange-600 font-bold">Aborting ... {progAbort} %</p>
	{:else if status === 'aborted'}
		<p class=" text-2xl text-green-600 font-bold">Progress Aborted</p>
	{/if}

	{#if !status.includes('abort')}
		<button on:click={aborting} class="text-xl font-bold bg-red-400 text-white p-3 px-6"
			>abort</button
		>
	{/if}
</main>
