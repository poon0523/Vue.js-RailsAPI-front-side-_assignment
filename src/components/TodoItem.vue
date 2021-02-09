<template>
  <div>
    <el-col :span="12"  v-for="(todo, index) in todos" :key="index">
      <el-card class="box-card" shadow="hover" style="margin: 5px 0">
        <div class="clearfix">
          <el-button style="float: right; margin-top: -5px" @click="done(index)" type="success" icon="el-icon-check" circle></el-button>
        </div>
        <div>{{todo.title ? todo.title : todo}}</div>
      </el-card>
    </el-col>
  </div>
</template>

<script>
export default {
  name: 'TodoItem',
  props: {
    todos: Array
  },
  methods: {
    done(index) {
      this.$emit('handleClick', index);
    }
  }
}
</script>
