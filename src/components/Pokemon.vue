<template>
    <div>
        <div class="card-wrapper">
            <div
                class="poke-card"
                v-for="(item, index) in items"
                v-bind:key="index"
            >
                <h1 class="poke-name">{{ item.name }}</h1>
                <div class="poke-img">
                    <img v-bind:src="item.img" />
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
            items: [{ name: "a" }, { name: "b" }],
            ready: false,
        };
    },
    methods: {
        fetchData: async function () {
            for (let index = 1; index < 21; index++) {
                // await new Promise((r) => setTimeout(r, 1000));
                const response = await fetch(
                    `https://pokeapi.co/api/v2/pokemon/${index}`
                );
                const json = await response.json();
                this.items.push({
                    index: index,
                    name: json.name,
                    img: json.sprites.front_default,
                });
            }
            console.log(this.items);
        },
    },
    mounted() {
        this.fetchData();
        this.ready = true;
    },
};
</script>
