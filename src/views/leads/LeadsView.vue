<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import type { TableColumnType } from 'ant-design-vue'

import type { Lead } from '@/types'

import LeadDetailsRow from './components/LeadDetailsRow.vue'
import SearchLeads from './components/SearchLeads.vue'

const getData = async (query?: string) => {
  isLoading.value = true
  try {
    const queryParams = query && !!query?.length ? `?query=${query}` : ''
    const url = '/api/leads' + queryParams
    data.value = await fetch(url).then((res) => res.json())
  } catch (e) {
    isError.value = true
  } finally {
    isLoading.value = false
  }
}

const data = ref<Lead[]>([])

const isLoading = ref<boolean>(false)
const isError = ref<boolean>(false)
const dataSource = computed(() => data.value.map((item) => ({ ...item, key: item.id })))
const { format } = new Intl.DateTimeFormat('ru', {
  year: 'numeric',
  month: 'short',
  day: 'numeric',
  hour: 'numeric',
  minute: '2-digit'
})

const columns: Array<TableColumnType> = [
  {
    customRender: () => {},
    title: '',
    dataIndex: 'contacts',
    key: 'contacts'
  },
  {
    title: 'Название',
    dataIndex: 'name',
    key: 'name'
  },
  {
    title: 'Бюджет',
    dataIndex: 'price',
    key: 'age'
  },
  {
    customRender: (opt) => {
      const value: Lead['status'] = opt.value
      return value.name
    },
    title: 'Статус',
    dataIndex: 'status',
    key: 'status'
  },
  {
    customRender: (opt) => {
      const value: Lead['user'] = opt.value
      return value.name
    },
    title: 'Ответственный',
    dataIndex: 'user',
    key: 'user'
  },
  {
    customRender: ({ value }) => format(value * 1000),
    title: 'Дата создания',
    dataIndex: 'created_at',
    key: 'created_at'
  }
]

onMounted(getData)
</script>

<template>
  <a-layout class="layout">
    <a-layout-content style="padding: 50px; min-height: 100vh">
      <a-card :bordered="false">
        <template #title>
          <a-space style="justify-content: space-between; width: 100%">
            <span>Шурашов тестовое</span>
            <SearchLeads :on-search="getData" />
          </a-space>
        </template>
        <a-table
          :columns="columns"
          :loading="isLoading"
          :dataSource="dataSource"
          :pagination="false"
        >
          <template #bodyCell="{ column, record }">
            <template v-if="column.key === 'status'">
              <span>
                <a-tag :bordered="false" :color="record.status.color">{{
                  record.status.name
                }}</a-tag>
              </span>
            </template>
          </template>
          <template #expandedRowRender="{ record }">
            <p style="margin: 0; padding-left: 64px">
              <LeadDetailsRow v-bind="record" />
            </p>
          </template>
        </a-table>
      </a-card>
    </a-layout-content>
  </a-layout>
</template>
