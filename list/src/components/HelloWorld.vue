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


        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage1"
          :page-sizes="[10, 20, 30, 40]"
          :page-size="pageSize"
          layout=" prev, pager, next, sizes, total"
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
      pageNum:1

    }
  },
  methods:{
    checkList(){





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
            page: 1,
          },
          success: function (res) {
            // self.data = res.data.slice(0, 3)
            // self.opencode = res.data[0].opencode.split(',')
            console.log(res);
            self.list=res.data
            console.log(self.list);
          }
        })

    },
    getUser: function () {
      let para = {
        pageNum: this.pageNum,
        pageSize: this.pageSize,
      };
      this.loading = true;
      // getList(para).then((res) => {
      //   if(res.data.success){
      //     this.total = res.data.data.total;
      //     this.currentPage1 = res.data.pageNum;
      //     this.users = res.dataList;
      //     this.loading = false;
      //   }else{
      //     this.loading = false;
      //     this.$message({
      //       message:  res.data.returnMsg,
      //       type: 'error'
      //     });
      //   }
      // })


    },
    // //改变时
    // handleSizeChange(val) {
    //   this.pageSize = val;
    //   this.getUser();
    // },
    // //条目改变时
    // handleCurrentChange(val) {
    //   this.pageNum = val;
    //   this.getUser();
    // },
    handleCurrentChange: function(currentPage){
      this.currPage = currentPage;
      this.getUser()
    },
    indexMethod(index) {
      return (this.currPage - 1)*10 + index + 1
    },


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
