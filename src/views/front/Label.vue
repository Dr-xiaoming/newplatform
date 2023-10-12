<template>
  <div style="margin-bottom: 100px">
    <el-select v-model="value1" multiple placeholder="请选择">
      <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value">
      </el-option>
    </el-select>
    <el-button type="primary" @click="submit">主要按钮</el-button>
  </div>
</template>

<script>
export default {
  name: "FrontHome",
  data() {
    return {
      options: [{
        value: 1,
        label: '黄金糕'
      }, {
        value: 2,
        label: 'xxx'
      },{
        value: 3,
        label: 'yyy'
      },{
        value: 4,
        label: '666'
      },{
        value: 5,
        label: '黄金糕5'
      }],
      value1: [1,2,4],
      value2: []
    };
  },
  created() {

  },
  methods: {
      submit(){
        console.log(this.value1)
        this.request.post("/blog/label", this.value1).then(res => {
          if (res.code === '200') {
            this.$message.success("保存成功")
          } else {
            this.$message.error(res.msg)
          }
        })
      }
  }
}
</script>

<style>

</style>
