<template>
  <div>
    <!-- 首页轮播图 -->
    <carousel />
    <!-- 搜索医院的表单 -->
    <search />
    <!-- 医院展示模块 -->
    <el-row gutter="20">
      <el-col :span="20">
        <level @getLevel="getLevel" />
        <region @getRegion="getRegion" />
        <div class="hospital" v-if="hasHospitalArr.length > 0">
          <card
            class="item"
            v-for="(item, index) in hasHospitalArr"
            :key="index"
            :hospitalInfo="item"
          />
        </div>
        <el-empty v-else description="暂无数据" />
        <!-- 分页器 -->
        <el-pagination
          v-model:current-page="pageNo"
          v-model:page-size="pageSize"
          :page-sizes="[10, 20, 30, 40]"
          :background="true"
          layout="prev, pager, next, jumper, ->, sizes, total"
          :total="total"
          @current-change="currentChange"
          @size-change="sizeChange"
        />
      </el-col>
      <el-col :span="4"><tip /></el-col>
    </el-row>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import { reqHospital } from "@/api/home";
import Carousel from "./carousel/index.vue";
import Search from "./search/index.vue";
import Level from "./level/index.vue";
import Region from "./region/index.vue";
import Card from "./card/index.vue";
import Tip from "./tip/index.vue";
import type { Content, HospitalResponseData } from "@/api/home/type";
// 分页器数据
let pageNo = ref<number>(1); // 页码
let pageSize = ref<number>(10);
let hasHospitalArr = ref<Content>([]);
let total = ref<number>(0);

let hostype = ref<string>("");
let districtCode = ref<string>("");

onMounted(() => {
  getHospitalInfo();
});

const getHospitalInfo = async () => {
  let result: HospitalResponseData = await reqHospital(
    pageNo.value,
    pageSize.value,
    hostype.value,
    districtCode.value
  );
  if (result.code === 200) {
    hasHospitalArr.value = result.data.content;
    total.value = result.data.totalElements;
  }
};

const currentChange = () => {
  getHospitalInfo();
};

const sizeChange = () => {
  pageNo.value = 1;
  getHospitalInfo();
};

const getLevel = (level: string) => {
  hostype.value = level;
  getHospitalInfo();
};

const getRegion = (region: string) => {
  districtCode.value = region;
  getHospitalInfo();
};
</script>

<style scoped lang="scss">
.hospital {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  .item {
    width: 48%;
    margin: 10px 0px;
  }
}
</style>
