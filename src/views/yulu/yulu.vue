<template>
  
    <div>
    <el-button  type="primary" class="add" @click="addyulu">点击添加</el-button>
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
      <el-table-column label="关键字" >
        <template slot-scope="scope">
          {{scope.row.word}}
        </template>
      </el-table-column>
      <el-table-column label="励志语录内容" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.content }}</span>
        </template>
      </el-table-column>
      <el-table-column class-name="status-col" label="操作" width="200" align="center">
        <template>
          <el-button size="mini" type="success">新建</el-button>
          <el-button size="mini" type="warning">删除</el-button>
        </template>
      </el-table-column>

    </el-table>



      <el-dialog
      title="添加语录" :visible.sync="dialogVisible" width="50%">
      <el-form ref="yuluFormRef" :model="yuluForm" label-width="80px">
          <el-form-item label="关键字">
            <el-input v-model="yuluForm.word"></el-input>
          </el-form-item>
          <el-form-item label="语录内容" >
            <el-input v-model="yuluForm.content"></el-input>
          </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="trueList">确 定</el-button>
      </span>
    </el-dialog>


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
      number:2,
      nums:3,
      dialogVisible:false,
      yuluForm:{
        word:'',
        content:''
      },
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
      const number=this.userInfo.pagenum
      const nums=this.userInfo.pagesize
      await this.$http.post('http://67a56885.cpolar.io/Quto/lookQuto'+'?number='+number+'&nums='+nums,)
      .then(response => {
        console.log(response.data.data[0]);
        this.list =Array(response.data.data[0])
        this.listLoading = false
      })
    },
    addyulu(){
      this.dialogVisible=true
    },
    async trueList(){
              this.$refs.yuluFormRef.validate(async valid=>{
                if(!valid) return
                this.list.word=this.yuluForm.word
                this.list.content=this.yuluForm.content
                console.log(this.yuluForm.word);
                this.$message.success('用户添加成功')
                this.dialogVisible=false
                this.fetchData()
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
