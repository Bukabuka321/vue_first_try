<template>
    <div class="post">
        <div>
            <div><strong>Name of post: </strong>{{ post.title }}</div>
            <div><strong>Description: </strong>{{ post.body }}</div>
        </div>
        <div class="post__btns">
            <MyButton @click="$emit('remove', post)">Delete</MyButton>
        </div>
    </div>
</template>

<script setup>
const props = defineProps({
    post: {
        type: Object,
        required: true
    }
})

</script>

<style scoped>
.post {
    padding: 15px;
    border: 2px solid teal;
    margin-top: 15px;
    display: flex;
    align-items: center;
    justify-content: space-between;
}
</style>