<template>
  <div id="app">
    <h1>{{ count }}</h1>
    <h1>{{ double }}</h1>
    <h1>{{ greetings }}</h1>
    <h1 v-if="loading">loading!...</h1>
    <img v-if="loaded" :src="result.message" />
    <h1>X:{{ x }},Y:{{ y }}</h1>
    <button @click="increase">+1</button>
    <button @click="updateGreetings">Update Title</button>
  </div>
</template>

<script lang="ts">
import { ref, computed, reactive, toRefs, watch } from "vue";
import useMousePosition from "@/hooks/useMousePosition";
import useURLLoader from "@/hooks/useURLLoader";

// 定义接口   定义data的类型 DataProps
interface DataProps {
  count: number;
  double: number;
  increase: () => void;
  numbers: number[];
  person: { name?: string };
}
export default {
  name: "App",
  setup() {
    const data: DataProps = reactive({
      count: 0,
      increase: () => {
        data.count++;
      },
      double: computed(() => data.count * 2),
      numbers: [0, 1, 2],
      person: {}
    });

    data.numbers[0] = 5;
    data.person.name = "ReginYuan";

    const { x, y } = useMousePosition();
    const { result, loading, loaded } = useURLLoader(
      "https://dog.ceo/api/breeds/image/random"
    );
    const greetings = ref("");
    const updateGreetings = () => {
      greetings.value += "hello!";
    };

    watch([greetings, () => data.count], (newValue, oldValue) => {
      console.log(newValue, oldValue);
      document.title = "updated" + greetings.value + data.count;
    });
    // toRefs将普通的对象转换成响应式对象
    const refData = toRefs(data);
    return {
      //...展开refData
      ...refData,
      greetings,
      updateGreetings,
      x,
      y,
      result,
      loading,
      loaded
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
