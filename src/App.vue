<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <my-input
            v-model="searchQuery"
            placeholder="Поиск...."
        />
        <div class="app__btns">
            <my-button
                @click="showDialog"
            >
                Создать пост
            </my-button>
            <my-select
            v-model="selectedSort"
            :options="sortOptions"
            />

            </div>
        <my-dialog v-model:show="dialogVisible">
            <post-form
            @create="createPost"
            />
        </my-dialog>
        
        <post-list 
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
        v-if="!isPostsLoading"
        />
        <div v-else>Идёт загрузка...</div>
        <div class="page__wrapper">
            <div 
            v-for="page in totalPages"
            :key="page"
            class="page"
            >
                {{ page }}
            </div>
        </div>
    </div>
</template>

<script>

import PostForm from '@/components/PostForm.vue';
import PostList from '@/components/PostList.vue';
import axios from 'axios';
import MySelect from './components/UI/MySelect.vue';
export default {
    components: {
    PostList,
    PostForm,
    MySelect
},
    data() {
        return {
            posts: [],
            dialogVisible: false,
            isPostsLoading: true,
            selectedSort: '',
            searchQuery : '',
            page : 1,
            limit: 10,
            totalPages: 0,
            sortOptions: [
                {value: 'title', name: 'По названию'},
                {value: 'body', name: 'По содержимому'},
                // {value: 'id', name: 'По id'},
            ]
        }
    },
    methods: {
        createPost(post) {
            this.posts.push(post);
            this.dialogVisible = false;
        },
        removePost(post) {
            this.posts = this.posts.filter(p => p.id !== post.id)
        },
        showDialog() {
            this.dialogVisible = true;
        },
        async fetchPosts() {
            try {
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                        params: {
                            _page: this.page,
                            _limit: this.limit
                        }
                    })
                    this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
                    this.posts = response.data;
            } catch (e) {
                console.log(e);
                alert('Ошибка')
            } finally {
                this.isPostsLoading = false;
            }
        },
    },
    mounted() {
         this.fetchPosts();
    },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(this.selectedSort))
        },
        sortedAndSearchedPosts() {
            return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
        }
    },
    watch : {
        
    }
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

.app__btns {
    display: flex;
    justify-content: space-between;
    margin: 15px 0;
}

.page__wrapper {
    display: flex;
    margin-top: 15px;
}

.page {
    border:1px solid black;
    padding: 10px;
}

</style>