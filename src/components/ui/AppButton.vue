<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'app-button'
};
</script>

<style scoped>
.btn {
  border: 1px solid teal;
  padding: 10px 15px;
  color: teal;
  background: none;
  cursor: pointer;
  align-self: flex-end;
}
</style>
