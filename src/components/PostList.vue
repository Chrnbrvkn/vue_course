<template>
  <div v-if="posts.length > 0">
    <h3>Список постов</h3>

    <transition-group name="post-list">
      <post-item 
        v-for="post in posts"
        :post
        :key="post.id"
        @remove="$emit('remove', post)"
      />
    </transition-group>
    </div>
  <h3 v-else style="color: red">Список постов пуст</h3>
</template>

<script>
import PostItem from '@/components/PostItem.vue'
  export default {
  components: { PostItem },
    props: {
      posts: {
        type: Array,
        required: true
      },
    }
  }
</script>

<style scoped>
.post-list-move,
.post-list-enter-active,
.post-list-leave-active {
  transition: all 0.6s cubic-bezier(0.55, 0, 0.1, 1);
}
.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: scaleY(0.01) translate(30px, 0);
}
.post-list-leave-active {
  position: absolute;
}
</style>