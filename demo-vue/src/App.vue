<script setup>
import { ref } from 'vue';

const type = ref('base');
const message = ref('你好👋');

const userPath = ref('/user/456');

function warn(message, event) {
  console.log('event', event);
  // 这里可以访问原生事件
  if (event) {
    event.preventDefault();
  }
  alert(message);
}
</script>

<template>
<div class="nav-wepper">
  <div 
    class="mr-10" 
    :class="{active: type === item}" 
    v-for="(item , index) in ['Base','Computed','List','Hello','Form','Life','User','Fa']" 
    :key="`nav_${index}`"
    @click="type = item"
    >
    <RouterLink :to="`/${item}`">{{ item }}</RouterLink>
  </div>  
</div>


  <main>
    <router-view />
  </main>
</template>

<style scoped>
.active {
  color: #ff99cc;
}

.border-black {
  border: #000001 solid px;
}

.pd-20 {
  padding: 20px;
}

.mr-10 {
  display: inline-block;
  margin-right: 10px;
}
</style>
