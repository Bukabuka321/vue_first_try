<template>
    <div>
        <h1>This is a post page with ID {{ id }}</h1>
        <div>
            <div></div>
            <div><strong>Name of post: </strong>{{ post.title }}</div>
            <div><strong>Description: </strong>{{ post.body }}</div>
            <MyButton @click="$router.push('/posts')">Back</MyButton>
        </div>
    </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios'

const props = defineProps(['id'])

const post = ref({
    body: '', 
    title: ''
})


const fetchPosts = async () => {
    try {
            const response = await axios.get(`https://jsonplaceholder.typicode.com/posts/${props.id}`)
            const data = response.data
            post.value = data
    } catch (error) {
        console.error(error)
    } 
}

onMounted(() => {
    fetchPosts()
})
</script>

<style scoped>

</style>