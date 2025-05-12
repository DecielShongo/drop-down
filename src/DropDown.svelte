<script lang="ts">
	let { placeholder, options } = $props();
	let selected = $state(placeholder);
	let selectedTmp = $state(placeholder);
	let index = $state(options.length);

	function selectOption(option: string) {
		selected = option;
		selectedTmp = option;
	}
	function handleKeyDown(e: KeyboardEvent) {
		if (e.code == 'ArrowDown' || e.code == 'ArrowUp') {
			if (index != options.length || e.code == 'ArrowUp') {
				e.code == 'ArrowDown' && (index = (index + 1) % options.length);
				e.code == 'ArrowUp' && (index = (index - 1) % options.length);
			} else {
				index = 0;
			}
			if (index < 0) {
				selectedTmp = options[options.length + index];
			} else {
				selectedTmp = options[index];
			}
		} else if (e.code == 'Enter') {
			console.log('Enter');
			selectOption(selectedTmp);
		}
	}
</script>

<button
	class="mt-36 flex h-12 w-3xl flex-row items-center justify-between rounded-md border-2"
	onkeydown={(e) => handleKeyDown(e)}
>
	{selected}
</button>
<ul>
	{#each options as option}
		<li>
			<button
				class="cursor-pointer"
				class:border-2={option == selectedTmp}
				onclick={() => {
					selectOption(option);
					index = options.indexOf(option);
				}}
				onkeydown={(e) => handleKeyDown(e)}
			>
				{option}
			</button>
		</li>
	{/each}
</ul>
