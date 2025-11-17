<script lang="ts">
  import type { MouseEventHandler } from "svelte/elements";
  import "../app.css";

  const mode = Object.freeze({
    DEV: "dev",
    PROD: "prod",
  });

  const currentMode = mode.PROD;

  let { dataForMS, name }: { dataForMS: any[]; name: string } = $props();

  let nameSelect: HTMLDivElement[] = $state([]);
  
  const nameCatg: Function = (arr: Array<string>): Array<string> => {
    let out: Array<string> = [];
    for (let i = 0; i < arr.length; i++) {
      for (let j = 0; j < nameSelect.length; j++) {
        if (nameSelect[j].id == arr[i])
          out.push(nameSelect[j].innerHTML.replace("↳", ""));
      }
    }
    return out;
  };

  const searchFunc: Function = (key: string) => {
    if (key != "") {
      nameSelect.forEach((elem) => {
        elem.className = "hidden";
        if (elem.innerHTML.toLowerCase().includes(key.toLowerCase())) elem.className = "block";
      });
    } else {
      nameSelect.forEach((elem) => (elem.className = "block"));
    }
  };

  const order: Function = (arr: Object[]): Object[] => {
    let temp: Object[] = [];
    arr.forEach((elem) => {
      if (elem.sub_id != null) {
        let find = arr.findIndex((obj) => obj.id == elem.sub_id);
        temp.splice(find + 1, 0, elem);
      } else {
        temp.push(elem);
      }
    });
    return temp;
  };

  const upload: Function = (form_data: FormData) => {
    form_data.append(name, JSON.stringify(select));
  };

  const devTest: MouseEventHandler<HTMLButtonElement> = () => {
    let formTest: FormData = new FormData();
    upload(formTest);
    console.log(formTest);
  };

  let select: Array<string | number> = $state([]);
  let selected: string[] = $derived(nameCatg(select));
  let search: string = $state("");

  const clickDiv: Function = (id: string, elem: HTMLDivElement) => {
    if (!select.find((elem) => elem == id)) {
      select.push(id);
      elem.classList.add("bg-blue-400");
    } else {
      select = select.filter((elem) => elem != id);
      elem.classList.remove("bg-blue-400");
    }
  };

</script>

<div class="flex flex-col space-y-2 w-full">
  
    <input
      bind:value={search}
      oninput={searchFunc(search)}
      class="text-black border-2 border-black border-solid bg-white rounded-md"
      type="text"
    />
    <div
      class="bg-white rounded-md text-black border-2 border-black border-solid"
    >
      {#each order(dataForMS) as data, i}
        <div
          bind:this={nameSelect[i]}
          class="block text-left w-full {data.sub_id != null
            ? 'pl-2'
            : ''} {select.find((elem) => elem == data.id) != undefined
            ? 'bg-blue-400'
            : ''}"
          id={data.id}
          role="option"
          aria-selected={select.find((elem) => elem == data.id) != undefined
            ? true
            : false}
          tabindex={i}
          onkeydown={(e) => {
            if (e.key == " ") clickDiv(data.id, nameSelect[i]);
          }}
          onclick={() => {
            clickDiv(data.id, nameSelect[i]);
          }}
        >
          {data.sub_id != null ? "↳" : ""}{data.name}
        </div>
      {/each}
    </div>
    <p class="text-black">{name} sélectioner: {selected}</p>
    {#if currentMode == mode.DEV}
      <button class="bg-orange-400 rounded-md w-fit p-2" onclick={devTest}
        >tester</button
      >
    {/if}
  
</div>
