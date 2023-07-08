<template>
    <input :value="modelValue" @input="updateInput" class="input" type="text">
</template>

<script setup>
const props = defineProps({
    modelValue: [String, Number]
})

const emit = defineEmits(['update:modelValue']);

const updateInput = (event) => {
    emit('update:modelValue', event.target.value)
}
</script>

<script>
    export default {
        name: "MyInput"
    }
</script>

<style scoped>
.input {
    width: 100%;
    border: 1px solid teal;
    padding: 10px 15px;
    margin-top: 15px;
}
</style>