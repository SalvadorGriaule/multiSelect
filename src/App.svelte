<script lang="ts">
  import MultiSelect from "./lib/MultiSelect.svelte";
  import DB from "./assets/catg.json";
  import DBLuxe from "./assets/luxe.json";

  let switchTog = $state(false);
  let currentDB = $state(DB.data)

  const onclick = () => {
    switchTog = !switchTog;
    currentDB = switchTog ? DB.data : DBLuxe.data
  };
</script>

<main class="flex flex-col justify-center items-center h-screen bg-zinc-200">
  <div class="p-2">
    <button
      {onclick}
      aria-label="switch"
      class="{switchTog
        ? 'bg-green-500'
        : 'bg-slate-500'} flex items-center p-2 h-6 w-20 rounded-xl duration-150"
    >
      <div
        class="bg-slate-200 rounded-full w-4 h-4 duration-150 {switchTog
          ? 'ml-[75%]'
          : 'ml-0'}"
      ></div>
    </button>
  </div>
  <div class="p-2 shadow-2xl rounded-xl bg-white">
    <MultiSelect dataForMS={currentDB} name="Réalisateur" />
  </div>
</main>
