<template>
  <div class="region">
    <div class="content">
      <div class="left">地区：</div>
      <ul class="hospital">
        <li :class="{ active: regionFlag == '' }" @click="changeRegion('')">
          全部
        </li>
        <li
          v-for="item in regionArr"
          :class="{ active: regionFlag == item.value }"
          :key="item.value"
          @click="changeRegion(item.value)"
        >
          {{ item.name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reqHospitalLevelAndRegion } from "@/api/home";
import { onMounted, ref } from "vue";
import type {
  HospitalLevelAndRegionResponseData,
  HospitalLevelAndRegionArr,
} from "@/api/home/type";

let regionArr = ref<HospitalLevelAndRegionArr>([]);
// 控制医院等级高亮的响应式数据
let regionFlag = ref<string>("");

onMounted(() => {
  getRegion();
});

const getRegion = async () => {
  let result: HospitalLevelAndRegionResponseData =
    await reqHospitalLevelAndRegion("Beijin");
  console.log(result);

  if (result.code === 200) {
    regionArr.value = result.data;
  }
};

const changeRegion = (region: string) => {
  regionFlag.value = region;
  $emit("getRegion", region);
};

let $emit = defineEmits(["getRegion"]);
</script>

<script lang="ts">
export default {
  name: "Region",
};
</script>

<style scoped lang="scss">
.region {
  color: #7f7f7f;
  margin: 10px 0;

  .content {
    display: flex;

    .left {
      margin-right: 10px;
      width: 52px;
    }

    ul {
      display: flex;
      flex-wrap: wrap;
      li {
        margin-right: 10px;
        margin-bottom: 10px;

        &.active {
          color: #55a6fe;
        }
      }
      li:hover {
        color: #55a6fe;
        cursor: pointer;
      }
    }
  }
}
</style>
