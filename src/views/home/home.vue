<template>
  <div class="home" ref="homeRef">
    <home-nav-bar/>
    <div class="banner">
      <img src="@/assets/img/home/banner.webp" alt="">
    </div>
    <home-search-box/>
    <home-categories/>

    <div class="search-bar"  v-if="isShowSearchBar">
      <search-bar :start-date="startDate" :end-date="endDate"/>
    </div>
    <home-content/>
    <!-- <button @click="moreBtnClick">加载更多</button> -->
  </div>
</template>

<script setup>
import HomeNavBar from './cpns/home-nav-bar.vue';
import HomeSearchBox from './cpns/home-search-box.vue'
import useHomeStore from '@/stores/modules/home';
import HomeCategories from "./cpns/home-categories.vue";
import HomeContent from './cpns/home-content.vue';
import SearchBar from '@/components/search-bar/search-bar.vue';
import useScroll from '@/hooks/useScroll'
import { watch,ref,computed,onActivated} from 'vue';
// 发送网络请求
const homeStore = useHomeStore()
homeStore.fetchHotSuggestData()
homeStore.fetchCategoriesData()

homeStore.fetchHouselistData()

// const moreBtnClick = () => {

//   homeStore.fetchHouselistData()
// }

// 监听window窗口滚动
// useScroll(() => {
//   homeStore.fetchHouselistData()
// })

// 监听滚动到底部
const homeRef = ref()
const { isReachBottom, scrollTop } = useScroll(homeRef)
watch(isReachBottom, (newvalue) => {
  if (newvalue) {
    homeStore.fetchHouselistData().then(() => {
      isReachBottom.value = false
    })
  }
})

// 搜索框显示的控制
// const isShowSearchBar = ref(false)
// watch(scrollTop, (newTop) => {
//   isShowSearchBar.value = newTop > 100
// })

const isShowSearchBar = computed(() => {
  return scrollTop.value >= 360
})

// 跳转回home,保留原来的位置
onActivated(() => {
  homeRef.value?.scrollTo({
    top: scrollTop.value
  })
})
</script>

<style lang="less" scoped>
.home {
   padding-bottom: 60px;
   height: 100vh;
   overflow-y: auto;
   box-sizing: border-box;
}
.banner {
  img {
    width: 100%;
  }
}

.search-bar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 45px;
  padding: 16px 16px 10px;
  background-color: #fff;
  z-index: 9;
}
</style>
