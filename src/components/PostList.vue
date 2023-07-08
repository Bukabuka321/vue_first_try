<template>
    <div v-if="posts.length > 0">
        <div>
            <h3>List of posts</h3>
        </div>
        <PostItem v-for="post in posts" :post="post" :key="post.id" @remove="$emit('remove', post)"/>
        </div>
        <h2 v-else style="color: red;">Posts weren't found</h2>
</template>

<script setup>
import PostItem from './PostItem.vue';
//ITS FORBIDDEN TO CHANGE PROPS IN A CHILD COMPONENT!!! !ONLY IN PARENT COMPONENT!!!!
const props = defineProps({
    posts: {
        type: Array, 
        required: true,
    }
})
</script>

<style scoped>

</style>