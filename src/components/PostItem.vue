<template>
  <div class="post">
    <div>
      <div>{{  post.id }}</div>
      <div><strong>Название:</strong> {{ post.title }}</div>
      <div><strong>Описание:</strong> {{ post.body }}</div>
    </div>
    <div class="post_btns">
      <app-button>Edit</app-button>
      <app-button
        @click="$emit('remove', post)"
      >
        Delete
      </app-button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
.post {
  margin-top: 15px;
  padding: 15px;
  display: flex;
  align-self: center;
  justify-content: space-between;
  border: 2px solid teal;
}
</style>
