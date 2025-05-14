<script lang="ts">
	import downArrow from '$lib/assets/down-arrow.png';
	import { fly, fade } from 'svelte/transition';

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
			isOpen = true;
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
			isOpen = !isOpen;
		}
	}
</script>

<div
	class="mt-36 flex w-xl cursor-pointer flex-col items-center justify-start"
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
		class="flex h-14 w-full cursor-pointer flex-row items-center justify-between rounded-xl border-1 px-6 transition-all hover:shadow-md focus:border-orange-300 focus:outline-2 focus:outline-orange-300"
		onclick={() => (isOpen = !isOpen)}
		onkeydown={(e) => {
			if (e.code === 'Enter') {
				e.preventDefault();
			}
		}}
	>
		<p class:text-gray-500={selected === placeholder} class="text-lg">
			{selected}
		</p>
		<img {src} alt="down-arrow" class="h-6 transition-all duration-300" class:rotate-180={isOpen} />
	</button>
	<ul class="mt-6 w-full rounded-xl">
		{#each options as option, i}
			{#if isOpen}
				<li
					class="transition-colors first:rounded-t-xl last:rounded-b-xl last:shadow-md"
					in:fly={{ delay: i * 100, duration: 300, x: -100 }}
					out:fade
					class:bg-orange-300={option == selected}
					class:text-white={option == selected}
					class:bg-gray-100={option == selectedTmp && option != selected}
					onmouseenter={() => {
						selectedTmp = option;
						index = options.indexOf(option);
					}}
					onmouseleave={() => {
						selectedTmp = selected;
						index = options.indexOf(selected);
					}}
				>
					<button
						class="h-14 w-full cursor-pointer px-6 text-left text-lg font-light"
						onclick={() => {
							selectOption(option);
							index = options.indexOf(option);
							isOpen = false;
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
			{/if}
		{/each}
	</ul>
</div>
