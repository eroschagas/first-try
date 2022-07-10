<template>
    <div v-if="this.items.length > 0">
        <div class="card-wrapper">
            <a
                href="#"
                class="poke-card"
                v-for="(item, index) in items"
                v-bind:key="index"
            >
                <h1 class="poke-name">{{ item.id }} - {{ item.name }}</h1>
                <div class="poke-img">
                    <img v-if="item.img" v-bind:src="item.img" />
                    <h1 v-else>loading</h1>
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
// import { computed } from "vue";
export default {
    name: "tryingFetch",
    data: function () {
        return {
            allPoke: [],
            items: [],
            n: 0,
            maxPoke: 10000,
        };
    },
    methods: {
        fetchData: async function () {
            const pokeFetch = [];
            for (let index = 1; index < this.maxPoke; index++) {
                try {
                    // await new Promise((r) => setTimeout(r, 1000));
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
                    break;
                }
            }

            this.allPoke = pokeFetch;
            this.items = this.allPoke.slice(0 + this.n, 21 + this.n);
            console.log(this.items);
        },
        handleNext: function () {
            if (this.n >= this.maxPoke - 21) {
                return;
            }
            this.n = this.n + 21;
            this.items = this.allPoke.slice(0 + this.n, 21 + this.n);
        },
        handlePrev: function () {
            if (this.n == 0) {
                return;
            }
            this.n = this.n - 21;
            this.items = this.allPoke.slice(0 + this.n, 21 + this.n);
        },
    },
    mounted() {
        this.fetchData();
    },
};
// const x = computed(() => {
//     return this.fetchData();
// });
</script>
