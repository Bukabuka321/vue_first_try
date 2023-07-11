<template>
    <div>
        <!-- Page heading -->
        <h1>Page with posts</h1>
        
        <!-- Custom input component for search query -->
        <MyInput
            v-focus
            v-model="serachQuery"
            placeholder="Search by title"
        />
        
        <!-- Container for buttons -->
        <div class="app__btns">
            <!-- Custom button component to create a post -->
            <MyButton
                @click="showDialog"
            >
                Create a post
            </MyButton>
            
            <!-- Custom select component for sorting options -->
            <MySelect
                v-model="selectedSort"
                :options="sortedOptions"
            />
        </div>
        
        <!-- Dialog component for creating a post -->
        <MyDialog 
            v-model:show="dialogVisible"
        >
            <PostForm @create="createPost"/>
        </MyDialog>
        
        <!-- List of posts component -->
        <PostList 
            :posts="sortedAndSearchedPosts" 
            @remove="removePost" 
            v-if="!isPostsLoading"
        />
        
        <!-- Loading indicator -->
        <div v-else>Loading...</div>
    </div>
        <!-- Endless scrolling component -->
    <div v-intersection="loadMorePosts" class="observer">

    </div>
        <!-- Pagination -->
    <!-- <div class="page__wrapper">
        <div v-for="pageNumber in totalPages"
        :key="pageNumber"
        class="page"
        :class="{
            'current-page': page === pageNumber
        }"
        @click="changePage(pageNumber)"
        >
        {{ pageNumber }}
        </div>
    </div> -->
</template>

<script setup>
import { computed, onMounted, ref } from 'vue';
import axios from 'axios'
import PostForm from "@/components/PostForm.vue"
import PostList from "@/components/PostList.vue"

// Reactive variables

const posts = ref([])               // Array to store the posts
const dialogVisible = ref(false)    // Boolean to control the visibility of the dialog
const isPostsLoading = ref(false)   // Boolean to indicate if the posts are being loaded
const selectedSort = ref('')        // String to store the selected sort option
const serachQuery = ref('')         // String to store the search query
const page = ref(1)                 // Current page number
const limit = ref(10)               // Maximum number of posts to fetch
const totalPages = ref(0)           // Maximum number of pages
// const observerItem = ref(null)

// Static options for sorting

const sortedOptions = ref([
    { value: 'title', name: 'Name or title' },
    { value: 'body', name: 'Text' },
])

// Function to create a new post

const createPost = (post) => {
   posts.value.push(post)
   dialogVisible.value = false
}

// Function to remove a post

const removePost = (post) => {
    posts.value = posts.value.filter(p => p.id !== post.id)
}

// Function to show the dialog

const showDialog = () => {
    dialogVisible.value = true
}

// Fuction to change page

// const changePage = (pageNumber) => {
//     page.value = pageNumber
// }

// Function to fetch the posts from an API

const fetchPosts = async () => {
    try {
        isPostsLoading.value = true
        // Simulating network delay using setTimeout
        setTimeout(async () => {
            const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                params: {
                    _page: page.value,
                    _limit: limit.value,
                },
            });
            totalPages.value = Math.ceil(response.headers['x-total-count'] / limit )
            const data = response.data
            posts.value = data
            isPostsLoading.value = false
        }, 500)
    } catch (error) {
        console.error(error)
    } 
}

// Function to fetch more posts from an API

const loadMorePosts = async () => {
    try {
        page.value += 1;
            const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                params: {
                    _page: page.value,
                    _limit: limit.value,
                },
            });
            totalPages.value = Math.ceil(response.headers['x-total-count'] / limit )
            const data = response.data
            posts.value = [...posts.value, ...data]
    } catch (error) {
        console.error(error)
    } 
}

// Fetch posts when the component is mounted

onMounted(() => {
    fetchPosts();

    // const options = {
    //   rootMargin: "0px",
    //   threshold: 1.0,
    // };

    // const callback = (entries, observer) => {
    //   if (entries[0].isIntersecting && page.value < totalPages.value) {
    //    loadMorePosts()
    //   }
    // };

    // const observer = new IntersectionObserver(callback, options);

    // observer.observe(observerItem.value);
}) 

// Computed property for sorting the posts based on selectedSort value

const sortedPosts = computed(() => {
    return [...posts.value].sort((post1, post2) => {
        return post1[selectedSort.value]?.localeCompare(post2[selectedSort.value])
    })
})

// Computed property for filtering the sorted posts based on search query

const sortedAndSearchedPosts = computed(() => {
    return sortedPosts.value.filter(post => post.title.toLowerCase().includes(serachQuery.value.toLowerCase()))
})

//Watch property to fetch the posts based in changePage function

// watch(page, () => {
//     fetchPosts()
// })
</script>

<style>
.app__btns {
    margin: 15px 0;
    display: flex;
    justify-content: space-between;
}

.page__wrapper {
    width: 100%;
    display: flex;
    justify-content: center;
    margin: 15px 0;
}

.page {
    border: 1px solid black;
    padding: 10px;
    margin-right: 10px;
}

.current-page {
    border: 2px solid teal;
}

.observer {
    height: 30px;
    background: green;
}
</style>
