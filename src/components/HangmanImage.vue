<script>
export default {
    name: "HangmanImage",
    props: {
        hearts: Number,
    }
}
</script>

<template>
    <img :src="`./images/${8 - hearts}.png`">
</template>

<style scoped>
img {
    max-width: 200px;
    max-height: 200px;
    border: 2px solid black;
}
</style>