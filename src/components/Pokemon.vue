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
            n: 0,
            maxPoke: false,
        };
    },
    methods: {
        fetchData: async function () {
            // await new Promise((r) => setTimeout(r, 100));
            const pokeFetch = [];
            for (let index = this.n + 1; index < this.n + 22; index++) {
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
            let that = this;
            if (this.maxPoke >= true) {
                return;
            }
            this.n = this.n + 21;

            setTimeout(function () {
                that.fetchData();
            }, 100);
        },
        handlePrev: function () {
            let that = this;
            if (this.n == 0) {
                return;
            }
            this.n = this.n - 21;
            this.maxPoke = false;
            setTimeout(function () {
                that.fetchData();
            }, 500);
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
