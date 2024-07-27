<script lang="ts">
	import {
		format,
		eachDayOfInterval,
		startOfWeek,
		addYears,
		endOfWeek,
		eachMonthOfInterval,
		isSunday,
		getWeeksInMonth,
		differenceInWeeks,
		lastDayOfMonth,
		nextSunday,
		previousSaturday,
		parseISO,
		isSameMonth
	} from 'date-fns';
	import { es } from 'date-fns/locale';
	import Button from './Button.svelte';
	import { createEventDispatcher } from 'svelte';

	export let date: string;
	export let index: number;
	export let hideDayNumbers: boolean = false;
	export let hideHabitTitles: boolean = false;

	const dispatch = createEventDispatcher();

	$: parsedChosenDate = parseISO(date);

	$: startOfChosenWeek = startOfWeek(parsedChosenDate);

	$: endOfWeekOfOneYearLater = endOfWeek(addYears(startOfChosenWeek, 1));

	$: months = eachMonthOfInterval({ start: parsedChosenDate, end: endOfWeekOfOneYearLater }).map(
		(month, index, array) => {
			if (index === 0) {
				const areInSameMonth = isSameMonth(parsedChosenDate, startOfChosenWeek);
				const lastDayOfCurrentMonth = lastDayOfMonth(parsedChosenDate);
				const nextSundayAfterLastDayOfCurrentMonth = nextSunday(lastDayOfCurrentMonth);
				const differenceInWeeksToNextMont = differenceInWeeks(
					nextSundayAfterLastDayOfCurrentMonth,
					areInSameMonth ? startOfChosenWeek : parsedChosenDate
				);

				return {
					month,
					weeks: differenceInWeeksToNextMont,
					show: differenceInWeeksToNextMont > 2,
					padding: !areInSameMonth
				};
			}

			if (index === array.length - 1) {
				const differenceInWeeksToEndOfLastWeek = differenceInWeeks(
					endOfWeekOfOneYearLater,
					isSunday(month) ? previousSaturday(month) : month
				);

				return {
					month,
					weeks: differenceInWeeksToEndOfLastWeek,
					show: differenceInWeeksToEndOfLastWeek > 1,
					padding: false
				};
			}

			return {
				month,
				weeks: isSunday(month) ? getWeeksInMonth(month) : getWeeksInMonth(month) - 1,
				show: true,
				padding: false
			};
		}
	);

	$: days = eachDayOfInterval({ start: startOfChosenWeek, end: endOfWeekOfOneYearLater });

	const today = new Date();

	const daysOfTheWeek = eachDayOfInterval({ start: startOfWeek(today), end: endOfWeek(today) }).map(
		(day) => format(day, 'EEE', { locale: es })
	);
</script>

<div class="grid gap-2 px-4 print:break-inside-avoid print:px-0">
	<div class="gap-2 {hideHabitTitles ? 'hidden' : 'flex flex-col items-start md:flex-row-reverse'}">
		<Button on:click={() => dispatch('remove')} class="shrink-0">
			<svg viewBox="0 0 24 24" fill="currentColor" class="size-4 shrink-0">
				<path
					fill-rule="evenodd"
					d="M16.5 4.478v.227a48.816 48.816 0 0 1 3.878.512.75.75 0 1 1-.256 1.478l-.209-.035-1.005 13.07a3 3 0 0 1-2.991 2.77H8.084a3 3 0 0 1-2.991-2.77L4.087 6.66l-.209.035a.75.75 0 0 1-.256-1.478A48.567 48.567 0 0 1 7.5 4.705v-.227c0-1.564 1.213-2.9 2.816-2.951a52.662 52.662 0 0 1 3.369 0c1.603.051 2.815 1.387 2.815 2.951Zm-6.136-1.452a51.196 51.196 0 0 1 3.273 0C14.39 3.05 15 3.684 15 4.478v.113a49.488 49.488 0 0 0-6 0v-.113c0-.794.609-1.428 1.364-1.452Zm-.355 5.945a.75.75 0 1 0-1.5.058l.347 9a.75.75 0 1 0 1.499-.058l-.346-9Zm5.48.058a.75.75 0 1 0-1.498-.058l-.347 9a.75.75 0 0 0 1.5.058l.345-9Z"
					clip-rule="evenodd"
				/>
			</svg>

			Borrar hábito #{index + 1}
		</Button>

		<input
			type="text"
			placeholder="Título hábito #{index + 1}"
			class="grow font-mono text-xl print:placeholder-shown:invisible"
		/>
	</div>

	<div class="grid grid-cols-[repeat(54,1fr)] grid-rows-8 gap-0.5 font-mono uppercase">
		<div
			class="col-start-2 -col-end-1 row-span-1 grid grid-cols-subgrid grid-rows-subgrid items-end text-[1vw]"
		>
			{#each months as { month, weeks, show, padding }}
				<div class="leading-none" style="grid-column: {padding ? '2 / ' : ''} span {weeks}">
					{show ? format(month, 'MMM', { locale: es }) : ''}
				</div>
			{/each}
		</div>

		<div
			class="col-span-1 row-start-2 -row-end-1 grid grid-cols-subgrid grid-rows-subgrid items-center text-[1vw]"
		>
			{#each daysOfTheWeek as day, index}
				<div class="leading-none">
					{index % 2 !== 0 ? day : ''}
				</div>
			{/each}
		</div>

		<div
			class="col-start-2 -col-end-1 row-start-2 -row-end-1 grid grid-flow-col grid-cols-subgrid grid-rows-subgrid items-center"
		>
			{#each days as day}
				<div class="relative aspect-square overflow-hidden border border-gray-400">
					<div
						class="absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 text-[0.7vw] leading-none text-gray-400 {hideDayNumbers
							? 'hidden'
							: 'block'}"
					>
						{format(day, 'dd')}
					</div>
				</div>
			{/each}
		</div>
	</div>
</div>
