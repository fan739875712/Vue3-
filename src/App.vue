<script setup>
import { onMounted, ref } from 'vue';
import Edit from './components/Edit.vue'
import axios from 'axios'
// TODO: 列表渲染
//声明响应式-> 调用接口请求数据 -> 赋值给响应式 -> 绑定到table
const list = ref([]);
const getList = async ()=>{
  const res = await axios.get('/list');
  // 注意使用value接收
  list.value = res.data;
  // console.log(list);
}
onMounted((id)=>getList())
// TODO: 删除功能
const delectById = async(id) => {
  // const res = await axios.delete(`/del/${id}`)
  const res = await axios.delete('/del/'+id)
  console.log(res.data);
  // 在次获取数据列表
  getList()

}


// TODO: 编辑功能
// 弹出信息框 -> 查询信息渲染上去，或者直接从list row获取 ->  修改信息 -> 重新获取list 
const editRef = ref(null)
const onEdit = (row) => {
  editRef.value.open(row)
  
}
</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <!-- 注意下面的插槽操作 -->
        <template #default="{row}">
          <el-button type="primary" @click="onEdit(row)" link>编辑</el-button>
        

          <el-button type="danger" @click="delectById(row.id)" link>删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  <!-- <Edit :userMessage=row ></Edit> -->
  <Edit ref="editRef" @on-update="getList"></Edit>
  </div>
  <Edit />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
