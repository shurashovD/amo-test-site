<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import { WarningOutlined } from '@ant-design/icons-vue'

const { onSearch } = defineProps<{ onSearch(query?: string): void }>()

const value = ref<string>('')
const showWarning = computed(() => value.value.length < 3)

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
    <a-tooltip placement="bottomRight">
      <template #title>
        <span>Работает от 3 символов</span>
      </template>
      <WarningOutlined v-if="showWarning" style="color: rgb(250, 140, 22); font-size: 16px" />
    </a-tooltip>
    <a-input-search v-model:value="value" placeholder="Поиск" style="width: 240px" />
  </a-space>
</template>
