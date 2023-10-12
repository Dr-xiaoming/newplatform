<template>
  <div style="margin-bottom: 100px">
    <div style="margin: 10px 0">
      <el-carousel height="450px" :interval="10000">
        <el-carousel-item v-for="(item,index) in files" v-if="index < 5" :key="item.id">
          <el-image
              style="width: 100%; height: 100%"
              :src="item.url"
              :fit="fit"></el-image>
        </el-carousel-item>
      </el-carousel>
    </div>

    <div style="margin-top: 15px;padding: 10px 0; border-bottom: 1px solid #ccc; color: orangered; font-size: 20px;font-weight: bold">
      <span style="text-align: center">推荐新闻</span>
    </div>
    <div>
      <el-row :gutter="20" style="margin: 10px 0">
        <el-col :span="4" v-for="item in blogRecommends" :key="item.id" style="margin-bottom: 10px">
          <div style="border: 1px solid #ccc; padding-bottom: 10px; border-radius: 20px; overflow: hidden">
            <img :src="item.img" alt="" style="width: 100%;height: 170px; cursor: pointer">
            <div style="padding: 5px; font-size: 18px; cursor: pointer;text-align: center"
                 @click="$router.push('/front/blogDetail?id=' + item.id)">
              <b style="text-align: center;color: #666666">{{ item.name }}</b>
            </div>
          </div>
        </el-col>
      </el-row>
    </div>


    <el-card style="width: 1200px;  margin: 10px auto">
      <h2 style="margin: 20px 0">公告列表</h2>
      <el-collapse accordion  v-model="active">
        <el-collapse-item v-for="(item,index) in notices" :key="item.id" :name="'' + index">
          <template slot="title">
            <span style="font-size: 20px;">{{ item.name }}</span>
            <span style="margin-left: 10px">{{ item.time }}</span>
          </template>
          <div>{{ item.content }}</div>
        </el-collapse-item>
      </el-collapse>
    </el-card>
  </div>
</template>

<script>
export default {
  name: "FrontHome",
  data() {
    return {
      imgs: [
        require('../../assets/27a28806bdfe7819933acada9742a9fa.jpg'),
        require('../../assets/1.jpeg'),
      ],
      files: [],
      notices: [],
      active: '0',
      blogRecommends: [],
      pageNum: 1,
      pageSize: 5,

    }
  },
  created() {
    // this.request.get("/").then(res => {
    //   this.files = res.data
    // })

    this.request.get("/notice").then(res => {
      this.notices = res.data.splice(0, 10)
    })

    this.request.get("/blog/recommend").then(res => {
      this.blogRecommends = res.data
      console.log(res,'推荐新闻');
    })

    this.request.get("/file/pageHome", {
      params: {
        pageNum: this.pageNum,
        pageSize: this.pageSize,
      }
    }).then(res => {
      this.files = res.data.records
      this.total = res.data.total

    })
  },
  methods: {

  }
}
</script>

<style>

</style>
