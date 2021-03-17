<template>
  <div>
    <h1>todoリスト</h1>
    <el-row style="margin: 10px 0">
      <form @submit.prevent="addTodo">
        <el-col :span="21">
          <el-input placeholder="todo" v-model="todo" @submit="onSubmit"></el-input>
        </el-col>
        <el-col :span="3">
          <el-button type="primary" @click.prevent="addTodo">作成</el-button>
        </el-col>
      </form> 
    </el-row>
    <el-row :gutter="12">
      <TodoItem @handleClick="removeTodo" :todos="todos" />
      <TodoItem @handleClick="closeIssue" :todos="issues" />
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
  beforeRouteEnter (to, from, next) {
    next(vm => {
      vm.getIssue();
    })
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
      const target = this.issues[index];
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
      this.issues.some((v, i) => {
        if(v.number==target.number) this.issues.splice(i, 1);
      })
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
