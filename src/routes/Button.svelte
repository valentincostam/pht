<script lang="ts">
	let className: string = '';

	export { className as class };
</script>

<button
	type="button"
	on:click
	class="{className} flex flex-nowrap items-center gap-1.5 border border-black px-3 py-1 text-left font-mono text-xs shadow-[2px_2px_0_0_black] active:translate-x-0.5 active:translate-y-0.5 active:shadow-none print:hidden"
>
	<slot />
</button>
