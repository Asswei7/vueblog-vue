<template>
  <div>
    <Header></Header>
    <div class="blog">
      <h2>{{blog.title}}</h2>
      <el-link icon="el-icon-edit" v-if="ownBlog">
        <router-link :to="{name:'BlogEdit',params:{blogId:blog.id}}">
          编辑
        </router-link>
        <div>
          <el-button @click="fruitDelete(blog.id,blog.title)" type="text" size="small">删除</el-button>
        </div>

      </el-link>
      <el-divider></el-divider>
      <div class="markdown-body" v-html="blog.content"></div>
    </div>
  </div>
</template>

<script>
import 'github-markdown-css'
import Header from "../components/Header";
export default {
  name: "BlogDetail",
  components: { Header },
  data () {
    return {
      blog: {
        id: '',
        title: '',
        content: '',
        description: ''

      },
      ownBlog: false
    }
  },
  methods:{
    fruitDelete(id,title) {
      let _this = this
      this.$confirm('是否确定要删除'+title+'？', '删除数据', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {

        this.$axios.get("/blog/" + id + "/delete").then(function (response) {
          if(response.data){
            _this.$alert('删除成功！', '删除数据', {
              confirmButtonText: '确定',

            });

          }

        })
        _this.$router.push('/blogs')
      }).catch(() => {

      });

    }
  },
  created () {
    //获取动态路由的 blogid
    const blogId = this.$route.params.blogId

    // console.log(blogId.userId)
    const _this = this
    console.log(_this.$store.getters.getUser.id)
    if (blogId) {
      this.$axios.get("/blog/" + blogId).then(res => {
        const blog = res.data.data
        _this.blog.id = blog.id
        _this.blog.title = blog.title
        _this.blog.description = blog.description
        console.log("userId")
        console.log(blog.userId)
        console.log( _this.$store.getters.getUser.id)
        // _this.blog.userId = blog.userId
        //MardownIt 渲染
        let MardownIt = require("markdown-it")
        let md = new MardownIt();
        let result = md.render(blog.content)
        _this.blog.content = result
        //查看是否是登录人 是则可以编辑
        _this.ownBlog = (blog.userId === _this.$store.getters.getUser.id)
        console.log(_this.ownBlog)
      })
    }

  }
}
</script>

<style scoped>
.blog {
  margin-top: 10px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  width: 100%;
  min-height: 700px;
  padding: 10px;
}
</style>
