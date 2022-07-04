<template>
    <div id="app">
        <Menu />
        <img v-on:click="imgClick" alt="Vue logo" src="./assets/logo.png" />
        <HelloWorld v-bind:test="teste2" msg="oi mooooo!!!" />
    </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import Menu from "./components/Menu.vue";

export default {
    name: "App",
    components: {
        HelloWorld,
        Menu,
    },
    data: function () {
        return {
            teste2: true,
        };
    },
    methods: {
        imgClick: function () {
            this.teste2 = !this.teste2;
        },
    },
};
</script>
