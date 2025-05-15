<script lang="ts">
	import downArrow from '$lib/assets/down-arrow.png';
	import { fly, fade } from 'svelte/transition';

	let { placeholder, options } = $props();
	let selected = $state(placeholder);
	let highlighted = $state(placeholder);
	let index = $state(options.length);
	let src = downArrow;
	let isOpen = $state(false);

	function selectOption(option: string) {
		selected = option;
		highlighted = option;
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
				highlighted = options[options.length + index];
			} else {
				highlighted = options[index];
			}
		} else if (e.code == 'Enter') {
			selectOption(highlighted);
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

	{#if isOpen}
		<ul class="mt-6 w-full rounded-xl shadow-md" in:fly={{ duration: 500, y: -20 }} out:fade>
			{#each options as option, i}
				<li
					class="transition-colors first:rounded-t-xl last:rounded-b-xl"
					in:fly|global={{ delay: i * 100, duration: 300, x: -50 }}
					out:fade|global
					class:bg-orange-300={option == selected}
					class:text-white={option == selected}
					class:bg-gray-100={option == highlighted && option != selected}
					onmouseenter={() => {
						highlighted = option;
						index = options.indexOf(option);
					}}
					onmouseleave={() => {
						highlighted = selected;
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
			{/each}
		</ul>
	{/if}
</div>
