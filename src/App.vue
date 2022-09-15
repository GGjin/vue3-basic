<template>
  <div id="app">
    <img alt="Vue Logo" src="./assets/logo.png" />
    <h1>
      {{ count }}
    </h1>
    <h1>
      {{ double }}
    </h1>
    <ul>
      <li v-for="number in numbers" :key="number">
        <h1>{{ number }}</h1>
      </li>
    </ul>
    <h1>{{ person.name }}</h1>
    <button @click="increase">👍+1</button>
  </div>
</template>

<script lang="ts">
import { computed, reactive, toRefs, onMounted, onUpdated } from "vue";
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
      console.log("onmounted");
    });
    onUpdated(() => {
      console.log("onupdated");
    });
    const data: DataProps = reactive({
      count: 0,
      increase: () => {
        data.count++;
      },
      double: computed(() => data.count * 2),
      numbers: [1, 2, 3],
      person: {},
    });
    data.numbers[0] = 5;
    data.person.name = "gg";
    const toRefData = toRefs(data);
    return { ...toRefData };
  },
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
