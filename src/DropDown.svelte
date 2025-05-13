<script lang="ts">
	import downArrow from '$lib/assets/down-arrow.png';
	import { text } from '@sveltejs/kit';
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
	class="mt-36 flex w-2xl cursor-pointer flex-col items-center justify-start"
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
		class="flex h-14 w-full cursor-pointer flex-row items-center justify-between rounded-lg border-2 px-4"
		onclick={() => (isOpen = !isOpen)}
		onkeydown={(e) => {
			if (e.code === 'Enter') {
				e.preventDefault();
			}
		}}
	>
		<p class:text-gray-500={selected === placeholder}>
			{selected}
		</p>
		<img {src} alt="down-arrow" class="h-6" />
	</button>
	<ul class="w-full">
		{#each options as option, i}
			{#if isOpen}
				<li
					class="first:rounded-t-lg last:rounded-b-lg last:shadow-md hover:bg-gray-100"
					in:fly={{ delay: i * 100, duration: 400, x: -100 }}
					out:fade
					class:bg-amber-200={option == selected}
					class:text-white={option == selected}
					class:bg-gray-100={option == selectedTmp && option != selected}
				>
					<button
						class="h-14 w-full cursor-pointer"
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
