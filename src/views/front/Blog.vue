<template>
  <div style="width:800px;margin: 15px auto">
      <div style="margin-top: 15px;padding: 10px 0; border-bottom: 1px solid #ccc; color: #494949; font-size: 20px;font-weight: bold">
        <div style="margin: 0 0 0 0">
          <div style="text-align: center">
            <el-input size="small" style="width: 700px" placeholder="请输入新闻名称/新闻类别" v-model="name2" suffix-icon="el-icon-search"></el-input>&nbsp;
            <!--        <el-input size="medium" style="width: 300px" placeholder="请输入评分" v-model="mark" suffix-icon="el-icon-search"></el-input>&nbsp;-->
            <!--        <el-input size="medium" style="width: 300px" placeholder="请输入年份" v-model="age" suffix-icon="el-icon-search"></el-input>-->
            <el-button size="small" class="ml-5" type="info" @click="$router.push('/front/search?name=' + name2)">搜索</el-button>
          </div>
        </div>
      </div>
    <el-row>
      <el-col :span="3">
        <div style="margin-top: 20px;">
          <el-tabs v-model="type" @tab-click="handleClickType"  tab-position="left">
            <el-tab-pane disabled>
              <span slot="label" style="font-size: 15px;color: #8c939d">类型</span>
            </el-tab-pane>
            <el-tab-pane v-for="item in types" :label="item.name"></el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
      <el-col :span="20">
        <div>
          <el-row :gutter="10" style="margin: 10px 0" v-for="item in tableData" :key="item.id">
            <el-col :span="7" style="margin-bottom: 10px">
              <img :src="item.img" alt="" style="width: 100%;height:200px; ">
            </el-col>
            <el-col :span="16" style="margin: 40px 0 10px 20px">
              <div class="fontCss" style="font-weight: bold" @click="$router.push('/front/blogDetail?id=' + item.id)">
                {{ item.name }}
              </div>
              <div style="margin-top: 10px" class="fontCss">
                发布人：{{ item.user }}
              </div>
              <div style="margin-top: 10px" class="fontCss">
                发布时间：{{ item.time }}
              </div>
              <div style="margin-top: 10px" class="fontCss">
                新闻类别：{{ item.blogType }}
              </div>
              <div style="margin-top: 10px" class="fontCss">
                浏览量：{{ item.pageviews }}
              </div>
            </el-col>
          </el-row>

          <div style="padding: 10px 0">
            <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="pageNum"
                :page-sizes="[2, 5, 10, 20]"
                :page-size="pageSize"
                layout="total, prev, pager, next"
                :total="total">
            </el-pagination>
          </div>
        </div>
      </el-col>
    </el-row>


  </div>
</template>

<script>
export default {
  name: "FrontHome",
  data() {
    return {
      pageNum: 1,
      pageSize: 5,
      total: 0,
      filmTops: [],
      value: 5.0,
      tableData: [],
      name: this.$route.query.name,
      form: {},
      user: localStorage.getItem("user") ? JSON.parse(localStorage.getItem("user")) : {},
      type: '',
      types: [],
      name2: ''
    }
  },
  created() {
    this.load()
  },
  methods: {
    handleClickType(tab, event) {
      this.type = this.types[tab.index - 1].name
      this.load();
    },
    load() {
      console.log(this.type)
      this.request.get("/blog/page/type", {
        params: {
          pageNum: this.pageNum,
          pageSize: this.pageSize,
          name: this.name,
          type: this.type
        }
      }).then(res => {
        this.tableData = res.data.records
        this.total = res.data.total
      })

      this.request.get("/blogType").then(res => {
        this.types = res.data
      })
    },
    handleSizeChange(pageSize) {
      console.log(pageSize)
      this.pageSize = pageSize
      this.load()
    },
    handleCurrentChange(pageNum) {
      console.log(pageNum)
      this.pageNum = pageNum
      this.load()
    },
  }
}
</script>

<style>
.fontCss {
  font-size: 13px;
  color: #494949;
}
</style>
