<script lang="ts">
	let submit = false;
</script>

{#if submit}
	<div>
		<p class="text-lg animate-pulse text-blue-400">scanning...</p>
	</div>
{:else}
	<form
		on:submit={(e) => {
			e.preventDefault();
			submit = true;
			setTimeout(() => {
				submit = false;
				alert('Invalid token or No Data found!');
			}, 2000);
		}}
		class="flex flex-col gap-2 items-center"
	>
		<label for="">Or</label>
		<input
			required
			class="p-1 rounded-mdd ring-2 ring-black flex-1"
			type="text"
			placeholder="Enter chat token text-sm "
		/>
	</form>
{/if}
