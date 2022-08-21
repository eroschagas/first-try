<template>
    <div v-if="this.items.length > 0">
        <div class="card-wrapper">
            <a
                href="#"
                class="poke-card"
                v-for="(item, index) in itemsSlice"
                v-bind:key="index"
            >
                <h1 class="poke-name">{{ item.id }} - {{ item.name }}</h1>
                <div class="poke-img">
                    <img v-show="item.img" v-bind:src="item.img" />
                </div>
            </a>
        </div>

        <div class="nav-menu">
            <h1 v-on:click="handlePrev">previous</h1>
            <h1 v-on:click="handleNext">next</h1>
        </div>
    </div>
    <div v-else>loading</div>
</template>

<script>
export default {
    name: "tryingFetch",
    data: function () {
        return {
            allPoke: [],
            items: [],
            page: 1,
            maxPoke: false,
            pokeshow: 21,
        };
    },
    computed: {
        pokemin() {
            return this.page * this.pokeshow - this.pokeshow;
        },
        pokemax() {
            return this.page * 5 * this.pokeshow + 1;
        },
        itemsSlice() {
            return this.items.slice(this.pokemin, this.pokemin + this.pokeshow);
        },
    },
    watch: {
        items(newvalue) {
            this.items = newvalue;
        },
    },
    methods: {
        fetchData: async function () {
            // await new Promise((r) => setTimeout(r, 100));
            const pokeFetch = [];
            for (let index = 1; index < this.pokemax; index++) {
                try {
                    const response = await fetch(
                        `https://pokeapi.co/api/v2/pokemon/${index}`
                    );
                    const json = await response.json();
                    pokeFetch.push({
                        index: index,
                        id: json.id,
                        name: json.name,
                        img: json.sprites.other["official-artwork"]
                            .front_default,
                    });
                } catch {
                    this.maxPoke = true;
                    break;
                }
            }

            this.items = pokeFetch;
        },
        handleNext: function () {
            if (this.maxPoke >= true) {
                return;
            }
            this.page++;
            this.fetchData();
            console.log("file: Pokemon.vue ~ line 85 ~ this.items", this.items);
        },
        handlePrev: function () {
            if (this.page == 1) {
                return;
            }
            this.page--;
            this.maxPoke = false;
            this.fetchData();
        },
    },
    mounted() {
        this.fetchData();
        console.log(document.readyState);
    },
};
// const x = computed(() => {
//     return this.fetchData();
// });
</script>
