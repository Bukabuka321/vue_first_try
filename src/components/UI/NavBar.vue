<template>
    <div class="navbar">
        <div>Vue 3</div>
        <div class="navbar__btns">
            <MyButton @click="$router.push('/posts')">Posts</MyButton>
            <MyButton @click="$router.push('/about')">About</MyButton>
            <MyButton @click="$router.push('/')">Main</MyButton>
        </div>
        <!-- <RouterLink to="/posts">Posts</RouterLink>
        <RouterLink to="/about">About</RouterLink>
        <RouterLink to="/">Main</RouterLink> -->
    </div>
</template>

<script setup>

</script>

<script>
    export default {
        name: "NavBar"
    }
</script>

<style scoped>
.navbar{
    height: 50px;
    background-color: lightgray;
    box-shadow: 2px 2px 4px gray;
    display: flex;
    align-items: center;
    padding: 0 15px;
}

.navbar__btns {
    margin-left: auto;
    display: flex;
    gap: 15px;
}
</style>