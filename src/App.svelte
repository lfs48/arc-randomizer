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
  ;}
</style>

<main class='w-screen flex items-center bg-zinc-100 font-roboto'>
  <section class="bg-anomaly-blue text-white border-r-2 border-r-deep-purple">
    <div class="flex flex-col items-center space-y-2">
      <h2>ANOMALY</h2>
      <h1>{anomaly || '???'}</h1>
    </div>
    <div><!----></div>
  </section>
  <section class="bg-reality-yellow text-deep-purple border-r-2 border-deep-purple">
    <div class="flex flex-col items-center space-y-2">
      <h2>REALITY</h2>
      <h1>{reality || '???'}</h1>
    </div>
    <div><!----></div>
  </section>
  <section class="bg-agency-red text-white">
    <div class="flex flex-col items-center space-y-2">
      <h2>COMPETENCY</h2>
      <h1>{competency || '???'}</h1>
    </div>
    <div><!----></div>
  </section>
  <button 
    class="fixed bottom-1/3 w-screen h-20 flex justify-center items-center bg-deep-purple border-y-2 border-white text-white text-4xl p-4 leading-none font-bold cursor-pointer"
    on:click={generate}
  >
    RANDOMIZE
  </button>
</main>