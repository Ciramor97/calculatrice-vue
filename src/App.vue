<template>
  <div class="w-screen h-screen flex justify-center items-center">
    <div class="w-[500px] px-2 pt-4 pb-1 bg-black border h-2/3 flex flex-col">
      <div
        class="h-1/4 border-b-2 border-white text-white flex justify-end items-end p-3"
      >
        {{ data.result }}
      </div>
      <div class="grid grid-cols-4 gap-3 p-3" style="height: 100%">
        <div
          v-for="(item, key) in data.values"
          :key="key"
          class="bg-white flex justify-center items-center rounded-sm"
          :class="{ 'col-span-2': item == 0 }"
        >
          <button
            class="w-full h-full rounded-sm"
            :class="{
              'bg-yellow-600': data.operations.includes(String(item)),
            }"
            @click="calcFunction"
          >
            {{ item }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";

const data = reactive({
  result: "",
  lastResult: "",
  printResult: false,
  operations: ["+", "-", "/", "*", "="],
  values: [
    "AC",
    "CE",
    "%",
    "/",
    7,
    8,
    9,
    "*",
    4,
    5,
    6,
    "-",
    1,
    2,
    3,
    "+",
    0,
    ".",
    "=",
  ],
});

function calcFunction(e: any) {
  let value = e.target.innerHTML;
  switch (value) {
    case "AC":
      data.lastResult = "";
      break;
    case "CE":
      data.lastResult = data.lastResult.slice(0, data.lastResult.length - 1);
      break;
    case "=":
      data.lastResult = eval(data.lastResult.slice(0, data.lastResult.length));
      data.printResult = true;
      break;
    default:
      if (!data.printResult) data.lastResult += value;
      else if (data.operations.includes(value)) {
        data.lastResult += value;
        data.printResult = false;
      } else {
        data.lastResult = "";
        data.result = "";
        data.printResult = false;
      }
      console.log("other", data.result);
      break;
  }
  data.result = data.lastResult + "";
}
</script>

<style scoped></style>
