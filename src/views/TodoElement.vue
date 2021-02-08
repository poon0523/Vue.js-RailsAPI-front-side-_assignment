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
        <el-col :span="12"  v-for="todo in todos" :key="todo">
          <el-card class="box-card" shadow="hover" style="margin: 5px 0">
            <div class="clearfix">
              <el-button style="float: right; margin-top: -5px" @click="deleteList(todo)" type="success" icon="el-icon-check" circle></el-button>
            </div>
            <div>{{todo}}</div>
          </el-card>
        </el-col>
        <el-col :span="12"  v-for="issue in issues" :key="issue.id">
          <el-card class="box-card" shadow="hover" style="margin: 5px 0">
            <div class="clearfix">
              <el-button style="float: right; margin-top: -5px" @click="closeIssue(issue.id)" type="success" icon="el-icon-check" circle></el-button>
            </div>
            <div>{{issue.title}}</div>
          </el-card>
        </el-col>
     </el-row>

  </div>
</template>

<script>
import axios from 'axios'
const HTTP = axios.create({
  headers: {
    // 'Authorization': `token ${process.env.VUE_APP_GITHUB_TOKEN}`,
    'Accept': 'application/vnd.github.v3+json',
    'Content-Type':'application/json',
  },
})
export default {
  name: 'Todo',
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
    deleteList(todo){
      const target = this.todos.indexOf(todo);
      this.todos.splice(target, 1);
    },
    closeIssue(id){
      const target = this.issues.find((item) => item.id === id)
      return HTTP.patch(`https://api.github.com/repos/diveintocode-corp/vue-todo/issues/${target.number}`,
        {
          state: "closed"
        },
        // {
        //   headers: {
        //     'Authorization': `token ${process.env.VUE_APP_GITHUB_TOKEN}`,
        //     'Accept': 'application/vnd.github.v3+json',
        //     'Content-Type': 'application/json',
        //   },
        // }
        )
        .then(() => {
          this.issues.some((v, i) => {
            if(v.number==target.number) this.issues.splice(i, 1);
          })
      })
    },
    getIssue() {
      return HTTP.get('https://api.github.com/repos/diveintocode-corp/vue-todo/issues',
          {
            status: 'open',
            // headers: {
            //   'Authorization': `token ${process.env.VUE_APP_GITHUB_TOKEN}`,
            //   'Accept': 'application/vnd.github.v3+json',
            //   'Content-Type': 'application/json',
            // },
          }
        )
        .then((res) => {
          this.issues = res.data
      })
    }
  }
}
</script>
