<template>
  <div id="app">
    <h1>获取的错误：{{ error }}</h1>
    <!-- 异步请求 -->
    <Suspense>
      <!-- 稍后显示 -->
      <template #default>
        <dog-show />
      </template>

      <!-- 最先显示 -->
      <template #fallback>
        <h1>Loading !.....</h1>
      </template>
    </Suspense>
    <br />
    <button @click="openModal">Open Modal</button><br />
    <modal :isOpen="modalIsOpen" @close-modal="onModalClose"> My Modal</modal>
  </div>
</template>

<script lang="ts">
import { ref, onErrorCaptured } from "vue";
import Modal from "@/components/Modal.vue";
import DogShow from "@/components/DogShow.vue";
export default {
  name: "App",
  components: {
    Modal,
    DogShow
  },
  setup() {
    // 抓取错误
    const error = ref(null);
    onErrorCaptured((e: any) => {
      error.value = e;
      return true;
    });
    const modalIsOpen = ref(false);
    const openModal = () => {
      modalIsOpen.value = true;
    };
    const onModalClose = () => {
      modalIsOpen.value = false;
    };
    return {
      modalIsOpen,
      openModal,
      onModalClose,
      error
    };
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
