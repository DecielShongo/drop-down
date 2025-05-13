<script lang="ts">
	import downArrow from '$lib/assets/down-arrow.png';

	let { placeholder, options } = $props();
	let selected = $state(placeholder);
	let selectedTmp = $state(placeholder);
	let index = $state(options.length);
	let src = downArrow;
	let isOpen = $state(false);

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
			console.log(selected);
			selectOption(selectedTmp);
		}
	}
</script>

<div
	class="mt-36 flex cursor-pointer flex-col items-center justify-start"
	role="listbox"
	tabindex="0"
	onkeydown={(e) => handleKeyDown(e)}
	onfocusout={(e) => {
		if (!e.relatedTarget || !(e.currentTarget as Node).contains(e.relatedTarget as Node)) {
			isOpen = false;
		}
	}}
>
	<button
		class="flex h-12 w-3xl cursor-pointer flex-row items-center justify-between rounded-md border-2"
		onfocus={() => (isOpen = true)}
	>
		<p>
			{selected}
		</p>
		<img {src} alt="down-arrow" class="h-10" />
	</button>
	<ul class:hidden={!isOpen}>
		{#each options as option}
			<li>
				<button
					class="cursor-pointer"
					class:border-2={option == selectedTmp}
					onclick={() => {
						selectOption(option);
						index = options.indexOf(option);
					}}
					onkeydown={(e) => {
						if (e.code === 'Enter') {
							e.preventDefault();
						}
					}}
					onfocus={() => (isOpen = true)}
				>
					{option}
				</button>
			</li>
		{/each}
	</ul>
</div>
