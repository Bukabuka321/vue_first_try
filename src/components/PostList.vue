<template>
    <div v-if="posts.length > 0">
        <div>
            <h3>List of posts</h3>
        </div>
        <TransitionGroup name="post-list">
            <PostItem v-for="post in posts" 
            :post="post" 
            :key="post.id" 
            @remove="$emit('remove', post)"/>
        </TransitionGroup>
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
.post-list-move, 
.post-list-enter-active,
.post-list-leave-active {
  transition: all 0.5s ease;
}
.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: translateX(130px);
}
</style>