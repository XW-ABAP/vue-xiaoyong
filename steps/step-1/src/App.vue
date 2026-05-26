<template>
  <h3>模板语法</h3>
  <p>{{ msg }}</p>
  <p>{{ hello }}</p>
  <p>{{ number + 1 }}</p>
  <p>{{ ok ? 'ok' : 'not ok' }}</p>
  <p>{{ message.split('').reverse().join('') }}</p>
  <div v-html="rawHtml"></div>
</template>


<script>
export default {
  data(){
    return{
      msg: 'Hello Vue!',
      hello: 'Hello World!',
      number: 123,
      ok: true,
      message: 'Hello Vue.js!',
      rawHtml: "<a href='https://www.baidu.com'>百度一下</a>"
    }
  }
}

</script>




