<template>
  <div id="app">
    <h1>{{ count }}</h1>
    <h1>{{ double }}</h1>
    <ul>
      <li v-for="number in numbers" :key="number">
        <h1>{{ number }}</h1>
      </li>
    </ul>
    <h1>{{ person.name }}</h1>
    <button @click="increase">+1</button>
    <button @click="updateGreetings">Update Title</button>
  </div>
</template>

<script lang="ts">
import {
  ref,
  computed,
  reactive,
  toRefs,
  onMounted,
  onUpdated,
  onRenderTriggered,
  watch
} from "vue";

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
    // const count = ref(0);
    // const double = computed(() => {
    //   return count.value * 2;
    // });
    // const increase = () => {
    //   count.value++;
    // };

    onMounted(() => {
      console.log("mounted");
    });
    onUpdated(() => {
      console.log("updated");
    });
    onRenderTriggered(event => {
      console.log(event);
    });

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
      updateGreetings
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
