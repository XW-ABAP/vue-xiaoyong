
<template>
 <!-- <div class = "{{ msg }}"> 测试 </div> -->
 <!-- <div v-bind:id="dynamicID" v-bind:class="dynamicClass" v-bind:title="dynamicTitle"> 测试 </div> -->
 <div :id="dynamicID" :class="dynamicClass" :title="dynamicTitle"> 测试 </div>
 <button :disabled="isButtonDisable"> Button </button>
 <div v-bind="objectOfattrs"> 测试 </div>
</template>

<script>
export default {
  data(){
    return{
      dynamicClass: 'appclass',
      dynamicID: 'appid',
      dynamicTitle: null,
      isButtonDisable: false,
      objectOfattrs: { 
        // dynamicClass: 'appclass',
        // dynamicID: 'appid',
        class: 'appclass',
        id: 'appid',
      }
    }
  }
}

</script>

<style>
.appclass{
  color: red;
  font-size: 20px;
}
</style>
