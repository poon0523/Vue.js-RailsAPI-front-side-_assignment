<template>
  <div>
    <h1>todoリスト</h1>
    <el-row style="margin: 10px 0">
      <el-col :span="21">
        <el-input placeholder="todo" v-model="todo" @submit="onSubmit"></el-input>
      </el-col>
      <el-col :span="3">
        <el-button type="primary" @click="onSubmit">作成</el-button>
      </el-col>
    </el-row>
    <el-button type="success" @click="getIssue()">issue取得</el-button>
    <el-row :gutter="12">
      <TodoItem @handleClick="deleteList" :todos="todos" />
      <!-- <el-col :span="12"  v-for="(todo, index) in todos" :key="todo">
        <el-card class="box-card" shadow="hover" style="margin: 5px 0">
          <div class="clearfix">
            <el-button style="float: right; margin-top: -5px" @click="deleteList(index)" type="success" icon="el-icon-check" circle></el-button>
          </div>
          <div>{{todo}}</div>
        </el-card>
      </el-col> -->
      <TodoItem @handleClick="closeIssue" :todos="issues" />
      <!-- <el-col :span="12"  v-for="(issue, index) in issues" :key="index">
        <el-card class="box-card" shadow="hover" style="margin: 5px 0">
          <div class="clearfix">
            <el-button style="float: right; margin-top: -5px" @click="closeIssue(index)" type="success" icon="el-icon-check" circle></el-button>
          </div>
          <div>{{issue.title}}</div>
        </el-card>
      </el-col> -->
    </el-row>
  </div>
</template>

<script>
import axios from 'axios'
import TodoItem from '@/components/TodoItem.vue'

const HTTP = axios.create({
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
  methods: {
    onSubmit(){
      this.todos.push(this.todo);
      this.todo= "";
    },
    deleteList(index){
      this.todos.splice(index, 1);
    },
    closeIssue(index){
      const target = this.issues[index];
      // const target = this.issues.find((item) => item.id === id)
      // return HTTP.patch(`${process.env.VUE_APP_GITHUB_URL}/issues/${target.number}`,
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
      return HTTP.get(`${process.env.VUE_APP_GITHUB_URL}/issues`,
          {
            status: 'open',
          },
        )
        .then((res) => {
          this.issues = res.data
      })
    }
  }
}
</script>
