<template>
  <p>
    <span :title="message">
      Hover your mouse over me for a few seconds to see my dynamically bound title!
    </span>
  </p>

  <p :class="{ red: isRed }" @click="toggleRed">
    This should be red... but click me to toggle it.
  </p>
  <p :style="{ color }" @click="toggleColor">
    This should be green... but click me to toggle it.
  </p>
</template>


<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      message: 'Hello Vue.js!',
      isRed: true,
      color: 'green'
    }
  },
  methods: {
    toggleRed() {
      this.isRed = !this.isRed
    },
    toggleColor() {
      this.color = this.color === 'green' ? 'blue' : 'green'
    }
  }
}
</script>

<style>
.red {
  color: red;
}
</style>