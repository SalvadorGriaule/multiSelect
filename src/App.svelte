<script lang="ts">
    import { onMount } from "svelte";
    import "../../../../../../public/build/assets/app-ms0tsOM0.css";
    let nameSelect: HTMLOptionElement[] = $state([]);
    let promise: Promise<void> = $state(new Promise(() => {}));
    let result: Object = $state({});

    const nameCatg: Function = (arr: Array<string>): Array<string> => {
        let out: Array<string> = [];
        for (let i = 0; i < arr.length; i++) {
            for (let j = 0; j < nameSelect.length; j++) {
                if (nameSelect[j].value == arr[i])
                    out.push(nameSelect[j].innerHTML.replace("↳", ""));
            }
        }
        return out;
    };

    const searchFunc: Function = (key: string) => {
        if (key != "") {
            nameSelect.forEach((elem) => {
                elem.className = "hidden";
                if (elem.innerHTML.includes(key)) elem.className = "block";
            });
        } else {
            nameSelect.forEach((elem) => (elem.className = "block"));
        }
    };

    const order: Function = (arr: Object[]): Object[] => {
        let temp: Object[] = [];
        arr.forEach((elem) => {
            if (elem.category_id != null) {
                let find = arr.findIndex((obj) => obj.id == elem.category_id);
                temp.splice(find + 1, 0, elem);
            } else {
                temp.push(elem);
            }
        });
        return temp;
    };

    const upload: Function = (form_data: FormData) => {
        form_data.append("categorie", select.toString());
    };

    let select: Array<string> = $state([]);
    let selected: Array<string> = $derived(nameCatg(select));
    let search: string = $state("");
    let data:string|null|undefined;
    onMount(() => {
        promise = (async () => {
            let resp = await fetch(
                "http://127.0.0.1:8000/api/?format=json",
            ).then((res) => res.json());
            result = resp;
        })();

        data = document.getElementById("app2")?.getAttribute("data-catg");
        if(data) {
            selected.push(data);
        };     

        document.addEventListener("formdata",(e) => {
            if(select.length > 0) upload(e.formData);
        })
    });
</script>

<div class="flex flex-col space-y-2 w-full">
    {#await promise then catg}
        <input
            bind:value={search}
            oninput={searchFunc(search)}
            class="text-black border-2 border-black border-solid bg-white rounded-md"
            type="text"
        />
        <select
            class="bg-white rounded-md text-black border-2 border-black border-solid"
            bind:value={select}
            name="category_id"
            id=""
            multiple
        >
            {#each order(result.data) as data, i}
                <option
                    class="block {data.category_id != null ? 'pl-2' : ''}"
                    bind:this={nameSelect[i]}
                    value={data.id}
                    >{data.category_id != null ? "↳" : ""}{data.name}</option
                >
            {/each}
        </select>
        <p class="text-black">Catégories sélectioner: {selected}</p>
    {/await}
</div>
