<script lang="ts">
	import { format, eachDayOfInterval, startOfWeek, addYears, endOfWeek, eachMonthOfInterval, isSunday, getWeeksInMonth, differenceInWeeks, lastDayOfMonth, nextSunday, previousSaturday, parseISO, isSameMonth } from 'date-fns';
  import { es } from "date-fns/locale";

  export let date: string;
  export let index: number;
  export let hideDayNumbers: boolean = false;
  export let hideHabitNames: boolean = false;

  $: parsedChosenDate = parseISO(date);
  
  $: startOfChosenWeek = startOfWeek(parsedChosenDate);
  
	$: endOfWeekOfOneYearLater = endOfWeek(addYears(startOfChosenWeek, 1));
  
  $: months = eachMonthOfInterval({start: parsedChosenDate, end: endOfWeekOfOneYearLater}).map((month, index, array) => {
    if (index === 0) {
        const areInSameMonth = isSameMonth(parsedChosenDate, startOfChosenWeek)
        const lastDayOfCurrentMonth = lastDayOfMonth(parsedChosenDate);
        const nextSundayAfterLastDayOfCurrentMonth = nextSunday(lastDayOfCurrentMonth); 
        const differenceInWeeksToNextMont = differenceInWeeks(nextSundayAfterLastDayOfCurrentMonth, areInSameMonth ? startOfChosenWeek : parsedChosenDate);

        return {
          month,
          weeks: differenceInWeeksToNextMont,
          show: differenceInWeeksToNextMont > 2,
          padding: !areInSameMonth
        }
    }

    if (index === array.length - 1) {
      const differenceInWeeksToEndOfLastWeek = differenceInWeeks(endOfWeekOfOneYearLater, isSunday(month) ? previousSaturday(month) : month);

      return {
        month,
        weeks: differenceInWeeksToEndOfLastWeek,
        show: differenceInWeeksToEndOfLastWeek > 1,
        padding: false
      }
    }

    return {
      month,
      weeks: isSunday(month) ? getWeeksInMonth(month) : getWeeksInMonth(month) - 1,
      show: true,
      padding: false
    }
  });
  
	$: days = eachDayOfInterval({ start: startOfChosenWeek, end: endOfWeekOfOneYearLater});

  const today = new Date();

  const daysOfTheWeek = eachDayOfInterval({ start: startOfWeek(today), end: endOfWeek(today) }).map(day => format(day, 'EEE', {locale: es}));

</script>


<div class="container">
  <div class="input-container" style="display: {hideHabitNames ? 'none' : 'flex'}">
    <input type="text" placeholder="Hábito #{index + 1}">
    <button type="button" on:click>Borrar hábito #{index + 1}</button>
  </div>
  
  <div class="grid">
    <div class="months">
      {#each months as {month, weeks, show, padding}}
        <div style="grid-column: {padding ? '2 / ' : ''} span {weeks}">
          {show ? format(month, 'MMM', {locale: es}) : ""}
        </div>
      {/each}
    </div>
  
    <div class="day-names">
      {#each daysOfTheWeek as day, index}
        <div class="day-name">
          {index % 2 !== 0 ? day : ""}
        </div>
      {/each}
    </div>
  
    <div class="days">
      {#each days as day}
        <div class="day-square">
          <div class="day-number" style="display: {hideDayNumbers ? 'none' : 'block'}">{format(day, 'dd')}</div>
        </div>
      {/each}
    </div>
  </div>
</div>




<style>
  .container {
    page-break-inside: avoid;
  }

  .input-container {
    align-items: center;
    gap: 1rem;
    padding: 0.5rem 3rch;
  }

  input[type="text"] {
    font-family: monospace;
    font-size: 1.2rem;
    box-sizing: border-box;
    display: block;
    flex-grow: 1;

    &:not(:placeholder-shown) {
      border: 0;
    }

    @media print {
      &:placeholder-shown {
        visibility: hidden;
      }
    }
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(54, 1fr);
    gap: 3px;
    width: 100%;
    page-break-inside: avoid;
  }

  .months {
    display: grid;
    grid-template-columns: subgrid;
    grid-column: 2 / -1;
    font-size: 11px;
    text-transform: uppercase;
    font-family: monospace;
  }

  .day-names {
    display: grid;
    grid-template-columns: subgrid;
    grid-column: span 1;
    grid-template-rows: repeat(7, minmax(0, 1fr));
    grid-auto-flow: column;
  }

  .days {
    display: grid;
    justify-self: start;
    grid-template-columns: subgrid;
    grid-column: 2 / -1;
    grid-template-rows: repeat(7, minmax(0, 1fr));
    grid-auto-flow: column;
    gap: 3px;
    font-family: sans-serif;
  }

  .day-name {
    display: flex;
    align-items: center;
    justify-content: end;
    font-size: 11px;
    text-transform: uppercase;
    font-family: monospace;
  }

	.day-square {
		border: 1px solid #000;
		display: flex;
		align-items: center;
		justify-content: center;
    color: #ddd;
    font-size: 1vw;
    aspect-ratio: 1;
    font-family: monospace;
    position: relative;
    overflow: hidden;
	}

  .day-number {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
</style>