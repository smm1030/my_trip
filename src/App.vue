<template>
  <div class="app">
    <router-view/>
    <!-- <tab-bar v-if="!route.meta.hideTabBar"/> -->
    <tab-bar v-if="!route.meta.hideTabBar"/>
    <loading/>
  </div>
</template>

<script setup>

import TabBar from "@/components/tab-bar/tab-bar.vue";
import { useRoute } from "vue-router";
import loading from '@/components/loading/loading.vue'
const route = useRoute()
</script>

<style scoped>

</style>
