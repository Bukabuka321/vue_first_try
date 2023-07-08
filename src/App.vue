<template>
    <div class="app">
        <PostForm @create="createPost"/>
        <PostList :posts="posts" @remove="removePost"/>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import PostForm from "@/components/PostForm.vue"
import PostList from "@/components/PostList.vue"


const posts = ref([
    {id: 1, title: "Javascript", body: "Description"}, 
    {id: 2, title: "Javascript 1", body: "Description"}, 
    {id: 3, title: "Javascript 2", body: "Description"}, 
    {id: 4, title: "Javascript 3", body: "Description"}, 
    {id: 5, title: "Javascript 2", body: "Description"}, 
])

const createPost = (post) => {
   posts.value.push(post)
}

const removePost = (post) => {
    posts.value = posts.value.filter(p => p.id !== post.id)
}

</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.app {
    padding: 20px;
}

</style>