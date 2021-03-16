<template>
  <div>
    <h1>todoリスト</h1>
    <form @submit.prevent="addTodo">
      <el-input placeholder="todo" v-model="todo"></el-input>
    </form>
    <el-row :gutter="12">
      <el-col :span="12"  v-for="( todo, index ) in todos" :key="index">
        <el-card class="box-card" shadow="hover" style="margin: 5px 0;">
          <el-row :gutter="12">
            <el-col :span="21">{{ todo }}</el-col>
            <el-col :span="3">
              <el-button @click="removeTodo(todo)" type="success" icon="el-icon-check" circle></el-button>
            </el-col>
          </el-row>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: 'Todos',
  data () {
    return {
      todo: "",
      todos: []
    }
  },
  methods: {
    addTodo(){
      this.todos.push(this.todo);
      this.todo = "";
    },
    removeTodo(todo){
      const target = this.todos.indexOf(todo);
      this.todos.splice(target, 1);
    }
  }
}
</script>
