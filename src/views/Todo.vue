<template>
  <div>
    <h1>todoリスト</h1>
    <form @submit.prevent="onSubmit">
      <input v-model="todo">
    </form>
    <ul>
      <li v-for="todo in todos" :key="todo">
        {{todo}}
        <span @click="deleteList(todo)">完了</span>
      </li>
    </ul>
     <v-card
      max-width="400"
      class="mx-auto my-auto"
      hover
    ></v-card>
  </div>
</template>

<script>
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Todo',
  data () {
    return {
      todo: "",
      todos: []
    }
  },
  methods: {
    onSubmit(){
      this.todos.push(this.todo);
      this.todo = "";
    },
    deleteList(todo){
      const target = this.todos.indexOf(todo);
      this.todos.splice(target, 1);
    }
  }
}
</script>
