<script lang="ts">
	export let ind: number;
	let maximize = false;

	const toggle = () => {
		maximize = !maximize;
		console.log('maxi');
	};
</script>

<img class="w-full" on:click={toggle} src="/images/{ind}.jpeg" alt="" />

{#if maximize}
	<div
		on:click={toggle}
		class="w-fulld bg-black fixed top-0 left-0 w-screen h-screen z-50 flex flex-col items-center justify-center"
	>
		<img class="w-screen" src="/images/{ind}.jpeg" alt="" />
	</div>
{/if}
