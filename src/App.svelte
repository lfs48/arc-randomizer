<script>
  import { RiLockLine, RiLockUnlockLine, RiSettings3Fill } from 'svelte-remixicon';

  const data = {
    anomaly: {
      base: [
        'Whisper',
        'Catalogue',
        'Drain',
        'Timepiece',
        'Growth',
        'Gun',
        'Dream',
        'Manifold',
        'Absence',
      ],
      amaranth: [
        'Storm',
        'Crown',
        'Ritual',
      ],
      pentachoron: [
        'Ascent',
      ],
    },
    reality: {
      base: [
        'Caretaker',
        'Overbooked',
        'Pursued',
        'Star',
        'Struggling',
        'Newborn',
        'Romantic',
        'Backbone',
        'Creature',
      ],
      amaranth: [
        'Doomed',
        'Seeker',
        'Elder',
      ],
      pentachoron: [
        'Obsessed',
      ],
    },
    competency: {
      base: [
        'PR',
        'R&D',
        'Barista',
        'CEO',
        'Intern',
        'Gravedigger',
        'Reception',
        'Hotline',
        'Clown',
      ],
      amaranth: [
        'Consultant',
        'Logistics',
        'Firefighter',
      ],
      pentachoron: [
        'Coach',
      ]
    },
  };

  const settings = $state({
    open: false,
    base: true,
    amaranth: false,
    pentachoron: false,
  });

  let selectedSources = $derived({
    anomaly: getSelectedSources('anomaly'),
    reality: getSelectedSources('reality'),
    competency: getSelectedSources('competency'),
  });

  function getSelectedSources(field) {
    const res = [];
    if (settings.base) { res.push(...data[field].base) }
    if (settings.amaranth) { res.push(...data[field].amaranth) }
    if (settings.pentachoron) { res.push(...data[field].pentachoron) }
    return res;
  }

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
    if (!arcs.anomaly.locked) { arcs.anomaly.value = getRandomElement( selectedSources['anomaly'] ) || '???' };
    if (!arcs.reality.locked) { arcs.reality.value = getRandomElement( selectedSources['reality'] ) || '???' };
    if (!arcs.competency.locked) { arcs.competency.value = getRandomElement( selectedSources['competency'] ) || '???' };
  }

  function toggleLock(field) {
    arcs[field].locked = !arcs[field].locked;
  }

  const buttonDisabled = $derived(
    !(settings.base || settings.amaranth || settings.pentachoron) || 
    (arcs.anomaly.locked && arcs.reality.locked && arcs.competency.locked)
  )
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
  button{
    @apply
    cursor-pointer
    disabled:cursor-default
  ;}
</style>

{#snippet arcSection(args)}
  <section class={`${args.class} text-deep-purple border-r-2 border-deep-purple`}>
    <button
      class="flex flex-col items-center space-y-2"
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

{#snippet checkbox(args)}
  <div class="flex items-center space-x-1">
    <input
      type="checkbox"
      class="w-4 h-4 cursor-pointer"
      bind:checked={settings[args.field]}
    />
    <label class="text-xs font-bold">{args.label}</label>
  </div>
{/snippet}

<main class='w-screen flex items-center bg-zinc-100 font-roboto'>
  {#if settings.open}
    <div class="fixed top-7 right-7 flex space-x-4 pl-3 py-3 pr-12 bg-zinc-100 rounded-full">
      {@render checkbox({field: 'base', label: 'Field Manual'})}
      {@render checkbox({field: 'amaranth', label: 'Amaranth Folder'})}
      {@render checkbox({field: 'pentachoron', label: 'Project Pentachoron'})}
    </div>
  {/if}
  <button
    class="fixed top-8 right-8 text-deep-purple"
    onclick={() => settings.open = !settings.open}
  >
    <RiSettings3Fill size='2rem'/>
  </button>
  {@render arcSection({field:'anomaly', class:'bg-anomaly-blue text-white'})}
  {@render arcSection({field:'reality', class:'bg-reality-yellow'})}
  {@render arcSection({field:'competency', class:'bg-agency-red text-white'})}
  <button 
    class="fixed bottom-1/2 w-screen h-20 flex justify-center items-center bg-deep-purple border-y-2 border-white text-white disabled:text-gray-400 text-4xl p-4 leading-none font-bold"
    onclick={generate}
    disabled={buttonDisabled}
  >
    RANDOMIZE
  </button>
</main>