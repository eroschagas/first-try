<template>
    <div>
        <div v-for="cat in info.categories" v-bind:key="cat.idCategory">
            <h1>{{ cat.strCategory }}</h1>
            <img
                v-bind:src="cat.strCategoryThumb"
                v-bind:alt="cat.strCategory"
            />
        </div>
    </div>
</template>

<script>
export default {
    name: "tryingFetch",
    data: function () {
        return {
            info: "null",
        };
    },
    mounted() {
        const fetchData = async () => {
            const response = await fetch(
                "https://www.themealdb.com/api/json/v1/1/categories.php"
            );
            const json = await response.json();
            this.info = json;
        };
        fetchData();
    },
};
</script>
