<script setup>
import { ref, computed } from 'vue' // 从Vue中导入ref和computed函数

// 定义组件的props，接收一个model对象
const props = defineProps({
    model: Object
})

// 定义一个响应式变量isOpen，用于控制文件夹的展开/收起状态
const isOpen = ref(false)
// 计算属性isFolder，判断当前节点是否为文件夹（是否有子节点）
const isFolder = computed(() => {
    return props.model.children && props.model.children.length
})

// 切换文件夹展开/收起状态的函数
function toggle() {
    isOpen.value = !isOpen.value
}

// 改变节点类型的函数，如果是文件则转换为文件夹
function changeType() {
    if (!isFolder.value) {
        props.model.children = [] // 初始化子节点数组
        addChild() // 添加一个默认子节点
        isOpen.value = true
    }
}

// 添加子节点的函数
function addChild() {
    props.model.children.push({ name: 'new stuff' }) // 添加一个名为"new stuff"的新节点
}
</script>

<template>
    <li>
        <!-- 
            节点显示区域，根据是否为文件夹添加bold类，
            点击切换展开/收起状态，双击改变节点类型
        -->
        <div :class="{ bold: isFolder }" @click="toggle" @dblclick="changeType">
            {{ model.name }} <!-- 显示节点名称 -->
            <!-- 如果是文件夹，显示展开/收起图标 -->
            <span v-if="isFolder">[{{ isOpen ? '-' : '+' }}]</span>
        </div>
        <ul v-show="isOpen" v-if="isFolder">
            <!--
        一个可以通过其“name”选项递归渲染自己的组件，
        (如果使用单文件组件，则从文件名推断)
            -->
            <TreeItem class="item" v-for="model in model.children" :model="model">
            </TreeItem>
            <li class="add" @click="addChild">+</li>
        </ul>
    </li>
</template>