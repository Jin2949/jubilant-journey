<template>
  <div class="inputBox shadow">
    <input type="text" v-model="newTodoItem" @keyup.enter="addTodo">
    <span><i @click="addTodo" class="fa-solid fa-plus"></i></span>
  </div>
</template>

<script>
export default {
  data: () => {
    return {
      newTodoItem: '',
    }
  },
  methods: {
    addTodo() {
      if(this.newTodoItem){
        this.$emit('addTodo', this.newTodoItem)
        this.clearInput()
      }
    },
    clearInput() {
      this.newTodoItem = ''
    },
  },
}
</script>

<style scoped>
  input:focus {
    outline: none;
  }

  input{
    border-style: none;
  }

  .shadow{
    box-shadow: 5px 10px 5px rgba(0, 0, 0, 0.05);
  }
  .inputBox {
    height: 50px;
    line-height: 50px;
    border-radius: 5px;
    background-color: white;
  }
</style>