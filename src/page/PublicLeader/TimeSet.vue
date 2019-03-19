<template>
  <el-form :model="ruleForm1"  :rules="rules"  ref="ruleForm1" label-width="100px" class="demo-ruleForm" >
      <!--时段一-->
     <div style="display: flex;align-items: center;justify-content: flex-start" v-if="isshowDefuled">
      <el-form-item label="" prop="pname1" style="">
        <el-input v-model="ruleForm1.pname1" placeholder="早" style="width: 120px;" required></el-input>
      </el-form-item>
      <el-form-item label="" prop="ontime"  class="dd">
        <el-time-select
          placeholder="起始时间"
          @change="shiduan1"
          v-model="ruleForm1.ontime"
          :picker-options="{
            start: '00:00',
            step: '00:15',
            end: '24:00',
          }" style="width: 180px;margin-left: -90px">
        </el-time-select>
      </el-form-item>
      <el-form-item label="" prop="offtime">
        <el-time-select
          placeholder="结束时间"
          v-model="ruleForm1.offtime"
          @change="shiduan2"
          :picker-options="{
           start: '00:00',
           step: '00:15',
           end: '24:00',
           minTime: ruleForm1.ontime
        }" style="width: 180px;margin-left: -90px">
        </el-time-select>
      </el-form-item>
      <el-form-item label="" prop="stime1">
        <el-time-select
          v-model="ruleForm1.stime1"
          @change="shiduan3"
          :picker-options="{
            start: '00:00',
            step: '00:15',
            end: '24:00',
            maxTime:ruleForm1.ontime
          }"
          placeholder="有效签到时间" style="width: 180px;margin-left: -90px">
        </el-time-select>
        <el-button type="primary" size="mini" style="margin-left: 10px;" @click="xinshe1()">新设</el-button>
        <el-button type="danger" size="mini"disabled>删除</el-button>
      </el-form-item>
    </div>
    <!--时段二-->
    <div style="display: flex;align-items: center;justify-content: flex-start" v-if="isshow">
      <el-form-item label="" prop="pname2">
        <el-input v-model="ruleForm1.pname2" placeholder="中" style="width: 120px;"></el-input>
      </el-form-item>
      <el-form-item label="" prop="ontime2">
        <el-time-select
          placeholder="起始时间"
          @change="shiduan4"
          v-model="ruleForm1.ontime2"
          :picker-options="{
            start: '00:00',
            step: '00:15',
            end: '24:00',
          }" style="width: 180px;margin-left: -90px">
        </el-time-select>
      </el-form-item>
      <el-form-item label="" prop="offtime2">
        <el-time-select
          placeholder="结束时间"
          v-model="ruleForm1.offtime2"
          @change="shiduan5"
          :picker-options="{
           start: '00:00',
           step: '00:15',
           end: '24:00',
          minTime: ruleForm1.ontime2
        }" style="width: 180px;margin-left: -90px">
        </el-time-select>
      </el-form-item>
      <el-form-item label="" prop="stime2">
        <el-time-select
          v-model="ruleForm1.stime2"
          @change="shiduan6"
          :picker-options="{
            start: '00:00',
            step: '00:15',
            end: '24:00',
             maxTime:ruleForm1.ontime2
          }"
          placeholder="有效签到时间" style="width: 180px;margin-left: -90px">
        </el-time-select>
        <el-button type="primary" size="mini" style="margin-left: 10px;" @click="xinshe2">新设</el-button>
        <el-button type="danger" size="mini" @click="shanchu2">删除</el-button>
      </el-form-item>
    </div>
    <!--时段三-->
    <div style="display: flex;align-items: center;justify-content: flex-start" v-if="isshow1">
      <el-form-item label="" prop="pname3">
        <el-input v-model="ruleForm1.pname3" placeholder="晚" style="width: 120px;"></el-input>
      </el-form-item>
      <el-form-item label="" prop="ontime3">
        <el-time-select
          placeholder="起始时间"
          @change="shiduan7"
          v-model="ruleForm1.ontime3"
          :picker-options="{
            start: '00:00',
            step: '00:15',
            end: '24:00',
          }" style="width: 180px;margin-left: -90px">
        </el-time-select>
      </el-form-item>
      <el-form-item label="" prop="offtime3">
        <el-time-select
          placeholder="结束时间"
          v-model="ruleForm1.offtime3"
          @change="shiduan8"
          :picker-options="{
          start: '00:00',
          step: '00:15',
          end: '24:00',
          minTime: ruleForm1.ontime3
        }" style="width: 180px;margin-left: -90px">
        </el-time-select>
      </el-form-item>
      <el-form-item label="" prop="stime3">
        <el-time-select
          v-model="ruleForm1.stime3"
          @change="shiduan9"
          :picker-options="{
          start: '00:00',
          step: '00:15',
          end: '24:00',
           maxTime:ruleForm1.ontime3
          }"
          placeholder="有效签到时间" style="width: 180px;margin-left: -90px">
        </el-time-select>
        <el-button type="primary" size="mini" style="margin-left: 10px;" disabled>新设</el-button>
        <el-button type="danger" size="mini" @click="shanchu3">删除</el-button>
      </el-form-item>
    </div>

  </el-form>
</template>

<script>
    export default {
        name: "TimeSet",
        data(){
          return {
            isshowDefuled:true,
            isshow:false,
            isshow1:false,
            ruleForm1: {
              name: '',
              address:'',
              phone:'',
              email:'',
              wifi:'',
              remark:'',
              pname1:'早',
              pname2:'中',
              pname3:'晚',
              ontime:'',
              ontime2:'',
              ontime3:'',
              offtime:'',
              offtime2:'',
              offtime3:'',
              stime1:'',
              stime2:'',
              stime3:'',
            },
            rules: {
              pname1: [
                { required: true, message: '请输入完整信息', trigger: 'blur' },
              ],
              ontime: [
                { required: true, message: '请输入完整信息', trigger: 'blur' },
              ],
              offtime: [
                { required: true, message: '请输入完整信息', trigger: 'blur' },
              ],
              stime1: [
                { required: true, message: '请输入完整信息', trigger: 'blur' },
              ],
              pname2: [
                { required: true, message: '请输入完整信息', trigger: 'blur' },
              ],
              ontime2: [
                { required: true, message: '请输入完整信息', trigger: 'blur' },
              ],
              offtime2: [
                { required: true, message: '请输入完整信息', trigger: 'blur' },
              ],
              stime2: [
                { required: true, message: '请输入完整信息', trigger: 'blur' },
              ],
              pname3: [
                { required: true, message: '请输入完整信息', trigger: 'blur' },
              ],
              ontime3: [
                { required: true, message: '请输入完整信息', trigger: 'blur' },
              ],
              offtime3: [
                { required: true, message: '请输入完整信息', trigger: 'blur' },
              ],
              stime3: [
                { required: true, message: '请输入完整信息', trigger: 'blur' },
              ],

            },
          }
        },
      created(){
        // console.log(111);
      },
      methods: {
        // clickEvent(){
        //   this.$emit('sendValueToParent',this.value1,this.value2,this.value3,true);
        // }
        shiduan1(e){
          this.ontime = e;
          console.log( this.ontime);
        },
        shiduan2(e){
          console.log(e);
        },
        shiduan3(e){
          console.log(e);
        },
        shiduan4(e){
          console.log(e);
        },
        shiduan5(e){
          console.log(e);
        },
        shiduan6(e){
          console.log(e);
        },
        shiduan7(e){
          console.log(e);
        },
        shiduan8(e){
          console.log(e);
        },
        shiduan9(e){
          console.log(e);
        },
        xinshe1(){
          this.isshow = true
        },
        xinshe2(){
          this.isshow1 = true
        },
        shanchu2(){
          this.isshow = false
        },
        shanchu3(){
          this.isshow1 = false
        }

      },
    }
</script>

<style scoped>

</style>
