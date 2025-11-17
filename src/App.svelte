<script lang="ts">
  import MultiSelect from "./lib/MultiSelect.svelte";
  import DB from "./assets/catg.json";
  import DBLuxe from "./assets/luxe.json";

  let checked = $state(false)
  $inspect(checked)
</script>

<main class="flex flex-col justify-center items-center h-screen bg-zinc-200">
  <div class="p-2">
    <label for="" class="">
      <input bind:checked={checked} type="checkbox" class="" />
      <div class="w-20 h-8 {checked ? "bg-green-400" : "bg-slate-500"}"></div>
    </label>
  </div>
  <div class="p-2 shadow-2xl rounded-xl bg-white">
    <MultiSelect dataForMS={DB.data} name="Réalisateur" />
  </div>
</main>
