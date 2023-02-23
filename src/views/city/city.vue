<template>
  <div class="city top-page">
    <div class="top">
      <!-- 搜索框 -->
      <van-search
        v-model="searchValue"
        placeholder="城市/区域/位置"
        shape="round"
        show-action
        @cancel="cancelClick"
      />
      <!-- tab切换 -->
      <van-tabs v-model:active="tabActive" color="#ff9854" line-height="3">
        <template v-for="(value, key, index) in allCities" :key="key">
          <van-tab :title="value.title" :name="key"></van-tab>
        </template>
      </van-tabs>
    </div>
    <div class="content">
      <!-- <template v-for="item in currentGroup?.cities">
        <city-group :group-data="currentGroup"/>
      </template> -->
      <template v-for="(value, key, index) in allCities">
        <city-group v-show="tabActive  === key" :group-data="value"/>
      </template>
    </div>


  </div>
</template>

<script setup>
import { ref,computed } from 'vue';
import { useRouter } from 'vue-router';
import { getCityAll } from "@/services";
import useCityStore from '@/stores/modules/city';
import { storeToRefs } from 'pinia';
import CityGroup from "./cpns/city-group.vue";
const router = useRouter()
//搜索框功能
const searchValue = ref("")
const cancelClick = () => {
  router.back()
}

// tab切换
const tabActive = ref()

// 网络请求: 请求城市数据
// const allCity = ref({})
// getCityAll().then(res => {
//   allCity.value = res.data
// })

// 从store中获取数据
const cityStore = useCityStore()
cityStore.fetchAllCitiesData()
const { allCities } = storeToRefs(cityStore)

// 获取选中标签后的数据
const currentGroup = computed( () => allCities.value[tabActive.value])

</script>

<style lang="less" scoped>

.city {
  // top固定定位
  // .top {
  //   position: fixed;
  //   top: 0;
  //   left: 0;
  //   right: 0;
  // }

  // .comtent {
  //   margin-top: 98px;
  // }
    .top {
      position: relative;
      z-index: 9;
    }
  // 局部滚动
  .content {
    height: calc(100vh - 98px);
    overflow-y: auto;
  }


}
</style>
