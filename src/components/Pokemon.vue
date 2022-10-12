<template>
    <div class="background" v-if="this.items.length > 0">
        <div :class="[pageTurn ? 'turn-page' : '']"></div>
        <div class="card-wrapper">
            <a
                href="#"
                class="poke-card"
                v-for="(item, index) in pokeSlice"
                v-bind:key="index"
            >
                <h1 class="poke-name">{{ item.id }} - {{ item.name }}</h1>
                <div class="poke-img">
                    <img v-show="item.img" v-bind:src="item.img" />
                </div>
            </a>
        </div>

        <div class="nav-menu">
            <h1 @click="handlePrev">previous</h1>
            <div v-for="(item, index) in pagesSlice" :key="index">
                <h1
                    :class="[
                        'nav-pages',
                        checkPage(item) ? '' : 'nav-page-highlight',
                    ]"
                    @click="goToPage(item)"
                >
                    {{ item }}
                </h1>
            </div>
            <h1 @click="handleNext">next</h1>
        </div>
    </div>
    <div class="poke-loading" v-else>loading</div>
</template>

<script>
export default {
    name: "pokemonFetch",
    data() {
        return {
            allPoke: [],
            items: [],
            page: 1,
            maxPoke: false,
            pokeshow: 21,
            index: "",
            pageTurn: false,
        };
    },
    computed: {
        pokemin() {
            return this.page * this.pokeshow - this.pokeshow;
        },
        pokemax() {
            return this.page * this.pokeshow;
        },
        pokeSlice() {
            return this.items.slice(this.pokemin, this.pokemax);
        },
        maxPages() {
            return Math.ceil(this.index / this.pokeshow);
        },
        pages() {
            var element = [];
            for (let index = 1; index <= this.maxPages; index++) {
                element.push(index);
            }
            return element;
        },
        pagesSlice() {
            if (this.page < 4) {
                return this.pages.slice(0, 7);
            } else if (this.page >= 4 && this.page < this.maxPages - 3) {
                return this.pages.slice(this.page - 4, this.page + 3);
            } else {
                return this.pages.slice(this.maxPages - 7, this.maxPages);
            }
        },
    },
    watch: {},
    methods: {
        fetchData: async function (pokemin, pokemax) {
            // await new Promise((r) => setTimeout(r, 100));
            const pokeFetch = [];
            for (let index = pokemin; index < pokemax; index++) {
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
                    this.index = index;
                } catch {
                    break;
                }
            }

            this.items = pokeFetch;
        },
        goToPage: function (item) {
            if (this.pageTurn == true) {
                return;
            }
            this.pageTurn = true;
            setTimeout(() => {
                this.page = item;
            }, 200);
            setTimeout(() => {
                this.pageTurn = false;
            }, 1000);
        },
        checkPage: function (item) {
            if (this.page != item) {
                return true;
            }
            return false;
        },
        handleNext: function () {
            if (this.pokemax >= this.index || this.pageTurn == true) {
                return;
            }
            this.pageTurn = true;
            setTimeout(() => {
                this.page++;
            }, 200);
            setTimeout(() => {
                this.pageTurn = false;
            }, 1000);
        },
        handlePrev: function () {
            if (this.page == 1 || this.pageTurn == true) {
                return;
            }
            this.pageTurn = true;
            setTimeout(() => {
                this.page--;
            }, 200);
            setTimeout(() => {
                this.pageTurn = false;
            }, 1000);
        },
    },
    mounted() {
        this.fetchData(1, 2000);
    },
    updated() {},
};
</script>
