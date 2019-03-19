<template>
   <div class="box">
     <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
       <el-form-item label="部门名称：" prop="name">
         <el-input v-model="ruleForm.name" style="width: 600px;"></el-input>
       </el-form-item>
       <div class="el-upload__tip" style="margin-left: 100px;">不能超过8个字符</div>
       <el-form-item style="margin-top: 200px;">
         <el-button type="primary" @click="submitForm('ruleForm')">提交保存</el-button>
         <el-button @click="resetForm('ruleForm')">重置</el-button>
       </el-form-item>
     </el-form>
   </div>
</template>

<script>
    export default {
        name: "AddPatrs",
      inject:['reload'],
        data() {
          return {
            ruleForm: {
              name: '',
            },
            rules: {
              name: [
                { required: true, message: '请输入部门名称', trigger: 'blur' },
                { max: 8, message: '不可超过8个字符', trigger: 'blur' }
              ],
            }
          };
        },
        methods: {
          submitForm(formName) {
            this.$refs[formName].validate((valid) => {
              if (valid) {
                axios.get('/api/Handler/Bu.ashx?type=web_addBu&groupname='+this.ruleForm.name)
                  .then( (response)=> {
                    console.log(response.data);
                    let errcode = response.data.errcode;
                    if(errcode===0){
                      this.$message({
                        message: '添加成功！',
                        type: 'success'
                      });
                      this.reload();
                    }

                  })
                  .catch(function (error) {
                    console.log(error);
                  });
              } else {
                console.log('error submit!!');
                return false;
              }
            });
          },
          resetForm(formName) {
            this.$refs[formName].resetFields();
          },
        }
    }
</script>

<style lang="less" scoped>
    .box{
      width: 1000px;
      /*height: 300px;*/
      /*background: forestgreen;*/
      margin: 0 auto;
      margin-top: 100px;
    }
</style>
