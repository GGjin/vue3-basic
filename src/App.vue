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
    <h1>X:{{ x }},Y:{{ y }}</h1>
    <h1 v-if="loading">Loading!...</h1>
    <button @click="openModal">open modal</button>
    <dialog-model :isOpen="modalIsOpen" @close-modal="closeDialogModal"
      >my modal!!!!</dialog-model
    >
    <img v-if="loaded" :src="result.message" width="100px" height="100px" />
    <button @click="increase">👍+1</button>
    <button @click="updateGreeting">添加title</button>
  </div>
  <div id="model"></div>
</template>

<script lang="ts">
import {
  computed,
  reactive,
  toRefs,
  onMounted,
  onUpdated,
  watch,
  ref,
} from "vue";
import useMounsePosition from "./hooks/userMousePosition";
import useURLLoader from "./hooks/useURLLoader";
import DialogModel from "./components/DialogModel.vue";
interface DataProps {
  count: number;
  double: number;
  increase: () => void;
  numbers: number[];
  person: { name?: string };
}
export default {
  components: { DialogModel },
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

    const greetings = ref("");
    const updateGreeting = () => {
      greetings.value += "hello!";
    };
    watch([greetings, data], (newValue, oldValue) => {
      console.log(newValue);
      console.log(oldValue);

      document.title = "updated" + greetings.value + data.count;
    });

    const toRefData = toRefs(data);
    const { x, y } = useMounsePosition();
    const { result, loading, error, loaded } = useURLLoader(
      "https://dog.ceo/api/breeds/image/random"
    );

    const modalIsOpen = ref(false);
    const openModal = () => {
      modalIsOpen.value = true;
    };
    const closeDialogModal = () => {
      modalIsOpen.value = false;
    };

    return {
      ...toRefData,
      greetings,
      updateGreeting,
      x,
      y,
      result,
      loading,
      error,
      loaded,
      modalIsOpen,
      openModal,
      closeDialogModal,
    };
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
