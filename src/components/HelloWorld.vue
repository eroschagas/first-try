<template>
    <div>
        <h1 v-on:click="handleClick">hide</h1>
        <div v-if="test || seen" class="hello">
            <h1>{{ msg }}</h1>
            <h2>{{ message }}</h2>
            <h3>{{ test }}</h3>
            <p>To aprendendo a usar o vue, olha só! Que legal!</p>
        </div>
    </div>
</template>

<script>
export default {
    name: "HelloWorld",
    props: {
        msg: String,
        test: Boolean,
    },
    data: function () {
        return {
            seen: true,
            message: "opa",
        };
    },
    methods: {
        handleClick: function () {
            this.seen = !this.seen;
        },
    },
};
</script>
