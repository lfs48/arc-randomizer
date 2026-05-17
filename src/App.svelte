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
        'Judgment',
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
        'Daredevil'
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
        'Quant',
      ]
    },
  };

  function initializeSettings() {
    const stored = localStorage.getItem('settings');
    if (stored) {
      const parsed = JSON.parse(stored);
      // Migrate old settings format to new format
      if ('base' in parsed && !('selectedItems' in parsed)) {
        const newSettings = {
          open: parsed.open ?? true,
          selectedItems: {}
        };
        // Initialize all items based on old settings
        for (const field in data) {
          for (const tier in data[field]) {
            const isSelected = parsed[tier] ?? false;
            for (const item of data[field][tier]) {
              newSettings.selectedItems[item] = isSelected;
            }
          }
        }
        return newSettings;
      }
      return parsed;
    }
    // Initialize new format with all base items selected
    const defaultSettings = {
      open: true,
      selectedItems: {}
    };
    for (const field in data) {
      for (const tier in data[field]) {
        for (const item of data[field][tier]) {
          defaultSettings.selectedItems[item] = tier === 'base';
        }
      }
    }
    return defaultSettings;
  }

  const settings = $state(initializeSettings());

  $effect(() => {
    localStorage.setItem('settings', JSON.stringify(settings));
  });

  let selectedSources = $derived({
    anomaly: getSelectedSources('anomaly'),
    reality: getSelectedSources('reality'),
    competency: getSelectedSources('competency'),
  });

  function getSelectedSources(field) {
    const res = [];
    for (const tier of ['base', 'amaranth', 'pentachoron']) {
      for (const item of data[field][tier]) {
        if (settings.selectedItems[item]) {
          res.push(item);
        }
      }
    }
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

  let loading = $state(false);

  function generate() {
    loading = true;
    const interval = setInterval(() => {
      if (!arcs.anomaly.locked) { arcs.anomaly.value = getRandomElement( selectedSources['anomaly'] ) || '???' };
      if (!arcs.reality.locked) { arcs.reality.value = getRandomElement( selectedSources['reality'] ) || '???' };
      if (!arcs.competency.locked) { arcs.competency.value = getRandomElement( selectedSources['competency'] ) || '???' };
    }, 75);
    setTimeout(() => {
      loading = false;
      clearInterval(interval);
    }, 975);
  }

  function toggleLock(field) {
    arcs[field].locked = !arcs[field].locked;
  }

  function getTierItems(tier) {
    return ['anomaly', 'reality', 'competency'].flatMap((field) => data[field][tier]);
  }

  function isTierFullySelected(tier) {
    return getTierItems(tier).every((item) => settings.selectedItems[item]);
  }

  function toggleTierSelection(tier) {
    const shouldSelect = !isTierFullySelected(tier);
    for (const item of getTierItems(tier)) {
      settings.selectedItems[item] = shouldSelect;
    }
  }

  const noSourceSelected = $derived(Object.values(settings.selectedItems).every(v => !v));
  const allColsLocked = $derived(arcs.anomaly.locked && arcs.reality.locked && arcs.competency.locked);
  const buttonDisabled = $derived(loading || noSourceSelected || allColsLocked);

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
    text-lg
    lg:text-4xl
    font-bold
  ;}
  h2 {
    @apply
    lg:text-2xl
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
      <h1 class={(loading && !arcs[args.field].locked) && 'opacity-50'}>{arcs[args.field].value}</h1>
    </div>
  </section>
{/snippet}

{#snippet itemCheckbox(args)}
  <div class="flex items-center space-x-1">
    <input
      type="checkbox"
      class="w-4 h-4 cursor-pointer"
      id={args.item}
      bind:checked={settings.selectedItems[args.item]}
    />
    <label class="text-[0.5rem] lg:text-xs font-bold cursor-pointer" for={args.item}>{args.item}</label>
  </div>
{/snippet}

<main class='w-screen flex items-center bg-zinc-100 font-roboto'>
  {#if settings.open}
    <div class="fixed z-20 top-7 right-7 max-h-[80vh] overflow-y-auto flex flex-col space-y-3 pl-3 py-3 pr-4 bg-zinc-100 rounded-lg shadow-lg">
      {#each ['base', 'amaranth', 'pentachoron'] as tier}
        <div class="border-b pb-2 mb-1">
          <h3 class="text-xs lg:text-sm font-bold uppercase mb-2">
            {tier === 'base' ? 'Field Manual' : tier === 'amaranth' ? 'Amaranth Folder' : 'Project Pentachoron'}
          </h3>
          <div class="flex flex-col space-y-2 ml-2">
            {#each ['anomaly', 'reality', 'competency'] as field}
              <div class="flex flex-col space-y-1">
                <div class={`text-[0.5rem] lg:text-xs font-semibold uppercase ${field === 'anomaly' ? 'text-anomaly-blue' : field === 'reality' ? 'text-reality-yellow' : 'text-agency-red'}`}>{field}</div>
                <div class="flex flex-col space-y-1 ml-2">
                  {#each data[field][tier] as item}
                    {@render itemCheckbox({item})}
                  {/each}
                </div>
              </div>
            {/each}
          </div>
          <div class="flex justify-end mt-2">
            <button
              class="text-[0.5rem] lg:text-xs font-bold uppercase text-deep-purple"
              onclick={() => toggleTierSelection(tier)}
            >
              {isTierFullySelected(tier) ? 'Deselect all' : 'Select all'}
            </button>
          </div>
        </div>
      {/each}
    </div>
  {/if}
  <button
    class="fixed z-30 top-8 right-12 text-deep-purple"
    onclick={() => settings.open = !settings.open}
  >
    <RiSettings3Fill size='2rem'/>
  </button>
  {@render arcSection({field:'anomaly', class:'bg-anomaly-blue text-white'})}
  {@render arcSection({field:'reality', class:'bg-reality-yellow'})}
  {@render arcSection({field:'competency', class:'bg-agency-red text-white'})}
  <button 
    class="fixed bottom-1/2 w-screen h-20 flex justify-center items-center bg-deep-purple border-y-2 border-white text-white text-4xl p-4 leading-none font-bold"
    onclick={generate}
    disabled={buttonDisabled}
  >
    <span class={buttonDisabled && 'opacity-50'}>
      {loading ? 'RANDOMIZING...' : 'RANDOMIZE' }
    </span>
  </button>
</main>