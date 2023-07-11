<template>
    <div class="dialog" v-if="show" @click.stop="hideDialog">
        <div @click.stop class="dialog__content">
            <slot></slot>
        </div>
    </div>
</template>

<script setup>

const props = defineProps({
    show: {
        type: Boolean, 
        default: false
    }
})

const emit = defineEmits(['update:show'])

const hideDialog = () => {
    emit('update:show', false)
}

</script>

<script>
export default {
    name: 'MyDialog'
}
</script>

<style scoped>
.dialog {
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0, 0, 0, 0.5);
    position: fixed;
}
.dialog__content {
    margin: auto;
    background: white;
    border-radius: 15px;
    min-height: 50px;
    max-width: 300px;
    padding: 20px;
}

</style>