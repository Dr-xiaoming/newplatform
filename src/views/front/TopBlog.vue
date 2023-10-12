<template>
  <div style="width:800px;margin: 15px auto">
    <div
        style="margin-top: 15px;padding: 10px 0; border-bottom: 1px solid #ccc; color: #494949; font-size: 20px;font-weight: bold">
      <span style="text-align: center">新闻浏览量排行榜</span>
    </div>
    <div>
      <el-row :gutter="10" style="margin: 10px 0" v-for="item in blogTops" :key="item.id">
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
  </div>
</template>

<script>
export default {
  name: "FrontHome",
  data() {
    return {
      pageNum: 1,
      pageSize: 6,
      total: 0,
      blogTops: [],
      value: 5.0
    }
  },
  created() {
    this.load()
  },
  methods: {
    load() {
      this.request.get("/blog/top", {
        params: {
          pageNum: this.pageNum,
          pageSize: this.pageSize,
        }
      }).then(res => {
        this.blogTops = res.data.records
        this.total = res.data.total
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
.fontCss{
  font-size: 13px;
  color: #494949;
}
</style>
