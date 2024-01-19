<script setup lang="ts">
import { Button } from 'ant-design-vue'

defineProps<{
  docUrl: string
  groupUrl: string
}>()

</script>
<template>
  <div class="h-[60px]"></div>
  <div class="flex flex-row text-sm justify-center items-center w-full fixed bottom-0 h-[60px]">
    <Button type="link" :href="docUrl" target="blank">使用文档</Button>
    <span class="text-gray-400">|</span>
    <Button type="link" :href="groupUrl" target="blank">用户交流群</Button>
  </div>
</template>