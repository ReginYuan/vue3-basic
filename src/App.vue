<template>
  <div id="app">
    <h1>{{ count }}</h1>
    <h1>{{ double }}</h1>
    <h1>{{ greetings }}</h1>
    <h1 v-if="loading">loading!...</h1>
    <img v-if="loaded" :src="result[0].url" />
    <h1>X:{{ x }},Y:{{ y }}</h1>
    <modal></modal>
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

// 在应用中的使用，可以定义不同的数据类型
interface DogResult {
  message: string;
  //xhr.status:获取当前服务器的响应状态  200=>成功
  status: string;
}

interface CatResult {
  id: string;
  url: string;
  width: number;
  height: number;
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
    // 使用 urlLoader 展示狗狗图片
    // const { result, loading, loaded } = useURLLoader<DogResult>(
    //   "https://dog.ceo/api/breeds/image/random"
    // );

    // 免费猫图片的 API  https://api.thecatapi.com/v1/images/search?limit=1
    const { result, loading, loaded } = useURLLoader<CatResult[]>(
      "https://api.thecatapi.com/v1/images/search?limit=1"
    );
    watch(result, () => {
      if (result.value) {
        console.log("value", result.value[0].url);
      }
    });
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
