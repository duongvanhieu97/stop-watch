<template>
<p :class="['todo-item',]">
  {{todoProps.title}} {{todoProps.tinhtoan}}
</p>
</template>

<script>
export default {
  name: "addTodos",
  props: ['todoProps']
}
</script>

<style scoped>

</style>