<template>
  <div>
    <el-col :span="12"  v-for="(todo, index) in todos" :key="index">
      <el-card class="box-card" shadow="hover" style="margin: 5px 0">
        <!-- <i class="el-icon-circle-check" style="float: right;"></i> -->
        <el-button style="float: right; margin: 5px" @click="done(index)" type="success" icon="el-icon-check" circle></el-button>
        <div style="text-align: left">{{todo.title ? todo.title : todo}}</div>
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
