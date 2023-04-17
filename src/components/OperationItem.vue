<script setup>
import { ref } from 'vue'
import _ from 'lodash'

const ChineseOP = { '+': '＋', '-': '－', '*': '×', '/': '÷'};

defineProps({
  operation: {
    v1: Number,
    v2: Number,
    v3: Number,
    operator: String,
    isCommutative: Boolean,
  }
})

</script>

<template>
  <div class="font-serif">
    <span class="px-4 py-1 bordered border border-black" v-if="_.isUndefined(operation.v1)"></span>
    <span v-else>{{ operation.v1 }}</span>
    <span >{{ ChineseOP[operation.operator] }}</span>
    <span class="px-4 py-1 bordered border border-black" v-if="_.isUndefined(operation.v2)"></span>
    <span v-else>{{ operation.v2 }}</span>
    <span >＝</span>
    <span class="px-4 py-1 bordered border border-black" v-if="_.isUndefined(operation.v3)"></span>
    <span v-else>{{ operation.v3 }}</span>
  </div>
</template>