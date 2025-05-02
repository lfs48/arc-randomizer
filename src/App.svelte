<script>
  import { RiLockLine, RiLockUnlockLine } from 'svelte-remixicon';

  const anomalies = [
    'Whisper',
    'Catalogue',
    'Drain',
    'Timepiece',
    'Growth',
    'Gun',
    'Dream',
    'Manifold',
    'Absence',
  ];

  const realities = [
    'Caretaker',
    'Overbooked',
    'Pursued',
    'Star',
    'Struggling',
    'Newborn',
    'Romantic',
    'Backbone',
    'Creature',
  ];

  const competencies = [
    'PR',
    'R&D',
    'Barista',
    'CEO',
    'Intern',
    'Gravedigger',
    'Reception',
    'Hotline',
    'Clown',
  ];

  function getRandomElement(arr) {
    return arr[Math.floor(Math.random() * arr.length)];
  }

  const arcs = $state({
    anomaly: {
      value: '???',
      locked: false,
    },
    reality: {
      value: '???',
      locked: false,
    },
    competency: {
      value: '???',
      locked: false,
    },
  });

  function generate() {
    if (!arcs.anomaly.locked) { arcs.anomaly.value = getRandomElement(anomalies) };
    if (!arcs.reality.locked) { arcs.reality.value = getRandomElement(realities) };
    if (!arcs.competency.locked) { arcs.competency.value = getRandomElement(competencies) };
  }

  function toggleLock(field) {
    arcs[field].locked = !arcs[field].locked;
  }
</script>

<style>
  @reference "./app.css";
  section {
    @apply
    basis-1/3
    h-screen
    flex
    flex-col
    justify-evenly
    items-center
  ;}

  h1 {
    @apply
    text-4xl
    font-bold
  ;}
  h2 {
    @apply
    text-2xl
    font-bold
    uppercase
  ;}
</style>

{#snippet arcSection(args)}
  <section class={`${args.class} text-deep-purple border-r-2 border-deep-purple`}>
    <button
      class="flex flex-col items-center space-y-2 cursor-pointer"
      onclick={() => toggleLock(args.field)}
    >
      {#if arcs[args.field].locked}
        <RiLockLine size='2rem'/>
      {:else}
        <RiLockUnlockLine size='2rem'/>
      {/if}
      <h2>{args.field}</h2>
    </button>
    <div><!----></div>
    <div><!----></div>
    <div class="flex flex-col items-center space-y-2">
      <h1>{arcs[args.field].value}</h1>
    </div>
  </section>
{/snippet}

<main class='w-screen flex items-center bg-zinc-100 font-roboto'>
  {@render arcSection({field:'anomaly', class:'bg-anomaly-blue'})}
  {@render arcSection({field:'reality', class:'bg-reality-yellow'})}
  {@render arcSection({field:'competency', class:'bg-agency-red'})}
  <button 
    class="fixed bottom-1/2 w-screen h-20 flex justify-center items-center bg-deep-purple border-y-2 border-white text-white text-4xl p-4 leading-none font-bold cursor-pointer"
    onclick={generate}
  >
    RANDOMIZE
  </button>
</main>