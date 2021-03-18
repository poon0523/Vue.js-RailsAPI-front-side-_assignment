<template>
  <div>
    <h1>todoリスト</h1>
    <form @submit.prevent="addTodo">
      <el-input placeholder="todo" v-model="todo"></el-input>
    </form> 
    <el-row :gutter="12">
      <TodoItem v-for="( todo, index ) in todos" :key="index" 
          @handleClick="removeTodo" 
          :todo="todo" 
          :index="index" />
      <TodoItem v-for="( issue, index ) in issues" :key="issue.id" 
          @handleClick="closeIssue"
          :todo="issue.title"
          :index="index" />
    </el-row>
  </div>
</template>

<script>
import axios from 'axios';
import TodoItem from '@/components/TodoItem.vue';

const HTTP = axios.create({
  baseURL: `${process.env.VUE_APP_GITHUB_ENDPOINT}`,
  headers: {
    // 'Authorization': `token ${process.env.VUE_APP_GITHUB_TOKEN}`,
    'Accept': 'application/vnd.github.v3+json',
    'Content-Type':'application/json',
  },
})

export default {
  name: 'TodosIssues',
  components: {
    TodoItem,
  },
  data () {
    return {
      todo: "",
      todos: [],
      issues: []
    }
  },
  mounted() {
    this.getIssue();
  },
  methods: {
    addTodo(){
      this.todos.push(this.todo);
      this.todo= "";
    },
    removeTodo(index){
      this.todos.splice(index, 1);
    },
    closeIssue(index){
      // const target = this.issues.find((item) => item.id === id)
      // return HTTP.patch(`/issues/${target.number}`,
      //     {
      //       state: "closed"
      //     },
      //   )
      //   .then(() => {
      //     this.issues.some((v, i) => {
      //       if(v.number==target.number) this.issues.splice(i, 1);
      //     })
      // })
      this.issues.splice(index, 1);
    },
    getIssue() {
      HTTP.get('/issues')
        .then((res) => {
          this.issues = res.data;
      })
    }
  }
}
</script>
