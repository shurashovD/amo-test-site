<script setup lang="ts">
import type { Lead } from '@/types'
import { UserOutlined } from '@ant-design/icons-vue'
import { computed } from 'vue'

const lead = defineProps<Lead>()
const showMailPhoneDivider = computed(() => !!lead.contacts[0]?.email && !!lead.contacts[0]?.phone)
const showEmpty = computed(() => !lead.contacts.length)
</script>

<template>
  <a-space v-if="lead.contacts.length">
    <span
      ><a-avatar size="small">
        <template #icon><UserOutlined /></template> </a-avatar
    ></span>
    <span>{{ lead.contacts[0].name }}</span>
    <span>{{ lead.contacts[0].phone }}</span>
    <a-divider type="vertical" v-if="showMailPhoneDivider" />
    <span>{{ lead.contacts[0].email }}</span>
    <span v-if="showEmpty">Нет контактов</span>
  </a-space>
</template>
