<template>
    <div class="card" style="width:40%">
      <el-form ref="formRef" :model="data.form" :rules="data.rules" label-width="80px" style="padding-right: 30px">
         <el-form-item  prop="password"label="密码">
            <el-input v-model="data.user.password" placeholder="请输入密码  " autocomplate="off" show-password      ></el-input>
         </el-form-item>
         <el-form-item prop="newpassword"label="新密码">
            <el-input v-model="data.user.newpassword" placeholder="请输入新密码  " autocomplate="off" show-password      ></el-input>
         </el-form-item>
          <el-form-item prop="confirmpassword"label="确认新密码">
            <el-input v-model="data.user.confirmpassword" placeholder="请输入确认密码  " autocomplate="off" show-password      ></el-input>
            
         </el-form-item>
         <div style="text-align:center">
            <el-button type="primary" size="large" @click="updatePassword">保存</el-button>
         </div>
      </el-form>
    </div>
    </template>

    <script setup>
        import {reactive,ref} from"vue";
        import request from "@/utils/request";
        import {ElMessavge} from "element-plus";

        
        const formRef=ref()
        const data=reactive({
           user:JSON.parse(localStorage.getItem('system-user')||'{}'), 
           rules:{
            password:[
                {required:true,message:'请输入原密码',trigger:'blur'},
            ],
            newpassword:[
                {required:true,message:'请输入新密码',trigger:'blur'},

            ],
            confirmpassword:[
                {required:true,message:'请确认新密码',trigger:'blur'},
            ]
        
           }
            
        })
        const updatePassword=()=>{
            if (data.user.newpassword!==data.user.confirmpassword){ 
                ElMessage.warning('两次输入的密码不一致，请确认!')
                return;
            }
            
            request.put('/updatePassword',data.user).then(res=>{
                if(res.code==='200'){
                    ElMessage.success('更新成功')
                    Logout()
                }else{
                    ElMessage.error(res.msg)
                }
            })
        }
        const Logout=()=>{
            router.push('/Login')
            ElMessage.success('请登录')
            localStorage.removeItem('system-user')
        }
    </script>