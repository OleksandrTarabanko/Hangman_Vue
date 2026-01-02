<script>
export default {
    name: "Keyboard",
    props: {
        keyboardWhiteList: String,
    },

    data() {
        return {
            whiteList: this.normalizeCase(this.keyboardWhiteList)
        }
    },

    methods: {
        normalizeCase(string) {
            return string.toUpperCase()
        }
    }
}
</script>

<template>
    <div>
        <button @click="$emit('handleKey', { keyPressed: keyButton })" v-for="keyButton in whiteList" :key="keyButton">
            {{ keyButton }}
        </button>
    </div>
</template>

<style scoped>
div {
    max-width: 650px;
    margin: 50px auto;
}

button {
    border: 1px solid black;
    margin: 5px;
}

button:hover {
    background-color: black;
    color: white;
}
</style>