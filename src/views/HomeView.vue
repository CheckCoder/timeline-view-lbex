<script setup lang="ts">
import { Form, FormItem, RadioGroup, RadioButton, Input, message, Spin, Button, type FormInstance } from 'ant-design-vue'
import { BarcodeOutlined, LoadingOutlined } from '@ant-design/icons-vue';
import { FieldType, bitable, IOpenSegmentType } from '@lark-base-open/js-sdk';
import type { IOpenSegment, ISingleSelectField, IOpenSingleSelect, ITextField } from '@lark-base-open/js-sdk';
import { ref, toRefs } from 'vue';
import TableSelect from '@/components/TableSelect.vue'
import FieldSelect from '@/components/FieldSelect.vue';
import { findRecord } from '@/utils/table'
import type { Rule } from 'ant-design-vue/es/form';

const formRef = ref<FormInstance>()
const form = ref<{
  tableId: string | undefined,
  startTimeFieldId: string | undefined,
  endTimeFieldId: string | undefined,
  focusOnFieldId: string | undefined,
}>({
  tableId: undefined,
  startTimeFieldId: undefined,
  endTimeFieldId: undefined,
  focusOnFieldId: undefined
})
const formRules: Record<string, Rule[]> = {
  tableId: [
    { required: true, message: '请选择表格' },
  ],
  startTimeFieldId: [
    { required: true, message: '请选择开始时间字段' },
  ],
  endTimeFieldId: [
    { required: true, message: '请选择结束时间字段' },
  ],
}

const { tableId, endTimeFieldId, startTimeFieldId } = toRefs(form.value)
bitable.base.getActiveTable().then(async (table) => {
  const id = table.id
  tableId.value = id
  table.getFieldMetaList().then((res) => {
    console.log(res)
  })
})

const loading = ref(false)
const onSubmit = async () => {
  await formRef.value?.validate()
  loading.value = false
}

</script>
<template>
  <main class="px-5 pb-5">
    <Form :layout="'vertical'" :model="form" :rules="formRules" ref="formRef">
      <FormItem label="表格" name="tableId">
        <TableSelect v-model:table-id="form.tableId"></TableSelect>
      </FormItem>
      <FormItem label="开始时间字段" name="startTimeFieldId">
        <FieldSelect v-model:table-id="form.tableId" v-model:field-id="form.startTimeFieldId" :field-type-list="[FieldType.DateTime]" placeholder="只支持时间字段">
        </FieldSelect>
      </FormItem>
      <FormItem label="结束时间字段" name="endTimeFieldId">
        <FieldSelect v-model:table-id="form.tableId" v-model:field-id="form.endTimeFieldId"
          :field-type-list="[FieldType.DateTime]" placeholder="只支持时间字段"></FieldSelect>
      </FormItem>
      <FormItem label="专注内容字段" name="focusOnFieldId">
        <FieldSelect v-model:table-id="form.tableId" v-model:field-id="form.focusOnFieldId"
          :field-type-list="[FieldType.Text]" placeholder="只支持文本字段" allow-clear></FieldSelect>
      </FormItem>
      <FormItem>
        <div class="flex flex-row items-center">
          <Button type="primary" @click="onSubmit" :loading="loading">确认</Button>
          <div class="text-sm text-gray-400 ml-3"></div>
        </div>
      </FormItem>
    </Form>
  </main>
  <div class="h-[60px]"></div>
  <div class="flex flex-row text-sm justify-center items-center w-full fixed bottom-0 h-[60px]">
    <Button type="link" href="https://checkcai.feishu.cn/base/QPUcb811SaRw6QsPeUgcIBb6nPd?table=ldx2WPdzOoLYVCHL" target="blank">使用文档</Button>
    <span class="text-gray-400">|</span>
    <Button type="link" href="https://applink.feishu.cn/client/chat/chatter/add_by_link?link_token=446uebce-5505-4ab8-98a0-2919278c7bb6" target="blank">用户交流群</Button>
  </div>
</template>
<style scoped></style>