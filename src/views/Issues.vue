<template>
  <div>
    <h1>todoリスト</h1>
    <el-button type="success" @click="getIssue()">issue取得</el-button>
    <el-row :gutter="12">
      <el-col :span="12"  v-for="(issue, index) in issues" :key="index">
        <el-card class="box-card" shadow="hover" style="margin: 5px 0">
          <div class="clearfix">
            <el-button style="float: right; margin-top: -5px" @click="closeIssue(index)" type="success" icon="el-icon-check" circle></el-button>
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
  name: 'TodoIssues',
  data () {
    return {
      issues: []
    }
  },
  // mounted() {
  //   this.getIssue();
  // },
  methods: {
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
      console.log(process.env.VUE_APP_GITHUB_URL)
      return HTTP.get(`${process.env.VUE_APP_GITHUB_URL}/issues`,
          {
            status: 'open',
          },
        )
        .then((res) => {
          console.log(res)
          this.issues = res.data
      })
    }
  }
}
</script>
