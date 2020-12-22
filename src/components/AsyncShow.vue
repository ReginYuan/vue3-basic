
<template>
  <h1>{{ result }}</h1>
</template>


<script lang="ts">
import { defineComponent } from "vue";
// 异步组件
export default defineComponent({
  setup() {
    return new Promise(resolve => {
      setTimeout(() => {
        return resolve({
          result: 50
        });
      }, 3000);
    });
  }
});
</script>