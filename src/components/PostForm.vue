<template>
  <form @submit.prevent="createPost">
    <h4>Создание поста</h4>
    <app-input v-model="post.title" type="text" placeholder="Название поста" />
    <app-input v-model="post.body" type="text" placeholder="Описание" />
    <app-button style="margin-top: 15px"> Создать </app-button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      post: {
        title: "",
        body: "",
      },
    };
  },
  methods: {
    createPost() {
      this.post.id = Date.now();
      this.$emit("create", this.post);
      this.post = {
        title: "",
        body: "",
      };
    },
  },

};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}
</style>
