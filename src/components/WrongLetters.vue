<script>

export default {
    name: "WrongLetters",
    props: {
        wrongLettersList: Array,
    },

    methods: {
        normalizeArr(arr) {
            return arr.join("").toUpperCase()
        }
    }
}
</script>

<template>
    <div>
        Wrong Letters:
        <span v-for="wrongLetter in normalizeArr(wrongLettersList)" :key="wrongLetter">{{ wrongLetter }}</span>
    </div>
</template>

<style scoped>
span {
    margin: 10px;
}
</style>