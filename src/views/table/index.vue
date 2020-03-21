<template>
  
    <div>
    <el-button  type="primary" class="add" @click="addFormList">点击添加</el-button>
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
      <el-table-column label="奖品图片" >
        <template slot-scope="scope">
          <img :src="scope.row.shopimgUrl" alt="" class="img">
          <!-- {{scope.row.shopimgUrl}} -->
        </template>
      </el-table-column>
      <el-table-column label="奖品类型" width="200" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.shopW }}</span>
        </template>
      </el-table-column>
      <el-table-column label="奖品名称" width="200" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.shopName }}</span>
        </template>
      </el-table-column>
      <el-table-column class-name="status-col" label="操作" width="200" align="center">
        <template slot-scope="scope">
          <el-button size="mini" type="success" @click="checkList(scope.row.shopId)">修改</el-button>
          <el-button size="mini" type="warning" @click="deleteList(scope.row.shopId)">删除</el-button>
        </template>
      </el-table-column>

    </el-table>


     <el-dialog title="修改用户" :visible.sync="editdialog" width="50%">
            <el-form :model="editForm"  ref="editFormRef"  label-width="70px">
            <el-form-item label="奖品类型">
                <el-input v-model="editForm.shopW"></el-input>
            </el-form-item>
            <el-form-item label="奖品名称">
                <el-input v-model="editForm.shopName"></el-input>
            </el-form-item>
            </el-form>
        <span slot="footer" class="dialog-footer">
            <el-button @click="editdialog = false">取 消</el-button>
            <el-button type="primary" @click="editdialog = false">确 定</el-button>
        </span>
        </el-dialog>



      <el-dialog title="添加用户" :visible.sync="dialogVisible" width="50%" @close="addclosed">
                <!-- 表单内容 -->
           <el-form  :model="addForm" status-icon  ref="addFormref" label-width="70px">
                    <el-form-item label="奖品类型">
                        <el-input v-model="addForm.shopW" ></el-input>
                    </el-form-item>
                    <el-form-item label="奖品名称">
                        <el-input v-model="addForm.shopName" ></el-input>
                    </el-form-item>
            </el-form>
            <!-- ======== -->
            <span slot="footer" class="dialog-footer">
                    <el-button @click="dialogVisible = false">取 消</el-button>
                    <el-button type="primary"  @click="addUser">确 定</el-button>
            </span>
        </el-dialog>
    
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
      editForm:{
        shopW:'',
        shopName:''
      },
      editdialog:false,
      addForm:{
                shopId:2,
                shopW:'',
                shopName:''
      },
      dialogVisible:false
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    async fetchData() {
      this.listLoading = true
      await this.$http.post('http://67a56885.cpolar.io/shop/findAll')
      .then(response => {
        this.list = response.data
        this.listLoading = false
      })
    },
    async deleteList(id){
            const deleteResult=await this.$confirm('此操作将永久删除该用户, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
            }).catch(() => {});
            await this.$http.post('http://67a56885.cpolar.io/shop/del/shopId'+'?shopId='+id)
            // if(res.meta.status!==200) return this.$http.error('删除失败')
            this.$message.success('删除成功')
            this.fetchData()
    },
    async checkList(id){
      this.editdialog=true
      // this.editForm.shopW=this.list.shopW
      await this.$http.post('http://67a56885.cpolar.io/shop/shopAdd'+'?shopId='+id)
      .then(response => {
        console.log(response);
      })
      .catch(error=>{
        console.log(error);
        
      })
    },
    addFormList(){
      this.dialogVisible=true
    },
    async addUser(){
      const shopId=this.addForm.shopId
      const shopW=this.addForm.shopW
      const shopName=this.addForm.shopName
      const res=await this.$http.post('http://67a56885.cpolar.io/shop/shopAdd'+'?shopId='+shopId+'&shopW='+shopW+'&shopName='+shopName)
     if(res.data.statues!==200){
        this.$message.error('添加用户失败')
      }
      this.$message.success('用户添加成功')
      // console.log(this.addForm);
      this.list=this.addForm
      this.dialogVisible=false
      this.fetchData()


    },
        addclosed(){
            this.$refs.addFormref.resetFields()
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
</style>
