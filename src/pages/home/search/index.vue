<template>
  <div class="search">
    <el-autocomplete
      clearable
      placeholder="请输入医院名称"
      v-model="hosname"
      :fetch-suggestions="fetchData"
      :trigger-on-focus="false"
      @select="goDetail"
    />
    <el-button type="primary" size="default" :icon="Search"></el-button>
  </div>
</template>

<script setup lang="ts">
import { Search } from "@element-plus/icons-vue";
import { useRouter } from "vue-router";
import { ref } from "vue";
import { reqHospitalInfo } from "@/api/home";
import type { HospitalInfo } from "@/api/home/type";

let $router = useRouter();

let hosname = ref<string>("");

const fetchData = async (keyword: string, cb: any) => {
  let result: HospitalInfo = await reqHospitalInfo(keyword);
  let showData = result.data.map((item) => {
    return {
      value: item.hosname,
      hoscode: item.hoscode,
    };
  });
  cb(showData);
};

const goDetail = (item: any) => {
  $router.push({
    path: "/hospital/register",
    query: { hoscode: item.hoscode },
  });
};
</script>

<script lang="ts">
export default {
  name: "Search",
};
</script>

<style scoped lang="scss">
.search {
  width: 100%;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 10px 0;

  ::v-deep(.el-input__wrapper) {
    width: 600px;
    margin: 0 10px;
  }
}
</style>
