<!-- 1级 -->
<script setup>
import Navbar from './components/Navbar.vue';
// 把配置好的路由导入到这里
import { RouterView } from 'vue-router';
</script>


<template>
    <Navbar />
    <RouterView />
</template>

