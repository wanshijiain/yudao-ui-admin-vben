<script lang="ts" setup>
import { Divider } from 'ant-design-vue'
import { columns } from './data'
import type { EditRecordRow } from '@/components/Table'
import { BasicTable, useTable } from '@/components/Table'
import type { CodegenColumnVO } from '@/api/infra/codegen/types'

defineProps({
  columnsInfo: {
    type: Array as PropType<CodegenColumnVO[]>,
    default: () => null,
  },
})

const emit = defineEmits(['next', 'prev'])

const [registerTable, { getDataSource }] = useTable({
  columns,
  maxHeight: 700,
  pagination: false,
  useSearchForm: false,
  showTableSetting: false,
  showIndexColumn: false,
})

async function customResetFunc() {
  emit('prev')
}

async function customSubmitFunc() {
  const tableValue = getDataSource()
  emit('next', tableValue)
}

function handleEdit(record: EditRecordRow) {
  record.onEdit?.(true)
}
</script>

<template>
  <div class="step2">
    <div class="mx-auto my-0 w-full">
      <BasicTable :data-source="columnsInfo" @register="registerTable" @row-click="handleEdit" />
    </div>
    <Divider />
    <div class="flex justify-center">
      <a-button @click="customResetFunc">
        上一步
      </a-button>
      <a-button type="primary" @click="customSubmitFunc">
        提交
      </a-button>
    </div>
    <h3 class="mb-3 text-base">
      说明
    </h3>
    <h4 class="mb-1 text-sm">
      配置字段
    </h4>
    <p> 配置表的字段类型，增删改查，字典等 </p>
  </div>
</template>
