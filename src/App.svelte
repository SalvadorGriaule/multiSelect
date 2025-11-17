<script lang="ts">
    import MultiSelect from "./lib/MultiSelect.svelte";
    import DB from "./assets/catg.json"
    console.log(DB.data);
    
</script>

<main class="flex justify-center items-center h-screen">
    <div>
        <MultiSelect dataForMS={DB.data} name="Réalisateur" />
    </div>
</main>