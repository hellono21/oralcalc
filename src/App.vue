<script setup>
import OperationItem from "./components/OperationItem.vue";
import { computed } from "@vue/runtime-core";
import _ from "lodash"


function rand(min, max) {
    return Math.round(Math.random() * (max - min)) + min;
}

function addition(v1, v2, v3) {
  return {
    v1,
    v2,
    v3,
    operator: '+',
    isCommutative: true
  }
}

function substraction(v1, v2, v3) {
  return {
    v1,
    v2,
    v3,
    operator: '-',
    isCommutative: false
  }
}

function genSubstractionWithin20() {
  const v1 = rand(6,20)
  const v2 = rand(0, v1)
  const v3 = v1 - v2

  return substraction(v1, v2, v3)
}

function genAdditionWithin20() {
  const v1 = rand(6,20)
  const v2 = rand(0, v1)
  const v3 = v1 - v2

  return addition(v2, v3, v1)
}

function genAdd2DigitMinus1Digit(){
  const num1 = rand(20,99)
  const num2 = rand(0, 9)
  const num3 = num1 - num2

  return `${num2}+${num3}=`
}

function gen10sOperate1s() {
  const num1 = rand(3,9)*10
  const num2 = rand(1, 9)
  const num3 = num1 + num2

  const dice = Math.random()
  let ret;
  switch (true) {
    case (dice < 1/3):
      ret = addition(num1, num2, num3)
      break
    case (dice < 2/3):
      ret = substraction(num3, num1, num2)
      break
    default:
      ret = substraction(num3, num2, num1)
      break
  }
  return ret
}

function operation2str(operation) {
  const opr_str = ChineseOP[operation.operator]
  if ( operation.isCommutative && Math.random() < 0.5) {
    const v1 = operation.v1
    operation.v1 = operation.v2
    operation.v2 = v1
  }

  return `${operation.v1}${opr_str}${operation.v2}`
}

const operations = computed(()=>{
  const data = []
  while (data.length < 20) {
    const dice = Math.random()
    let str;
    switch (true) {
      case (dice < 2/20):
        str = gen10sOperate1s()
        break
      case (dice < 5/20):
        str = genAdditionWithin20()
        break
      default:
        //str = operation2str(genAdditionWithin20())
        str = genSubstractionWithin20()
    }
    if (!_.includes(data, str)) {
      data.push(str)
    }
  }

  _.each(data, (operation) => {
    const dice = Math.random()
    if (operation.isCommutative && dice < 0.5){
      const v = operation.v1
      operation.v1 = operation.v2
      operation.v2 = v
      console.log(operation)
    }
    switch ( true ) {
      case ( dice < 1/20):
        delete operation.v1
        break
      case ( dice < 2/20):
        delete operation.v2
        break
      default:
        delete operation.v3
    }
  })

  return data
})
</script>
<template>
  <div v-for="index in [1,2,3,4,5,6]" :key="index" class="print:block relative rounded-xl overflow-auto p-8" :class="{ hidden: index>1 }">
    <h1 class="py-8 px-16 text-lg font-bold text-center"> 日期：_____月_____日 用时:_____ 对题：______道（第{{index}}次）</h1>
    <div class="px-16 grid grid-cols-4 gap-3 sm:text-sm tracking-widest print:text-black text-black text-lg bg-stripes-pink rounded-lg">
      <div class="font-serif" v-for="operation in operations" :key="operation">
        <OperationItem :operation="operation"/>
      </div>
    </div>
  </div>
</template>