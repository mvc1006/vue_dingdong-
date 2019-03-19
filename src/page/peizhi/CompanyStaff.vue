<template>
  <div>
    <div class="nav">
      <el-button type="primary" size="medium " @click="addYuangong" >新增员工</el-button>
      <el-button type="danger" size="medium"   @click="delParts" style="margin-left: 10px;margin-right: 20px;">删除该部门</el-button>
    </div>
    <el-table
      v-loading="loading"
      :data="tableData3"
      stripe
      border
      style="width: 100%">
      <el-table-column
        prop="UserName"
        label="姓名">
      </el-table-column>
      <el-table-column
        prop="GongHao"
        label="工号">
      </el-table-column>
      <el-table-column
        prop="UserPhone"
        label="电话">
      </el-table-column>
      <el-table-column
        prop="ZhiWei"
        label="职位">
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button
            size="mini"
            @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
  export default {
    name: "CompanyStaff",
    inject:['reload'],
    data(){
      return{
        groupid: '',
        CompanyId:'',
        partLists:[],
        tableData3:[],
        groupname:'',
        firstGroupId:'',
        loading:true
      }
    },
    created(){
      let CompanyId = localStorage.getItem('CompanyId')
      this.CompanyId = CompanyId;
      //部门列表
      axios.get('/api/Handler/Cpy.ashx?type=cpyGroup&cpySn='+CompanyId)
        .then( (response)=> {
          console.log(response.data.data);
          this.partLists = response.data.data;
          let firstGroupId = response.data.data[0].GroupID;
          this.firstGroupId = firstGroupId;
        })
        .catch(function (error) {
          console.log(error);
        });


      //执行单个人员删除操作后的显示   此外还有删除部门后显示该是什么？
      this.groupid = this.$route.query.groupid;
      console.log( this.groupid );
      axios.get('/api/Handler/User.ashx?type=web_UserList&companysn='+this.CompanyId+'&groupid='+this.groupid+'&names='+'')
        .then( (response)=> {
          console.log(response.data.data);
          let userList = response.data.data;
          this.tableData3 = userList;
          this.loading = false;
        })
        .catch(function (error) {
          console.log(error);
        });

    },

    watch:{
      $route(to,from){
        this.groupid = to.query.groupid;
        console.log(this.groupid);
        this.groupname = to.query.groupname;
        axios.get('/api/Handler/User.ashx?type=web_UserList&companysn='+this.CompanyId+'&groupid='+this.groupid+'&names='+'')
          .then( (response)=> {
            console.log(response.data.data);
            let userList = response.data.data;
            this.tableData3 = userList;
            this.loading = false;
          })
          .catch(function (error) {
            console.log(error);
          });
      }

    },
    methods: {
      addYuangong(){
        // this.$router.push({path: 'CompanyStaff', query: {groupid: groupid,groupname:groupname}})
        this.$router.push({path: 'AddStaffs'})
      },
      delParts(){
        this.$confirm('是否要删除'+this.groupname+'部门？', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          axios.get('/api/Handler/Bu.ashx?type=web_delBu&groupid='+this.groupid)
            .then( (response)=> {
              console.log(response.data.errcode);
              let errcode = response.data.errcode;
              let errmsg = response.data.errmsg;
              if(errcode===0){
                //执行删除部门后默认显示第一个部门的信息

                axios.get('/api/Handler/User.ashx?type=web_UserList&companysn='+this.CompanyId+'&groupid='+this.firstGroupId+'&names='+'')
                  .then( (response)=> {
                    console.log(response.data.data);
                    let userList = response.data.data;
                    this.tableData3 = userList;
                    this.$notify({
                      title: '成功',
                      message: '删除成功',
                      type: 'success'
                    });
                    this.reload();
                  })
                  .catch(function (error) {
                    console.log(error);
                  });
              }else{
                this.$notify.error({
                  title: '错误',
                  message: errmsg
                });
              }

            })
            .catch(function (error) {
              console.log(error);
            });

        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });

      },
      handleEdit(index, row) {
        console.log(index, row);
        let userid = row.UserID;
        this.$router.push({path: 'EditStaffs', query: {userid: userid}})
        // console.log(userid)
      },
      handleDelete(index, row) {
        console.log(index, row);
        let userid = row.UserID;
        let usename = row.UserName;
        this.$confirm('是否要删除'+usename+'？', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          axios.get('/api/Handler/User.ashx?type=web_UserDelMulty&uids='+userid)
            .then( (response)=> {
              console.log(response.data.data);
              this.$message({
                type: 'success',
                message: '删除成功!'
              });
              this.reload();
            })
            .catch(function (error) {
              console.log(error);
            });

        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });

      }
    }
  }
</script>

<style lang="less" scoped>
  .nav{
    width: 100%;
    height: 50px;
    /*background: firebrick;*/
    display: flex;
    align-items: center;
    justify-content: flex-start;
    margin-bottom: 10px;
  }
</style>
