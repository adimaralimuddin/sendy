<script lang="ts">
	import ImgItem from './ImgItem.svelte';

	const imgCounts = new Array(28).fill(1);

	export let onBack: () => void;
</script>

<main class="bg-slate-200 absolute top-0 left-0 min-h-screen w-screen">
	<button on:click={onBack} class="p-3 text-lg font-medium fixed top-0 left-0 w-full bg-white z-40"
		>{'<-'} go back</button
	>
	<div class="grid grid-cols-2 flexd gap-1 flex-wrap justify-center">
		{#each imgCounts as img, ind}
			<!-- <img class="max-w-32d" src="/images/{ind + 1}.jpeg" alt="" /> -->
			<ImgItem ind={ind + 1} />
		{/each}
	</div>
</main>
