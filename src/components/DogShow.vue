<template>
  <img class="dog" :src="result && result.message" />
</template>
<script lang="ts">
import axios from "axios";
import { defineComponent } from "vue";

// 异步组件写法
export default defineComponent({
  // 异步数据
  async setup() {
    // 异步获取
    const rawData = await axios.get("https://dog.ceo/api/breeds/image");

    return {
      result: rawData.data
    };
  }
});
</script>

<style scoped>
.dog {
  width: 200px;
  height: 150px;
}
</style>>