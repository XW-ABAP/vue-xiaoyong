<script setup>
// 导入 Vue 3 的组合式 API
import { ref, computed } from 'vue'

// 定义组件接收的 props
const props = defineProps({
  data: Array,       // 表格数据源，对象数组，每个对象代表一行
  columns: Array,    // 需要展示的列名数组（对应对象中的键名）
  filterKey: String  // 全局搜索过滤的关键字
})

// 响应式变量：当前排序依据的列名，空字符串表示不排序
const sortKey = ref('')

// 响应式变量：记录每一列的排序方向
// 1 表示升序，-1 表示降序
// 利用 reduce 遍历 columns，将所有列的初始排序方向设为 1
const sortOrders = ref(
  props.columns.reduce((o, key) => ((o[key] = 1), o), {})
)

// 计算属性：核心逻辑，对数据进行过滤和排序后返回
const filteredData = computed(() => {
  // 从 props 中解构出数据和过滤关键字
  let { data, filterKey } = props

  // ================= 1. 数据过滤逻辑 =================
  if (filterKey) {
    // 统一转为小写，实现大小写不敏感的搜索
    filterKey = filterKey.toLowerCase()
    data = data.filter((row) => {
      // 遍历当前行的所有属性键
      return Object.keys(row).some((key) => {
        // 将属性值转为字符串并转小写，查找是否包含关键字
        // indexOf > -1 等价于 includes，为了兼容性这里用了 indexOf
        return String(row[key]).toLowerCase().indexOf(filterKey) > -1
      })
    })
  }

  // ================= 2. 数据排序逻辑 =================
  const key = sortKey.value
  if (key) {
    // 获取当前列的排序方向（1 或 -1）
    const order = sortOrders.value[key]
    // 使用 slice() 浅拷贝数组后再排序，避免修改原始 props.data（Vue 单向数据流原则）
    data = data.slice().sort((a, b) => {
      // 获取比较的两行数据中，指定排序列的值
      a = a[key]
      b = b[key]
      // 比较逻辑：如果 a === b 返回 0；如果 a > b 返回 1；否则返回 -1
      // 最后乘以 order，实现升序（1 * 1 = 1）或降序（1 * -1 = -1）的控制
      return (a === b ? 0 : a > b ? 1 : -1) * order
    })
  }

  return data
})

/**
 * 点击表头触发的排序函数
 * @param {string} key - 被点击的列名
 */
function sortBy(key) {
  // 设置当前排序列
  sortKey.value = key
  // 翻转该列的排序方向（1 变 -1，-1 变 1）
  sortOrders.value[key] *= -1
}

/**
 * 辅助函数：将字符串首字母大写
 * @param {string} str - 传入的列名字符串
 * @returns {string} - 首字母大写后的字符串
 */
function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1)
}
</script>

<template>
  <!-- 如果过滤后的数据存在，则渲染表格 -->
  <table v-if="filteredData.length">
    <thead>
      <tr>
        <!-- 遍历 columns 数组生成表头单元格 -->
        <th v-for="key in columns" @click="sortBy(key)" :class="{ active: sortKey == key }">
          <!-- 显示格式化后的列名 -->
          {{ capitalize(key) }}
          <!-- 排序方向箭头，根据 sortOrders 的值动态应用 asc 或 dsc 类名 -->
          <span class="arrow" :class="sortOrders[key] > 0 ? 'asc' : 'dsc'">
          </span>
        </th>
      </tr>
    </thead>
    <tbody>
      <!-- 遍历过滤并排序后的数据生成表格行 -->
      <tr v-for="entry in filteredData">
        <!-- 遍历 columns 获取对应的键，从行对象中取值渲染到单元格 -->
        <td v-for="key in columns">
          {{ entry[key] }}
        </td>
      </tr>
    </tbody>
  </table>
  <!-- 如果过滤后无数据，显示提示信息 -->
  <p v-else>No matches found.</p>
</template>

<style>
/* 表格整体样式：边框、圆角和背景色 */
table {
  border: 2px solid #42b983;
  border-radius: 3px;
  background-color: #fff;
}

/* 表头样式 */
th {
  background-color: #42b983;
  color: rgba(255, 255, 255, 0.66);
  /* 默认文字半透明白色 */
  cursor: pointer;
  /* 鼠标悬停变小手，提示可点击 */
  user-select: none;
  /* 禁止选中文字，防止双击排序时误选文本 */
}

/* 表格内容单元格样式 */
td {
  background-color: #f9f9f9;
}

/* 统一设定表头和单元格的最小宽度和内边距 */
th,
td {
  min-width: 120px;
  padding: 10px 20px;
}

/* 当前激活（正在排序）的列，文字变为纯白色，提高对比度 */
th.active {
  color: #fff;
}

/* 激活列的箭头完全不透明 */
th.active .arrow {
  opacity: 1;
}

/* 排序箭头基础样式：利用 CSS border 技巧生成小三角形 */
.arrow {
  display: inline-block;
  vertical-align: middle;
  /* 垂直居中对齐文字 */
  width: 0;
  height: 0;
  margin-left: 5px;
  opacity: 0.66;
  /* 默认半透明 */
}

/* 升序箭头：向下指的三角形（底部边框实色，左右透明） */
.arrow.asc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-bottom: 4px solid #fff;
}

/* 降序箭头：向上指的三角形（顶部边框实色，左右透明） */
.arrow.dsc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 4px solid #fff;
}
</style>
