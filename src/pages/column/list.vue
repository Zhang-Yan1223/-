<template>
  <div style="padding:1em">
      <h1>栏目管理</h1>
    <el-button type="primary" size="small" @click="toAddHandlar">添加</el-button>
    <el-button type="danger" size="small">删除</el-button>
    <el-table :data="columns">
        <el-table-column prop="id" label="编号"></el-table-column>
        <el-table-column prop="name" label="栏目名称"></el-table-column>
        <el-table-column prop="num" label=" 序号"></el-table-column>
        <el-table-column prop="icon" label="父栏目" width="200"></el-table-column>
        <el-table-column label="操作">
            <template v-slot="slot">
                <a href="" @click.prevent="toDeleteHandle(slot.row.id)">删除</a>
                <a href="" @click.prevent="toupdateHandler">修改</a>
                <a href="" @click.prevent="toAddHandlar">详情</a>
            </template>
        </el-table-column>
    </el-table>
    <!-- 分页 -->
    <el-pagination
    background
    layout="prev, pager, next"
    :total="50">
    </el-pagination>
    <!-- 分页结束 -->
    <el-dialog
        title="录入栏目信息"
        :visible.sync="visible"
        width="60%">
        {{form}}
        <!-- model双向数据绑定 -->
        <el-form :model="form" label-width="80px">
          <el-form-item label="编号" >
            <el-input v-model="form.id"></el-input>  
          </el-form-item>
          <el-form-item label="栏目名称" >
            <el-input v-model="form.name"></el-input>  
          </el-form-item> 
          <el-form-item label="序号" >
            <el-input v-model="form.num"></el-input>  
          </el-form-item>
          <el-form-item label="父栏目" >
            <el-input v-model="form.icon"></el-input>  
          </el-form-item>       
        </el-form>
        <!-- :before-close="handleClose" -->
        <!-- <span>这是一段信息</span> -->
        <span slot="footer" class="dialog-footer">
            <el-button @click="closeModelHandler">取消</el-button>
            <el-button type="primary" @click="submitHandle">确定</el-button>
        </span>
    </el-dialog>
  </div>
</template>

<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
  methods: {
    loadDate(){
      let url = 'http://localhost:6677/category/findAll'
      request.get(url).then((response)=>{
        this.columns = response.data;
      })
    },
    submitHandle() {
      //通过request与后台进行交互
      let url = 'http://localhost:6677/category/saveOrUpdate'
      // request.post(url.this.form);
      request({
        url,
        method : "post",
        headers:{
          "Content-Type":"application/x-www-form-urlencoded"
        },
        data : querystring.stringify(this.form)
      }).then((response)=>{
        //请求结束
        this.closeModelHandler();
        this.loadDate();
        this.$message({
          type:"success",
          $message:response.$message,
        })
      })
    },
    toDeleteHandle() {
      this.$confirm('确定删除这段数据？', {
        confirmButtonText: '是的',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$message({
          type: 'success',
          message: '已删除'
        })
      })
    },
    toupdateHandler() {
      this.visible = true
    },
    closeModelHandler() {
      this.visible = false
    },
    toAddHandlar() {
      this.visible = true
    }
  },
  data() {
    return {
      visible: false,
       form:{
         type : "columns"
  },
      columns : [{
      }]
    }
  },
  created() {
    // // vue实例创建完毕
    // let url = 'http://localhost:6677/customer/findAll'
    // request.get(url).then((response)=>{
    //   // 将查询结果设置到customers
    //   this.customers = response.data;
    // },
    this.loadDate();
  },
 
}
</script>
<style scoped>

</style>
