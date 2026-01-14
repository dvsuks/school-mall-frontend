<template>
    <div>
     <div class="card" style="margin-left:5px">
        <el-input style="width:300px" v-model="input2" placeholder="请输入名称查询" :prefix-icon="Search"/>
        <el-button @click="load" type="primary" style="margin-left:10px">查询</el-button>
        <el-button @click="reset" type="Info" >重置</el-button>
     </div>
     <div class="card"syple="margin-bottom:5px">
        <div style="margin-bottom：10px">
            <el-button @click="reset" type="primary" >新增</el-button>
        </div>
        <div>
             <el-table :data="data.tableData" stripe style="width：100%">
            <el-table-column prop="username" label="账号"  />
             <el-table-column prop="name" label="姓名"  />
             <el-table-column prop="role" label="角色" />
             <el-table-column prop="account" label="账户余额" />
             <el-table-column label="操作"width="180" fixed="right">
               <template #default="scope">
                   <el-button type="primary">编辑</el-button>
               <el-button type="danger"@click="delete(scope.row.id)">删除</el-button>
               </template>
             </el-table-column>
             </el-table>
        </div>
     </div>
     <div class="card">
      <el-pagination v-model:current-page="data.pageNum" v-model:page-size="data.pageSize"
      @current-change="load"background layout="total,prev,pager,next" :total="data.total"/>
     </div>
     <el-dialog title="用户信息" v-model="data.formVisible" width="30%" destroy-on-close>
      <el-form ref="formRef" :model="data.form" :rules="data.rules" label-width="80px" style="padding-right: 30px">
         <el-form-item  prop="username"label="账号">
            <el-input v-model="data.form.username" placeholder="请输入账号" autocomplate="off"></el-input>
         </el-form-item>
         <el-form-item prop="name"label="姓名">
            <el-input v-model="data.form.name" placeholder="请输入姓名" autocomplate="off"></el-input>
         </el-form-item>
          <el-form-item prop="avater"label="头像">
            <el-input v-model="data.form.avater" placeholder="请输入头像" autocomplate="off"></el-input>
            <el-upload
            :action="baseUrl +'/files/upload'"
            List-type="picture"
            :on-success="handleFileUpload">
            <el-button type="primary">点击上传</el-button>
         </el-upload>
         </el-form-item>
      </el-form>
      <template #footer>
         <span class="dialog-footer">
            <el-button @click="data.formVisible=false">取消</el-button>
            <el-button type="primary"@click="save">确定</el-button>
         </span>
      </template>
     </el-dialog>
    </div>
    </template>

    <script setup>
      import { reactive } from 'vue';
      import{Search} from "@element-plus/icons-vue";
      import request from "@/utils/request";
      import { ElMessage,ElMessageBox } from "element-plus";

     const data=reactive({
        name:null,
        tableData:[ ],
        total:0,
        pageNum:1,
        pageSize:5

     })

     const load=()=>{
      request.get('/user/selectPage',{
         params:{
            pageNum:data.pageNum,
            pageSize:data.pageSize,
            name:data.name,
         }
      }).then(res=>{
         if(res.code==='200'){
             data.tableData=res.data?.list
             data.total=res.data?.total
         }else{
            ElMessage.error(res.msg)   
         }
      })
     }
     load()
     const reset=()=>{
      data.name=null,
      load()
     }
     const del=(id)=>{
      ElMessageBox.confirm('您确定删除吗?','删除确认',{type:'warning'}).then(res=>{
         request.delete(url,'/user/delete/'+id).then(res=>{
         if(res,code==='200'){
            ElMessage.success('操作成功')
            load()
         }else{
            ElMessage.error(res.msg)
         }
      })
      }).catch(err=>{})
         
      const handleEdit=(row)=>{
         data.form=JSON.parse(JSON.stringify(row))
         data.formVisible=true
      }
      const add=()=>{
         request.post('/user/add',data.form).then(res=>{
            if(res.code==='200'){
               ElMessage.success('操作成功')
               data.formVisible=false
               load()
            }else{
               ElMessage.error(res.msg)
            }
         })
      }
         const update=()=>{
            request.put('/user/update',data.form).then(res=>{
               if(res.code==='200'){
                  ElMessage.success('操作成功')
                  data.formVisible=false
                  load()
               }else{
                  ElMessage. error(res.msg)
               }
            })
         }
         const savue=()=>{
            formRef.value.vaLidate(vaLid) ; {
               if(vaLid){
                  data.form.id?update():add()
               }

            }
         }   
         }
     
    </script>