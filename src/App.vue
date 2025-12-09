<template>
  <div>
    <div class="card" style="margin-bottom:5px">
      <el-input style="width:300px" v-model="date.name" placeholder="请输入名称查询"：prefix-icon="search"/>
      <el-button @click="load" type="primary"style="margin-left: 10px">查询</el-button>
       <el-button @click="reset" type="Info">重置</el-button>
    </div>
      <div class="card">
      <div style="margin-bottom:10px">
        <el-button @click="reset" type="Info">新增</el-button>
      </div>
      <div>
        <el-table :data="date.tableData" stripe style="width: 100%">
    <el-table-column prop="username" label="账号" width="180" />
     <el-table-column prop="name" label="姓名" width="180" />
    <el-table-column prop="role" label="角色" />
  </el-table>
      </div>
  </div>
  </div>
</template>
<script setup>
  import {reactive}from "vue"
  import{search}from "@element-plus/icons-vue";

  const date=reactive(target,{
    name:null,
    tableDate:[
      {username:'aaa',name:'小小',role:'普通用户'}
    ]
  })
  const load=()=>{

  }
  const reset=()=>{

  }
</script>