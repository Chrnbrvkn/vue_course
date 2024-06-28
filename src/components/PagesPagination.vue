<template>
  <div class="page__wrapper">
    <div
      v-for="pageNumber in totalPages"
      :key="pageNumber"
      class="post_page"
      :class="{
        'current-page': page === pageNumber,
      }"
      @click="changePage(pageNumber)"
    >
      {{ pageNumber }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    totalPages: {
      type: Number,
      required: true
    },
    currentPage: {
      type: Number,
      required: true
    },
  },
  data() {
    return {};
  },
  methods: {
    changePage(pageNumber) {
      this.$emit('change-page', pageNumber);
    }
  },
  watch: {
    page(){
      this.fetchPosts()
    }
  }
};
</script>

<style scoped>
.page__wrapper {
  display: flex;
  margin-top: 15px;
}
.post_page {
  border: 1px solid teal;
  padding: 10px;
  cursor: pointer;
}
.post_page:hover {
  background-color: rgb(0, 131, 131);
}
.current-page {
  color: white;
  background-color: rgb(1, 100, 100);
  border: 2px solid teal;
}
</style>
