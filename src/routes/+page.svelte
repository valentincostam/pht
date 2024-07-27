<script lang="ts">
	import Button from './Button.svelte';
	import HabitTracker from './HabitTracker.svelte';

	let habitTrackerIds = [crypto.randomUUID()];

	const today = new Date();

	let chosenDate = today.toISOString().split('T')[0];

	let hideDayNumbers = false;

	let hideHabitTitles = false;

	function addHabitTracker() {
		habitTrackerIds = [...habitTrackerIds, crypto.randomUUID()];
	}

	function removeHabitTracker(habitTrackerId: string) {
		habitTrackerIds = habitTrackerIds.filter((id) => id !== habitTrackerId);
	}
</script>

<div class="flex flex-col gap-2 px-4 py-8 md:flex-row md:flex-wrap print:hidden">
	<label for="date" class="flex items-center gap-2 font-mono text-xs">
		Fecha de inicio:
		<input
			id="date"
			type="date"
			class="flex-grow border border-black px-3 py-1 shadow-[inset_2px_2px_0_0_black]"
			bind:value={chosenDate}
		/>
	</label>

	<Button on:click={addHabitTracker}>
		<svg viewBox="0 0 24 24" fill="currentColor" class="size-4 shrink-0">
			<path
				fill-rule="evenodd"
				d="M12 2.25c-5.385 0-9.75 4.365-9.75 9.75s4.365 9.75 9.75 9.75 9.75-4.365 9.75-9.75S17.385 2.25 12 2.25ZM12.75 9a.75.75 0 0 0-1.5 0v2.25H9a.75.75 0 0 0 0 1.5h2.25V15a.75.75 0 0 0 1.5 0v-2.25H15a.75.75 0 0 0 0-1.5h-2.25V9Z"
				clip-rule="evenodd"
			/>
		</svg>

		Agregar hábito
	</Button>

	<Button on:click={() => print()}>
		<svg viewBox="0 0 24 24" fill="currentColor" class="size-4 shrink-0">
			<path
				fill-rule="evenodd"
				d="M7.875 1.5C6.839 1.5 6 2.34 6 3.375v2.99c-.426.053-.851.11-1.274.174-1.454.218-2.476 1.483-2.476 2.917v6.294a3 3 0 0 0 3 3h.27l-.155 1.705A1.875 1.875 0 0 0 7.232 22.5h9.536a1.875 1.875 0 0 0 1.867-2.045l-.155-1.705h.27a3 3 0 0 0 3-3V9.456c0-1.434-1.022-2.7-2.476-2.917A48.716 48.716 0 0 0 18 6.366V3.375c0-1.036-.84-1.875-1.875-1.875h-8.25ZM16.5 6.205v-2.83A.375.375 0 0 0 16.125 3h-8.25a.375.375 0 0 0-.375.375v2.83a49.353 49.353 0 0 1 9 0Zm-.217 8.265c.178.018.317.16.333.337l.526 5.784a.375.375 0 0 1-.374.409H7.232a.375.375 0 0 1-.374-.409l.526-5.784a.373.373 0 0 1 .333-.337 41.741 41.741 0 0 1 8.566 0Zm.967-3.97a.75.75 0 0 1 .75-.75h.008a.75.75 0 0 1 .75.75v.008a.75.75 0 0 1-.75.75H18a.75.75 0 0 1-.75-.75V10.5ZM15 9.75a.75.75 0 0 0-.75.75v.008c0 .414.336.75.75.75h.008a.75.75 0 0 0 .75-.75V10.5a.75.75 0 0 0-.75-.75H15Z"
				clip-rule="evenodd"
			/>
		</svg>

		Imprimir
	</Button>

	<Button on:click={() => (hideDayNumbers = !hideDayNumbers)}>
		{#if hideDayNumbers}
			<svg viewBox="0 0 24 24" fill="currentColor" class="size-4 shrink-0">
				<path d="M12 15a3 3 0 1 0 0-6 3 3 0 0 0 0 6Z" />
				<path
					fill-rule="evenodd"
					d="M1.323 11.447C2.811 6.976 7.028 3.75 12.001 3.75c4.97 0 9.185 3.223 10.675 7.69.12.362.12.752 0 1.113-1.487 4.471-5.705 7.697-10.677 7.697-4.97 0-9.186-3.223-10.675-7.69a1.762 1.762 0 0 1 0-1.113ZM17.25 12a5.25 5.25 0 1 1-10.5 0 5.25 5.25 0 0 1 10.5 0Z"
					clip-rule="evenodd"
				/>
			</svg>
		{:else}
			<svg viewBox="0 0 24 24" fill="currentColor" class="size-4 shrink-0">
				<path
					d="M3.53 2.47a.75.75 0 0 0-1.06 1.06l18 18a.75.75 0 1 0 1.06-1.06l-18-18ZM22.676 12.553a11.249 11.249 0 0 1-2.631 4.31l-3.099-3.099a5.25 5.25 0 0 0-6.71-6.71L7.759 4.577a11.217 11.217 0 0 1 4.242-.827c4.97 0 9.185 3.223 10.675 7.69.12.362.12.752 0 1.113Z"
				/>
				<path
					d="M15.75 12c0 .18-.013.357-.037.53l-4.244-4.243A3.75 3.75 0 0 1 15.75 12ZM12.53 15.713l-4.243-4.244a3.75 3.75 0 0 0 4.244 4.243Z"
				/>
				<path
					d="M6.75 12c0-.619.107-1.213.304-1.764l-3.1-3.1a11.25 11.25 0 0 0-2.63 4.31c-.12.362-.12.752 0 1.114 1.489 4.467 5.704 7.69 10.675 7.69 1.5 0 2.933-.294 4.242-.827l-2.477-2.477A5.25 5.25 0 0 1 6.75 12Z"
				/>
			</svg>
		{/if}

		{hideDayNumbers ? 'Mostrar' : 'Ocultar'} números de los días
	</Button>

	<Button on:click={() => (hideHabitTitles = !hideHabitTitles)}>
		{#if hideHabitTitles}
			<svg viewBox="0 0 24 24" fill="currentColor" class="size-4 shrink-0">
				<path d="M12 15a3 3 0 1 0 0-6 3 3 0 0 0 0 6Z" />
				<path
					fill-rule="evenodd"
					d="M1.323 11.447C2.811 6.976 7.028 3.75 12.001 3.75c4.97 0 9.185 3.223 10.675 7.69.12.362.12.752 0 1.113-1.487 4.471-5.705 7.697-10.677 7.697-4.97 0-9.186-3.223-10.675-7.69a1.762 1.762 0 0 1 0-1.113ZM17.25 12a5.25 5.25 0 1 1-10.5 0 5.25 5.25 0 0 1 10.5 0Z"
					clip-rule="evenodd"
				/>
			</svg>
		{:else}
			<svg viewBox="0 0 24 24" fill="currentColor" class="size-4 shrink-0">
				<path
					d="M3.53 2.47a.75.75 0 0 0-1.06 1.06l18 18a.75.75 0 1 0 1.06-1.06l-18-18ZM22.676 12.553a11.249 11.249 0 0 1-2.631 4.31l-3.099-3.099a5.25 5.25 0 0 0-6.71-6.71L7.759 4.577a11.217 11.217 0 0 1 4.242-.827c4.97 0 9.185 3.223 10.675 7.69.12.362.12.752 0 1.113Z"
				/>
				<path
					d="M15.75 12c0 .18-.013.357-.037.53l-4.244-4.243A3.75 3.75 0 0 1 15.75 12ZM12.53 15.713l-4.243-4.244a3.75 3.75 0 0 0 4.244 4.243Z"
				/>
				<path
					d="M6.75 12c0-.619.107-1.213.304-1.764l-3.1-3.1a11.25 11.25 0 0 0-2.63 4.31c-.12.362-.12.752 0 1.114 1.489 4.467 5.704 7.69 10.675 7.69 1.5 0 2.933-.294 4.242-.827l-2.477-2.477A5.25 5.25 0 0 1 6.75 12Z"
				/>
			</svg>
		{/if}

		{hideHabitTitles ? 'Mostrar' : 'Ocultar'} nombres de los hábitos
	</Button>
</div>

<div class="grid gap-6">
	{#each habitTrackerIds as habitTrackerId, index (habitTrackerId)}
		<HabitTracker
			date={chosenDate}
			{index}
			{hideDayNumbers}
			{hideHabitTitles}
			on:remove={() => removeHabitTracker(habitTrackerId)}
		/>
	{/each}
</div>
