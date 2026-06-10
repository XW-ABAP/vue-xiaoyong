<!--
一个可以递归渲染自己的嵌套树组件。
你可以双击一个项目将其转变为一个文件夹。
-->

<script setup>
// 从Vue中引入ref函数
import { ref } from 'vue'
// 引入TreeItem组件
import TreeItem from './components/TreeItem.vue'

// 使用ref定义一个响应式数据treeData，表示树形结构的数据
const treeData = ref({
  name: 'My Tree', // 根节点的名称
  children: [
    { name: 'hello' }, // 第一个子节点
    { name: 'world' }, // 第二个子节点
    {
      name: 'child folder', // 第三个子节点，是一个文件夹
      children: [ // 文件夹的子节点
        {
          name: 'child folder', // 嵌套的文件夹
          children: [{ name: 'hello' }, { name: 'world' }] // 嵌套文件夹的子节点
        },
        { name: 'hello' }, // 文件夹中的普通节点
        { name: 'world' }, // 文件夹中的普通节点
        {
          name: 'child folder', // 文件夹中的另一个嵌套文件夹
          children: [{ name: 'hello' }, { name: 'world' }]
        }
      ]
    }
  ]
})
</script>

<template>
  <ul>
    <TreeItem class="item" :model="treeData"></TreeItem>
  </ul>
</template>

<style>
/* 定义列表项的基本样式 */
.item {
  cursor: pointer;  /* 设置鼠标指针为手型，表示可点击 */
  line-height: 1.5; /* 设置行高为1.5倍，使文本更加易读 */
}

/* 定义加粗文本样式 */
.bold {
  font-weight: bold; /* 设置字体为粗体 */
}
</style>