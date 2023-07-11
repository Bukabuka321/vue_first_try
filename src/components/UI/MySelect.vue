<template>
    <select v-model="props.modelValue" @change="changeOption">
        <option disabled value="">Select a filter</option>
        <option v-for="option in props.options" :key="option.value" :value="option.value">{{ option.name }}</option>
    </select>
</template>

<script setup>
const props = defineProps({
    modelValue: {
        type: String,
    }, 
    options: {
        type: Array,
        default: () => []
    }
})

const emit = defineEmits(['update:modelValue'])

const changeOption = (event) => {
    emit('update:modelValue', event.target.value)
}
</script>

<script>

export default {
    name: 'MySelect'
}

</script>

<style scoped>

</style>