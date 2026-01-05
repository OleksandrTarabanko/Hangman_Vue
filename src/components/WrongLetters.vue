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
div {
    margin: 20px auto 0 auto;
}

span {
    margin: 10px;
    padding: 5px;
    border: 1.5px solid black;
}
</style>