<script lang="ts">
	let { placeholder, options } = $props();
	let selected = $state(placeholder);
	let index = $state(options.length);

	function selectOption(option: string) {
		selected = option;
	}
	function handleArrowKeys(e: KeyboardEvent) {
		if (index != options.length || e.code == 'ArrowUp') {
			e.code == 'ArrowDown' && (index = (index + 1) % options.length);
			e.code == 'ArrowUp' && (index = (index - 1) % options.length);
		} else {
			index = 0;
		}
		if (index < 0) {
			selected = options[options.length + index];
		} else {
			selected = options[index];
		}
	}
</script>

<button
	class="mt-36 flex h-12 w-3xl flex-row items-center justify-between rounded-md border-2"
	onkeydown={(e) => (e.code == 'ArrowDown' || e.code == 'ArrowUp') && handleArrowKeys(e)}
>
	{selected}
</button>
<ul>
	{#each options as option}
		<li>
			<button
				class="cursor-pointer"
				onclick={() => {
					selectOption(option);
					index = options.indexOf(option);
				}}
			>
				{option}
			</button>
		</li>
	{/each}
</ul>
