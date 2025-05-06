<script lang="ts">
	let { placeholder, options } = $props();
	let selected = $state(placeholder);

	function selectOption(option: string) {
		selected = option;
	}
</script>

<button class="mt-36 flex h-12 w-3xl flex-row items-center justify-between rounded-md border-2">
	{selected}
</button>
<ul>
	{#each options as option}
		<li>
			<button
				class="cursor-pointer"
				onclick={() => {
					selectOption(option);
				}}
			>
				{option}
			</button>
		</li>
	{/each}
</ul>
