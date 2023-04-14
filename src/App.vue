<script setup>
import { computed } from "@vue/runtime-core";
import _ from "lodash"

function rand(min, max) {
    return Math.round(Math.random() * (max - min)) + min;
}

function genMinusIn20() {
  const num1 = rand(6,20)
  const num2 = rand(0, num1)

  return `${num1}-${num2}=`
}

function genAddIn20() {
  const num1 = rand(6,20)
  const num2 = rand(0, num1)
  const num3 = num1 - num2

  return `${num2}+${num3}=`
}

function genAddMinusInDecade() {
  const num1 = rand(3,9)*10
  const num2 = rand(1, 9)
  const num3 = num1 + num2

  const dice = Math.random()
  let ret;
  switch (true) {
    case (dice < 1/3):
      ret = `${num1}+${num2}=`
      break
    case (dice < 1/3):
      ret = `${num3}-${num1}=`
      break
    default:
      ret = `${num3}-${num2}=`
      break
  }

  return ret
}

const formulas = computed(()=>{
  const data = []
  while (data.length < 20) {
    const dice = Math.random()
    let str;
    switch (true) {
      case (dice < 3/20):
        str = genAddIn20()
        break
      case (dice < 6/20):
        str = genAddMinusInDecade()
        break
      default:
        str = genMinusIn20()
        break
    }
    if (!_.includes(data, str)) {
      data.push(str)
    }
  }
  return data
})
</script>
<template>
  <div v-for="index in [1,2,3,4,5,6]" :key="index" class="print:block relative rounded-xl overflow-auto p-8" :class="{ hidden: index>1 }">
    <h1 class="py-8 px-16 text-lg font-bold text-center"> 日期：_____月_____日 用时:_____ 对题：______道（第{{index}}次）</h1>
    <div class="px-16 grid grid-cols-4 gap-3 sm:text-sm tracking-widest print:text-black text-black text-lg bg-stripes-pink rounded-lg">
      <div v-for="formula in formulas" :key="formula">
        {{formula}}<span class="border-solid border-black border px-3"></span>
      </div>
    </div>
  </div>
</template>