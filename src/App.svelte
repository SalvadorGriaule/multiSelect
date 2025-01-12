<script lang="ts">
    import type { MouseEventHandler } from "svelte/elements";
    import "./app.css";
    import { onMount } from "svelte";

    const mode = Object.freeze({
        DEV: "dev",
        PROD: "prod",
    });

    interface ProductCatg {
        id:number,
    }

    const currentMode = mode.PROD;

    let nameSelect: HTMLButtonElement[] = $state([]);
    let promise: Promise<void> = $state(new Promise(() => {}));
    let result: Object = $state({});

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
        form_data.append("categorie", JSON.stringify(select));
    };

    const devTest: MouseEventHandler<HTMLButtonElement> = () => {
        let formTest: FormData = new FormData();
        upload(formTest);
        console.log(formTest);
    };

    let select: Array<string> = $state([]);
    let selected: Array<string> = $derived(nameCatg(select));
    let search: string = $state("");
    let data: string | null | undefined;

    const clickDiv: Function = (id: string,elem:HTMLButtonElement) => {
        if(!select.find((elem) => elem == id)){
            select.push(id)
            elem.classList.add("bg-blue-400");
        } else {
            select = select.filter((elem) => elem != id);
            elem.classList.remove("bg-blue-400");
        }
    };

    onMount(() => {
        promise = (async () => {
            let resp = await fetch(
                "http://127.0.0.1:8000/api/?format=json",
            ).then((res) => res.json());
            result = resp;
        })();

        data = document
            .getElementById("multiSelect")
            ?.getAttribute("data-catg");
        if (data) {
            let test:ProductCatg[] = JSON.parse(data)
            test.forEach((elem) => {select.push(String(elem.id))})
        }

        document.addEventListener("formdata", (e) => {
            if (select.length > 0) upload(e.formData);
        });

        console.log(select,selected);
        
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
        <div
            class="bg-white rounded-md text-black border-2 border-black border-solid"
        >
            {#each order(result.data) as data, i}
                <button
                    bind:this={nameSelect[i]}
                    class="block text-left w-full {data.category_id != null ? 'pl-2' : ''} { select.find((elem) => elem == data.id) != undefined ? 'bg-blue-400' : "" }"
                    id={data.id}
                    onclick={() => {clickDiv(data.id,nameSelect[i])}}
                >
                    {data.category_id != null ? "↳" : ""}{data.name}
                </button>
            {/each}
        </div>
        <p class="text-black">Catégories sélectioner: {selected}</p>
        {#if currentMode == mode.DEV}
            <button class="bg-orange-400 rounded-md w-fit p-2" onclick={devTest}
                >tester</button
            >
        {/if}
    {/await}
</div>
