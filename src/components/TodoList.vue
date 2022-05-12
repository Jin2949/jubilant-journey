<template>
  <div>
    <ul>
      <li v-for="(todo, idx) in tododata" :key="`todo-${idx}`">
        <span 
        :class="{ completed: todo.completed }" 
        @click="toggleTodo(idx)">
        {{ todo.item }}
        </span>
        <i class="fa-solid fa-square-minus removeIcon" @click="removeTodo(todo, idx)"></i>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    tododata: Array,
  },
  methods: {
    removeTodo(todo, idx) {
      this.$emit('removeTodo', todo.item, idx);
    },
    toggleTodo(idx) {
      this.$emit('toggleTodo', idx)
    },
  },
}
</script>

<style>
li {
  display: flex;
  height: 30px;
}

.removeIcon {
  color: rgb(186, 44, 44);
  margin-left: auto;
}

.completed {
  text-decoration: line-through;
  color: grey;
}
</style>