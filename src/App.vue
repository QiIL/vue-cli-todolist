<template>
  <div>
    <input class="item" type="text" v-model="inputValue">
    <button @click="handleClick">提交</button><br>
    未完成：
    <ul>
      <transition-group enter-active-class="animated fadeInLeft" leave-active-class="animated fadeOutRight">
        <todo-item v-for="(item, index) of doingList"
          :key="'doing'+index"
          :item="item"
          :index="item.id"
          @complete="handleComplete"
          @delete="handleDelete"
        ></todo-item>
      </transition-group>
    </ul>
    <!-- 已完成的 -->
    已完成：
    <ul>
      <transition-group enter-active-class="animated fadeInLeft" leave-active-class="animated fadeOutRight">
        <todo-item v-for="(item, index) of doneList"
          :key="'done'+index"
          :item="item"
          :index="item.id"
          @complete="handleComplete(item.id)"
          @delete="handleDelete(item.id)"
        ></todo-item>
      </transition-group>
    </ul>
  </div>
</template>

<script>
import ToDoItem from './components/ToDoItem'
export default {
  components: {
    'todo-item': ToDoItem
  },
  data () {
    return {
      inputValue: '',
      list: []
    }
  },
  created () {
    this.list = localStorage.todoList ? JSON.parse(localStorage.todoList) : []
  },
  computed: {
    doingList () {
      return this.list.filter((item) => { return item.isDone === false })
    },
    doneList () {
      return this.list.filter((item) => { return item.isDone === true })
    }
  },
  methods: {
    handleClick () {
      this.list.push({
        id: this.list.length,
        content: this.inputValue,
        isDone: false
      })
      this.inputValue = ''
    },
    handleDelete (index) {
      this.list.splice(index, 1)
      this.list.forEach((item, index) => {
        item.id = index
      })
      this.saveData()
    },
    handleComplete (id) {
      this.list[id].isDone = !this.list[id].isDone
      this.saveData()
    },
    saveData () {
      localStorage.todoList = JSON.stringify(this.list)
    }
  }
}
</script>

<style>

</style>
