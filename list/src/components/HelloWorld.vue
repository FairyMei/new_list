<template>
  <div class="list">
    <el-container>
      <el-main>
        <ul>
          <li v-for="item in list">
            <div>
              <p>{{item.time}} </p>
              <p>{{item.title}}</p>
            </div>
          </li>
        </ul>


        <!--<el-pagination-->
          <!--@size-change="handleSizeChange"-->
          <!--@current-change="handleCurrentChange"-->
          <!--:current-page="currentPage1"-->
          <!--:page-sizes="[10, 20, 30, 40]"-->
          <!--:page-size="pageSize"-->
          <!--layout=" prev, pager, next, sizes, total"-->
          <!--:total="total">-->
        <!--</el-pagination>-->


        <el-pagination
          background
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage1"
          :page-sizes="[10, 20, 30, 40]"
          :page-size="pageSize"
          layout="prev, pager, next,sizes, total"
          :total="total">
        </el-pagination>
      </el-main>



    </el-container>
  </div>
</template>

<script>
  import axios from 'axios'
export default {
  data () {
    return {
      list:[],
      currentPage1: 1,
      total: 10,
      page: 1,
      pageSize: 10,
      pageNum:''

    }
  },
  methods:{
    checkList(val){
        var self = this
      $.ajax({
          url: 'http://cms.hzzkj.net/admin/api/article_list',
          type: 'GET',
          dataType: 'JSONP',
          jsonp: 'callback', //指定一个查询参数名称来覆盖默认的 jsonp 回调参数名 callback
          jsonpCallback: 'handleResponse', //设置回调函数名
          data: {
            uid:10001,
            key:'wqr',
            limit: 10,
            page: this.currentPage1,
          },
          success: function (res) {
            // self.data = res.data.slice(0, 3)
            // self.opencode = res.data[0].opencode.split(',')
            console.log(res);
            self.list=res.data

          }
        })

    },
    getUser: function (val) {
      var urla="http://cms.hzzkj.net/admin/api/article_list"
      let dataa = {
        pageNum: this.pageNum,
        pageSize: this.pageSize,
        uid:10001,
        key:'wqr',
        limit: 10,
        page: 1,
      };
      this.loading = true;
      // axios.get(urla,{params:dataa}).then((res)=>{
      //   return Promise.resolve(res.data)
      //   console.log(res);
      // }).catch((err)=> {
      //   console.log("网络繁忙")
      // })

      this.$http.jsonp("http://cms.hzzkj.net/admin/api/article_list",{
        params: {
              uid:10001,
              key:'wqr',
              limit: 10,
              page:this.pageNum,//输入的关键词
        },
        jsonpCallback:'callbackFunction'
      }).then(function(res) {
        this.total = res.body.total;
        this.currentPage1 = res.body.curr;
        this.list = res.body.data;
        console.info(res);
      },function(res) {//失败显示状态码
        // alert("res.status:"+res.status)
      })
    },
    //改变时
    handleSizeChange(val) {
      this.pageSize = val;
      this.getUser();
      console.log(this.pageSize);
    },
    //条目改变时
    handleCurrentChange(val) {
      this.pageNum = val;
      this.getUser();
      console.log('this.pageNum:',this.pageNum);
    },
    // handleCurrentChange: function(currentPage){
    //   this.currPage = currentPage;
    //   this.getUser()
    // },
    // indexMethod(index) {
    //   console.log(index)
    //   return (this.pageNum - 1)*10 + index + 1
    //   console.log((this.pageNum - 1)*10)
    // },


  },
  created:function () {
    this.checkList();
    this.getUser();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .list{
    background-color: pink;
    height: auto;
  }
h1, h2 {
  font-weight: normal;
}
ul {
  padding: 0;
}
li {
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
