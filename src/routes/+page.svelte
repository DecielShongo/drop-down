<script>
	import DropDown from '../DropDown.svelte';
	let options = ['Option 1', 'Option 2', 'Option 3', 'Option 4'];
	let placeholder = 'Select option';
</script>

<main class="flex h-screen w-full flex-col items-center">
	<h1 class="mt-5 text-5xl font-semibold">Frontend Challenge #1</h1>
	<DropDown {placeholder} {options} />
</main>
