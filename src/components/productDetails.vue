<template>
    <ul>
        <li v-for="detail in details" :key="detail.id">{{ detail }}</li>
    </ul>
</template>

<script>
export default {
    name: "productDetails",
    props: {
        details: {
            type: Array,
            required: true
        }
    },
};
</script>

<style scoped>

</style>