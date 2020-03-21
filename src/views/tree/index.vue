<template>
  
    <div>
    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
    >
      <el-table-column align="center" label="ID" width="95">
        <template slot-scope="scope">
          {{ scope.$index }}
        </template>
      </el-table-column>
      <el-table-column label="中奖时间" >
        <template slot-scope="scope">
          {{scope.row.createTime}}
        </template>
      </el-table-column>
      <el-table-column label="中奖用户" width="200" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.createUser }}</span>
        </template>
      </el-table-column>
      <el-table-column label="用户id" width="200" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.userId }}</span>
        </template>
      </el-table-column>
      <el-table-column label="中奖产品标题" width="200" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.shopTitle }}</span>
        </template>
      </el-table-column>
      <el-table-column label="产品类型" width="200" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.shopType }}</span>
        </template>
      </el-table-column>
      <el-table-column label="发货状态" width="200" align="center">
        <template slot-scope="scope">
          <span @click="deliver(scope.row.userId)">{{ scope.row.deliver==1?'发货':'未发货' }}</span>
        </template>
      </el-table-column>
      <el-table-column label="兑换状态" width="200" align="center">
        <template slot-scope="scope">
          <span @click="codeGQ(scope.row.userId)">{{ scope.row.codeGQ==1?'没有拒绝':'拒绝' }}</span>
        </template>
      </el-table-column>
      <el-table-column label="兑换码" width="200" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.code }}</span>
        </template>
      </el-table-column>
      <el-table-column class-name="status-col" label="操作" width="200" align="center">
        <template>
          <el-button size="mini" type="success">修改</el-button>
          <el-button size="mini" type="warning">删除</el-button>
        </template>
      </el-table-column>

    </el-table>

    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="userInfo.pagenum"
      :page-sizes="[1, 2, 3, 4,5]"
      :page-size="userInfo.pagesize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total">
    </el-pagination>
  </div>
</template>

<script>
// import { getList } from '@/api/table'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'gray',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      list: null,
      listLoading: true,
      page:2,
       total:1,
      userInfo:{
        pagesize:1,
        pagenum:2
      }
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    async fetchData() {
      this.listLoading = true
      const page=this.page
      await this.$http.post('http://67a56885.cpolar.io/score/PagefindAll/page'+'?page='+page)
      .then(response => {
        console.log(response.data.rows);
        this.list = response.data.rows
        console.log(this.list);
        
        this.listLoading = false
      })
    },
     handleSizeChange(newSize){
            console.log(newSize);
            this.userInfo.pagesize=newSize
            this.fetchData()  
        },
        handleCurrentChange(newPage){
            console.log(newPage);
            this.userInfo.pagenum=newPage
            this.fetchData()  
        },
      async codeGQ(id){
        console.log(id);
        await this.$http.post('http://67a56885.cpolar.io/score/codeGQ'+'?userId='+id)
      },
      async deliver(id){
        console.log(id);
        
        await this.$http.post('http://67a56885.cpolar.io/score/deliver?userId=aaacgrgraeg'+'?userId='+id)
        .then(response => {
        console.log(response.data);
        if(response.data.statu!==200){
          this.$message.error(response.data.msg)
        }
      })
      }
  }
}
</script>

<style scoped>
.img{
  width: 50px;
  height: 50px;
}
.add{
  margin-left: 92%;
  margin-bottom: 20px;
  margin-top: 20px;
}
.el-pagination{
  margin-top: 30%;
}
</style>
