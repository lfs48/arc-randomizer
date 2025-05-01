<script>
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

  let anomaly = null;
  let reality = null;
  let competency = null;

  function generate() {
    anomaly = getRandomElement(anomalies);
    reality = getRandomElement(realities);
    competency = getRandomElement(competencies);
  }
</script>



<style>
  @reference "./app.css";
  section {
    @apply
    basis-1/3
    h-full
    flex
    flex-col
    justify-center
    items-center
    space-y-4
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
  ;}
</style>

<main class='w-screen h-screen flex flex-col justify-evenly items-center py-16 bg-zinc-100 font-roboto'>
  <div class="w-[80rem] h-[20rem] flex border-2 border-deep-purple rounded-[7px]">
    <section class="bg-anomaly-blue text-white rounded-l border-r-2 border-r-deep-purple">
      <h2>ANOMALY</h2>
      <h1>{anomaly || '???'}</h1>
    </section>
    <section class="bg-reality-yellow text-deep-purple border-r-2 border-r-deep-purple">
      <h2>REALITY</h2>
      <h1>{reality || '???'}</h1>
    </section>
    <section class="bg-agency-red text-white rounded-r">
      <h2>COMPETENCY</h2>
      <h1>{competency || '???'}</h1>
    </section>
  </div>
  <button 
    class="flex items-center bg-deep-purple rounded text-white text-4xl p-4 leading-none font-bold cursor-pointer"
    on:click={generate}
  >
    RANDOMIZE
  </button>
</main>