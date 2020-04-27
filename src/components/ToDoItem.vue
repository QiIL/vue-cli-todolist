<template>
  <li class="item">
    <input type="checkbox" @click="emitComplete" :checked="item.isDone">
    {{item.content}}
    <button @click="handleClick">删除</button>
  </li>
</template>

<script>
export default {
  props: ['item'],
  methods: {
    handleClick () {
      this.$emit('delete', this.item.id)
    },
    emitComplete () {
      this.$emit('complete', this.item.id)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.item {
  color: blue;
}
</style>
