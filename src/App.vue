<script setup>
import { shallowRef } from 'vue'
import Step1 from './tutorial-steps/Step1.vue'
import Step2 from './tutorial-steps/Step2.vue'

// 使用 shallowRef 存储当前激活的组件
const currentView = shallowRef(Step1)
</script>

<template>
  <div class="nav">
    <button @click="currentView = Step1">第一步：响应式</button>
    <button @click="currentView = Step2">第二步：条件渲染</button>
  </div>
  
  <hr>

  <component :is="currentView" />
</template>