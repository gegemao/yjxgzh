<template>
  <div class="app-container">
    <el-card>
      <el-button class="add" type="primary" @click="addsetting">保存</el-button>
      <el-form ref="form" :model="form" label-width="120px">
      <el-form-item label="签到送x积分">
        <el-input v-model="form.jifen" width="200px !important" />
      </el-form-item>
      <el-form-item label="签到送x红包">
        <el-input v-model="form.hongbao" width="200px" />
      </el-form-item>
      <el-form-item label="邀请好友送x张补签卡" label-width="180px">
        <el-input v-model="form.buqian" width="200px" />
      </el-form-item>
    </el-form>
    </el-card>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        jifen: '',
        hongbao: '',
        buqian: ''
      }
    }
  },
  methods: {
    onSubmit() {
      this.$message('submit!')
    },
    onCancel() {
      this.$message({
        message: 'cancel!',
        type: 'warning'
      })
    },
    addsetting(){
      if(this.form.jifen!==""&&this.form.hongbao!==""&&this.form.buqian!==""){
        this.$message.success('保存成功！')
        this.form.jifen='',
        this.form.hongbao='',
        this.form.buqian=''
        return
      }
      this.$message.error('请输入该有的值！')
    }
  }
}
</script>

<style scoped>
.el-card {
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15) !important;
}
.add{
  margin-left: 92%;
}
.el-table {
  margin-top: 15px;
  font-size: 12px;
}
.el-form{
  margin-top: 20px;
}
.el-input{
  width: 30% !important;
}
</style>

