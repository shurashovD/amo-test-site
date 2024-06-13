<script setup lang="ts">
import type { Lead } from '@/types'
import { MailOutlined, PhoneOutlined, UserOutlined } from '@ant-design/icons-vue'
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
    <a href="tel:{{lead.contacts[0].phone}}">
      <PhoneOutlined />
    </a>
    <a-divider type="vertical" v-if="showMailPhoneDivider" />
    <a href="mailto:{{lead.contacts[0].email}}">
      <MailOutlined />
    </a>
    <span v-if="showEmpty">Нет контактов</span>
  </a-space>
</template>
