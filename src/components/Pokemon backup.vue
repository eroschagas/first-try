<template>
    <div>
        <div class="card-wrapper">
            <div
                class="poke-card"
                v-for="(poke, index) in info.results"
                :key="index"
            >
                <h1 class="poke-name">{{ poke.name }}</h1>
                <div class="poke-img">
                    <img v-bind:src="info2[index]" />
                </div>
            </div>
        </div>

        <div class="nav-menu">
            <h1 v-on:click="handlePrev">previous</h1>
            <h1 v-on:click="handleNext">next</h1>
        </div>
    </div>
</template>

<script>
export default {
    name: "tryingFetch",
    data: function () {
        return {
            url: "",
            info: [],
            info2: [],
        };
    },
    methods: {
        fetchData: async function () {
            // await new Promise((r) => setTimeout(r, 100));
            const response = await fetch(this.url);
            const json = await response.json();
            this.info = json;
        },
        handleNext() {
            this.url = this.info.next;
            this.pokeImage();
        },
        handlePrev() {
            if (this.info.previous != null) {
                this.url = this.info.previous;
                this.pokeImage();
            }
        },
        pokeImage: async function () {
            await new Promise((r) => setTimeout(r, 500));
            this.info2 = [];
            for (let index = 0; index < 21; index++) {
                const fetchData2 = async () => {
                    const response = await fetch(this.info.results[index].url);
                    const json = await response.json();
                    const pokeImg =
                        json.sprites.other.dream_world.front_default;
                    this.info2.push(pokeImg);
                };
                await fetchData2();
            }
        },
    },
    mounted() {
        this.url = "https://pokeapi.co/api/v2/pokemon/?limit=21&offset=0";
        this.fetchData();
        this.pokeImage();
    },
    updated() {
        this.fetchData();
        // this.pokeImage();
    },
};
</script>
