<script setup>
import { ref } from 'vue'
import {ElNotification,ElButton} from 'element-plus'
const url='ws://localhost:8080/websocket'
const info = {
  id:1,
  roomId:1,
  patientId:1,
  status:2
}
const equipment={
  id:null,
  equipmentId:"qqwe2"
}
const message = {
  type: "edit",
  data: info
}
const saveMessage={
  type:'save',
  data: equipment
}
const jsonData=JSON.stringify(message);
const socket=new WebSocket(url)
socket.onopen = function(e){
  ElNotification({
    type:'success',
    message:'连接成功',
    duration :2000
  })
  socket.send(JSON.stringify(saveMessage))
}
const onclose=()=>{
  socket.close()
  ElNotification({
    type:'error',
    message:'断开连接',
    duration :2000
  })
}
socket.onmessage=(e)=>{
  ElNotification({
    type:'info',
    message:e.data,
    duration :2000
  })
}
socket.onerror=()=>{
  ElNotification({
    type:'error',
    message:'连接错误',
    duration :2000
  })
}
const sendUUid=()=>{
  socket.send(jsonData)
}
</script>

<template>
  <ElButton type="primary" @click="sendUUid()">发送id</ElButton>
  <ElButton type="primary" @click="onclose()">断开连接</ElButton>
</template>

<style scoped>
</style>
