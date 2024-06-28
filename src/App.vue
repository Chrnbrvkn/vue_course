<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <app-input
     v-model="searchQuery"
     placeholder="Searching..."
    />
    <div class="app__btns">
      <app-button @click="showDialog"> Создать пост </app-button>
      <app-select v-model="selectedSort" :options="sortOptions" />
    </div>
    <app-dialog v-model:show="dialogVisible">
      <post-form @create="createPost" />
    </app-dialog>
    <post-list 
      :posts="searchPosts" 
      @remove="removePost" 
      v-if="!isPostLoading" 
    />
    <div v-else>Loading...</div>
    <pages-pagination
      :totalPages="totalPages"
      :page="page"
      @changePage="changePage"
    />
    <!-- <div class="page__wrapper">
      <div 
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        class="post_page"
        :class="{
          'current-page' : page === pageNumber
        }"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div> -->
  </div>
</template>

<script>
import axios from "axios";
import PostList from "@/components/PostList.vue";
import PostForm from "@/components/PostForm.vue";
import PagesPagination from "@/components/PagesPagination.vue";

export default {
  components: {
    PostList,
    PostForm,
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostLoading: false,
      selectedSort: "",
      searchQuery: '',
      page: 1,
      limit: 5,
      totalPages: 0,
      sortOptions: [
        { value: "id", name: "По номеру" },
        { value: "title", name: "По названию" },
        { value: "body", name: "По содержимому" },
      ],
    };
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter((p) => p.id != post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },
    changePage(pageNumber){
      this.page = pageNumber;
    },
    async fetchPosts() {
      try {
        this.isPostLoading = true;
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts", {
            params: {
              _page: this.page,
              _limit: this.limit
            }
          }
        );
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data;
        console.log(response);
      } catch (e) {
        alert(`Error: ${e.message}`);
      } finally {
        this.isPostLoading = false;
      }
    },
  },
  mounted() {
    this.fetchPosts();
  },
  computed: {
    sortedPosts(){
      return [...this.posts].sort((post1, post2) => {
        if(this.selectedSort === 'id'){
          return post1.id - post2.id
        } else {
          return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
        }
      })
    },
    searchPosts(){
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    },
  },
  watch: {
    page(){
      this.fetchPosts()
    }
  }

};
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
.post_page{
  border: 1px solid teal;
  padding: 10px;
  cursor: pointer;
}
.post_page:hover{
  background-color:  rgb(0, 131, 131);;
}
.current-page {
  color: white;
  background-color:  rgb(1, 100, 100);;
  border: 2px solid teal;
}
</style>
