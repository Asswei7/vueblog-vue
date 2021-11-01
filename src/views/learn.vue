<template>
  <div id="app">
    <div class="input-num">
      <button @click="sub">-</button>
      <span>{{num}}}</span>
      <button @click="add">+</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "learn",
  data:{
    num:1
  },
  methods: {
    add:function (){
      this.num++;
    },
    sub:function (){
      this.num--;
    }
  }
}
</script>

<style scoped>

</style>