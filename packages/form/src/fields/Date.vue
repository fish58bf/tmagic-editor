<template>
  <TMagicDatePicker
    v-model="model[name]"
    type="date"
    :size="size"
    :placeholder="config.placeholder"
    :disabled="disabled"
    :format="config.format || 'YYYY/MM/DD'"
    :value-format="config.valueFormat || 'YYYY/MM/DD'"
    @change="changeHandler"
  ></TMagicDatePicker>
</template>

<script lang="ts" setup>
import { TMagicDatePicker } from '@tmagic/design';

import type { DateConfig, FieldProps } from '../schema';
import { datetimeFormatter } from '../utils/form';
import { useAddField } from '../utils/useAddField';

defineOptions({
  name: 'MFormDate',
});

const props = defineProps<FieldProps<DateConfig>>();

const emit = defineEmits<{
  change: [value: string];
}>();

useAddField(props.prop);

props.model[props.name] = datetimeFormatter(props.model[props.name], '', props.config.valueFormat || 'YYYY/MM/DD');

const changeHandler = (v: string) => {
  emit('change', v);
};
</script>
