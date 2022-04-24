<script setup lang="ts">
import { ref } from "vue";
import { storeToRefs } from "pinia";
import { useCalculator } from "../stores/calculator";

const store = useCalculator();
const { ans, operator, num } = storeToRefs(store);
const { calculate, addToNumber } = store;
const newTodo = ref("");
const buttons = [
  "C",
  "π",
  "%",
  "⌫",
  "1/x",
  "x²",
  "√x",
  "÷",
  7,
  8,
  9,
  "x",
  4,
  5,
  6,
  "-",
  1,
  2,
  3,
  "+",
  "+/-",
  0,
  ".",
  "=",
];
const handleClick = (button: string | number) => {
  if (typeof button === "number") {
    addToNumber(button);
  } else {
    if (button === "C") {
      ans.value = 0;
      num.value = 0;
      operator.value = "";
    } else if (button === "π") {
      if (operator.value === "") {
        ans.value = 3.142;
      } else {
        num.value = 3.142;
      }
    } else if (button === "%") {
      checkCalculate();
      ans.value = parseFloat((ans.value / 100).toFixed(3));
    } else if (button === "⌫") {
      if (operator.value === "") {
        if (ans.value.toString().length === 1) {
          ans.value = 0;
        } else {
          ans.value = parseFloat(ans.value.toString().slice(0, -1));
        }
      } else if (num.value === 0) {
        operator.value = "";
      } else {
        if (num.value.toString().length === 1) {
          num.value = 0;
        } else {
          num.value = parseFloat(num.value.toString().slice(0, -1));
        }
      }
    } else if (button === "1/x") {
      checkCalculate();
      ans.value = parseFloat((1 / ans.value).toFixed(3));
    } else if (button === "x²") {
      checkCalculate();
      ans.value = parseFloat((ans.value * ans.value).toFixed(3));
    } else if (button === "√x") {
      checkCalculate();
      ans.value = parseFloat(Math.sqrt(ans.value).toFixed(3));
    } else if (button === "÷") {
      checkCalculate();
      operator.value = "divide";
    } else if (button === "x") {
      checkCalculate();
      operator.value = "multiply";
    } else if (button === "-") {
      checkCalculate();
      operator.value = "subtract";
    } else if (button === "+") {
      checkCalculate();
      operator.value = "add";
    } else if (button === "+/-") {
      if (num.value === 0) {
        ans.value = -ans.value;
      } else {
        num.value = -num.value;
      }
    } else if (button === "=") {
      calculate();
    } else if (button === ".") {
      // operator.value = 'dot';
    }
  }
};
const checkCalculate = (): void => {
  if (!(operator.value === "" || num.value === 0)) {
    calculate();
  }
};
const operators: any = {
  add: "+",
  subtract: "-",
  multiply: "x",
  divide: "÷",
};
</script>

<template>
  <div
    id="calculator"
    class="my-auto h-screen overflow-auto bg-purple-300 py-10 text-center"
  >
    <div
      class="mx-auto w-11/12 rounded-lg bg-black p-6 text-xl sm:w-3/5 sm:text-4xl"
    >
      <div class="mb-5 w-full bg-green-600 p-3 text-right font-mono text-black">
        {{ ans.toLocaleString("en-US", { maximumFractionDigits: 3 }) }}
        {{ operators[operator] }}
        {{
          num !== 0
            ? num.toLocaleString("en-US", {
                maximumFractionDigits: 3,
              })
            : ""
        }}
      </div>
      <div class="grid grid-cols-4 gap-4">
        <button
          class="w-full rounded bg-gray-300 py-2 px-4 font-bold text-gray-900 hover:bg-gray-200 hover:text-gray-900"
          :class="{
            'bg-gray-500':
              ((index + 1) % 4 === 0 || index < 7) &&
              button !== '=' &&
              button !== 'C',
            'hover:bg-gray-400':
              ((index + 1) % 4 === 0 || index < 7) &&
              button !== '=' &&
              button !== 'C',
            'bg-red-500': button === 'C',
            'hover:bg-red-400': button === 'C',
            'bg-yellow-600': button === '=',
            'hover:bg-yellow-500': button === '=',
          }"
          v-for="(button, index) in buttons"
          :key="index"
          @click="handleClick(button)"
          type="button"
        >
          {{ button }}
        </button>
      </div>
    </div>
  </div>
</template>
