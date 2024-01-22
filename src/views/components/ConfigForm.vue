<script setup lang="ts">
import { Form, FormItem, Button, type FormInstance } from 'ant-design-vue'
import { FieldType, bitable } from '@lark-base-open/js-sdk';
import { ref, toRefs } from 'vue';
import TableSelect from '@/components/TableSelect.vue'
import FieldSelect from '@/components/FieldSelect.vue';
import type { Rule } from 'ant-design-vue/es/form';

const formRef = ref<FormInstance>()
const form = ref<{
  tableId: string | undefined,
  titleFieldId: string | undefined,
  startTimeFieldId: string | undefined,
  endTimeFieldId: string | undefined,
}>({
  tableId: undefined,
  titleFieldId: undefined,
  startTimeFieldId: undefined,
  endTimeFieldId: undefined,
})
const formRules: Record<string, Rule[]> = {
  tableId: [
    { required: true, message: '请选择表格' },
  ],
  titleFieldId: [
    { required: true, message: '请选择标题字段' },
  ],
  startTimeFieldId: [
    { required: true, message: '请选择开始时间字段' },
  ],
  endTimeFieldId: [
    { required: false, message: '请选择结束时间字段' },
  ],
}

const { tableId, titleFieldId, endTimeFieldId, startTimeFieldId } = toRefs(form.value)
bitable.base.getActiveTable().then(async (table) => {
  const id = table.id
  tableId.value = id
})

const loading = ref(false)
const onSubmit = async () => {
  await formRef.value?.validate()
  loading.value = false
}

</script>
<template>
  <Form :layout="'vertical'" :model="form" :rules="formRules" ref="formRef">
    <FormItem label="表格" name="tableId">
      <TableSelect v-model:table-id="form.tableId"></TableSelect>
    </FormItem>
    <FormItem label="标题字段" name="titleFieldId">
      <FieldSelect v-model:table-id="form.tableId" v-model:field-id="form.titleFieldId" :field-type-list="[FieldType.Text]" placeholder="只支持文本字段">
      </FieldSelect>
    </FormItem>
    <FormItem label="开始时间字段" name="startTimeFieldId">
      <FieldSelect v-model:table-id="form.tableId" v-model:field-id="form.startTimeFieldId" :field-type-list="[FieldType.DateTime]" placeholder="只支持时间字段">
      </FieldSelect>
    </FormItem>
    <FormItem label="结束时间字段" name="endTimeFieldId">
      <FieldSelect v-model:table-id="form.tableId" v-model:field-id="form.endTimeFieldId"
        :field-type-list="[FieldType.DateTime]" placeholder="只支持时间字段" allow-clear></FieldSelect>
    </FormItem>
    <FormItem>
      <div class="flex flex-row items-center">
        <Button type="primary" @click="onSubmit" :loading="loading">保存</Button>
        <div class="text-sm text-gray-400 ml-3"></div>
      </div>
    </FormItem>
  </Form>
</template>
<style scoped></style>