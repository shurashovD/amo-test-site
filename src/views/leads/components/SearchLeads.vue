<script setup lang="ts">
import { ref, watch } from 'vue'
import { WarningOutlined } from '@ant-design/icons-vue'

const { onSearch } = defineProps<{ onSearch(query?: string): void }>()

console.log(onSearch)

const value = ref<string>('')

let timeoutId: ReturnType<typeof setTimeout> | null = null

watch(value, () => {
  if (timeoutId) {
    clearTimeout(timeoutId)
  }

  timeoutId = setTimeout(() => {
    onSearch(value.value.length > 2 ? value.value : undefined)
  }, 800)
})
</script>

<template>
  <a-space style="align-items: center">
    <WarningOutlined style="color: rgb(250, 140, 22); font-size: 16px" />
    <a-input-search v-model:value="value" placeholder="Поиск" style="width: 240px" />
  </a-space>
</template>
