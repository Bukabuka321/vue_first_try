<template>
     <form @submit.prevent>
        <h4>New post</h4>
        <MyInput v-focus v-model="post.title"  type="text" placeholder="Name of post"/>
        <MyInput v-model="post.body" type="text" placeholder="Description of post"/>
        <MyButton @click="createPost" style="align-self: flex-end; margin-top: 15px">Create</MyButton>
    </form>
</template>

<script setup>
import { ref } from 'vue';

let post = ref({
    title: '',
    body: ''
})

const emit = defineEmits(['create']);

const createPost = () => {
    emit('create', post.value);
    post.value.id = Date.now();
    post.value = {
        title: '',
        body: ''
    }
}

</script>

<style scoped>
form {
    display: flex;
    flex-direction: column;
}

</style>