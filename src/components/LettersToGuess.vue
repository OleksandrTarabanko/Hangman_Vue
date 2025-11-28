<script>
export default {
    name: "LettersToGuess",
    props: {
        currentState: Array,
    },
}
</script>

<template>
    <div>
        <span v-for="letter in currentState" :key="letter">{{ letter }}</span>
    </div>
</template>

<style scoped>
span {
    margin: 10px;
}
</style>