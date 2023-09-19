<template>
  <div class="container">
    <HospitalTop />
    <div class="content">
      <router-view></router-view>
    </div>
    <HospitalBottom />
  </div>
</template>

<script setup></script>

<style lang="scss" scoped>
.container {
  display: flex;
  flex-direction: column;

  .content {
    margin-top: 70px;
    width: 1200px;
    height: 700px;
  }
}
</style>
