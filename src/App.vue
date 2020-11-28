<template>
  <div id="app">
    <button @click="openModal">Open Modal</button><br />
    <modal :isOpen="modalIsOpen" @close-modal="onModalClose"> My Modal</modal>
  </div>
</template>

<script lang="ts">
import { ref } from "vue";
import Modal from "./components/Modal.vue";
export default {
  name: "App",
  components: {
    Modal
  },
  setup() {
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
      onModalClose
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
