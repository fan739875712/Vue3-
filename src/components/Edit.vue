<script setup>
// TODO: 编辑
import { ref, defineProps } from 'vue'
import axios from 'axios';
// const props = defineProps({
//   userMessage:Object
// })
// console.log(props);
// 弹框开关
const from = ref({
  id:'',
  name:'',
  place:''
})
const dialogVisible = ref(false)

const open = (row) => {
  console.log(row);
  from.value.id = row.id
  from.value.name = row.name
  from.value.place = row.place
  // 开启弹窗
  dialogVisible.value = true
}
// 暴露open方法
defineExpose({
  open
})
const emit = defineEmits(['on-update'])
const onUpdate = async() => {
  // 提交更改
  await axios.patch(`/edit/${from.value.id}`, {
  name: from.value.name,
  place: from.value.place,
})
// 关闭弹窗
dialogVisible.value = false
// 通知父组件更改成功,触发，更新列表
emit('on-update')
  
}

</script>

<template>
  <el-dialog v-model="dialogVisible" title="编辑" width="400px">
    <el-form label-width="50px">
      <el-form-item label="姓名">
        <el-input placeholder="请输入姓名"  v-model="from.name"/>
      </el-form-item>
      <el-form-item label="籍贯">
        <el-input placeholder="请输入籍贯" v-model="from.place"/>
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="onUpdate">确认</el-button>
      </span>
    </template>
  </el-dialog>
</template>

<style scoped>
.el-input {
  width: 290px;
}
</style>
