<template>
    <div>
        <!-- ! THERE IS AN ALERT IN ACTIONS VARIABLE -->
        <!-- TODO: you need to fix it later -->
         <!-- Import a value from global store -->
         <!-- <h1>{{ $store.state.isAuth ? "Welcome" : "Log in " }}</h1>
        <h1>{{ $store.getters.doubleLike }}</h1>
        <div>
            <MyButton
                @click="$store.commit('incrementLikes')"
            >
                Like
            </MyButton>
            <MyButton
                @click="$store.commit('decrementLikes')"
            >
                Dislike
            </MyButton>
        </div> -->
        <!-- Page heading -->
        <h1>Page with posts</h1>
        
        <!--  Custom input component for search query -->
        <!-- <MyInput
            v-focus
            v-model="serachQuery"
            placeholder="Search by title"
        /> -->
        
        <!-- Container for buttons -->
        <div class="app__btns">
            <!-- Custom button component to create a post -->
            <MyButton
                @click="showDialog"
            >
                Create a post
            </MyButton>
            
            <!-- Custom select component for sorting options -->
            <!-- <MySelect
                v-model="selectedSort"
                :options="sortedOptions"
            /> -->
        </div>
        
        <!-- Dialog component for creating a post -->
        <MyDialog 
            v-model:show="dialogVisible"
        >
            <PostForm @create="createPost"/>
        </MyDialog>
        
        <!-- List of posts component -->
        <PostList 
            :posts="!comp.sortedAndSearchedPosts" 
            @remove="removePost" 
            v-if="!comp.isPostsLoading"
        />
        
        <!-- Loading indicator -->
        <div v-else>Loading...</div>
    </div>
        <!-- Endless scrolling component -->
    <div v-intersection="actions.loadMorePosts" class="observer"></div>
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
import PostForm from "@/components/PostForm.vue"
import PostList from "@/components/PostList.vue"
import { mapState, mapGetters, mapMutations, mapActions} from 'vuex'

// Reactive variables

const dialogVisible = ref(false)    // Boolean to control the visibility of the dialog
 
const actions = {
     ...mapActions({
    loadMorePosts: 'post/loadMorePosts',
      fetchPosts: 'post/fetchPosts'
    })}


const mutations ={
    ...mapMutations({
        setPage: "post/setPage",
})
}

const comp = computed(() => {
    return {
        ...mapState({
            posts: state => state.post.posts,
            dialogVisible: state => state.post.dialogVisible, 
            isPostsLoading: state => state.post.isPostsLoading, 
            selectedSort: state => state.post.selectedSort, 
            serachQuery: state => state.post.serachQuery, 
            page: state => state.post.page, 
            limit: state => state.post.limit, 
            totalPages: state => state.post.totalPages, 
            observerItem: state => state.post.observerItem,
            sortedOptions: state => state.post.sortedOptions
        }),
        ...mapGetters({
            sortedPosts: 'post/sortedPosts', 
            sortedAndSearchedPosts: 'post/sortedAndSearchedPosts'
        })
    }
})

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



onMounted(() => {
   actions.fetchPosts
}) 


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
