<template>
    <h1>{{ message }}</h1>

    <button @click="reverseMessage">Reverse Message</button>

    <button @click="message += '!'">Append "!"</button>

    <!-- 代码写法点击后的行为<a href="..." @click="notify">既会触发 notify() 函数，也会跳转到目标网页。
        <a href="..." @click.prevent="notify">只执行 notify() 函数，不会跳转到目标网页。 -->
    <a href="https://vuejs.org" @click.prevent="notify">
        A link with e.preventDefault()
    </a>
</template>

<script>
// 导出 Vue 组件的配置对象
export default {
    // data() 函数必须返回一个对象，该对象中定义了组件需要用到的响应式数据
    data() {
        return {
            // 定义一个名为 message 的字符串变量，初始值为 'Hello World'
            message: 'Hello World'
        }
    },
    // methods 选项包含组件中所有的函数（逻辑代码）
    methods: {
        // 定义 reverseMessage 函数，用于反转字符串
        reverseMessage() {
            // 在选项式 API 中，使用 this 关键字访问 data 中定义的响应式数据
            // split('') 将字符串转为数组，reverse() 反转数组，join('') 再转回字符串
            this.message = this.message.split('').reverse().join('')
        },
        // 定义 notify 函数，触发一个浏览器弹窗
        notify() {
            // alert 是浏览器内置的弹窗方法
            alert('navigation was prevented.')
        }
    }
}
</script>

<style>
/* 样式部分：仅对当前组件内的元素生效 */
button,
a {
    display: block;
    /* 让按钮和链接独占一行 */
    margin-bottom: 1em;
    /* 设置底部的外边距为 1em，使元素间产生间隔 */
}
</style>