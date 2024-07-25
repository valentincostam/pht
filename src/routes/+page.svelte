<script lang="ts">
  import HabitTracker from './HabitTracker.svelte';

  let habitTrackerIds = [crypto.randomUUID()]

  const today = new Date();

  let chosenDate = (today).toISOString().split('T')[0];

  let hideDayNumbers = false;
  let hideHabitNames = false;

  function addHabitTracker() {
    habitTrackerIds = [...habitTrackerIds, crypto.randomUUID()];
  }

  function removeHabitTracker(habitTrackerId: string) {
    habitTrackerIds = habitTrackerIds.filter(id => id !== habitTrackerId);
  }
</script>

<div class="inputs">
    <label for="date">
      Fecha de inicio:
      <input id="date" type="date" bind:value={chosenDate}>
    </label>
    <button type="button" on:click={addHabitTracker}>Agregar hábito</button>
    <button type="button" on:click={() => hideDayNumbers = !hideDayNumbers}>{hideDayNumbers ? "Mostrar" : "Ocultar"} números de los días</button>
    <button type="button" on:click={() => hideHabitNames = !hideHabitNames}>{hideHabitNames ? "Mostrar" : "Ocultar"} nombres de los hábitos</button>
    <button type="button" on:click={() => print()}>Imprimir</button>
</div>

<div class="container">
  {#each habitTrackerIds as habitTrackerId, index (habitTrackerId)}
    <HabitTracker date={chosenDate} {index} {hideDayNumbers} {hideHabitNames} on:click={() => removeHabitTracker(habitTrackerId)}/>
  {/each}
</div>

<style>
  .inputs {
    font-family: monospace;
    padding: 3rch;
    display: flex;
    gap: 0.5rem;
    align-items: center;
    flex-wrap: wrap;

    @media print {
      display: none;
    }
  }

  .container {
    display: grid;
    gap: 1rem;
  }
</style>