<template>
  <div>
    <TMagicSteps :active="active" align-center :space="config.space">
      <TMagicStep
        v-for="(item, index) in config.items"
        :key="item.__key"
        :title="item.title"
        :active="active"
        @click="stepClick(index)"
      ></TMagicStep>
    </TMagicSteps>

    <template v-for="(step, index) in config.items">
      <template v-for="item in step.items">
        <Container
          v-if="item"
          v-show="active - 1 === index"
          :key="(item as Record<string, any>)[mForm?.keyProp || '__key']"
          :config="item"
          :model="step.name ? model[step.name] : model"
          :lastValues="step.name ? lastValues[step.name] : lastValues"
          :is-compare="isCompare"
          :prop="`${step.name}`"
          :size="size"
          :disabled="disabled"
          :label-width="config.labelWidth || labelWidth"
          @change="changeHandler"
          @addDiffCount="onAddDiffCount()"
        ></Container>
      </template>
    </template>
  </div>
</template>

<script setup lang="ts">
import { inject, ref, watchEffect } from 'vue';

import { TMagicStep, TMagicSteps } from '@tmagic/design';

import type { ContainerChangeEventData, FormState, StepConfig } from '../schema';

import Container from './Container.vue';

defineOptions({
  name: 'MFormStep',
});

const props = withDefaults(
  defineProps<{
    model: any;
    name?: string;
    lastValues?: any;
    isCompare?: boolean;
    config: StepConfig;
    stepActive?: number;
    labelWidth?: string;
    size?: string;
    disabled?: boolean;
  }>(),
  {
    stepActive: 1,
  },
);

const emit = defineEmits<{
  change: [v: any, eventData: ContainerChangeEventData];
  addDiffCount: [];
}>();

const mForm = inject<FormState | undefined>('mForm');
const active = ref(1);

watchEffect(() => {
  active.value = props.stepActive;
});

const stepClick = (index: number) => {
  active.value = index + 1;
  mForm?.$emit('update:stepActive', active.value);
};

const changeHandler = (v: any, eventData: ContainerChangeEventData) => {
  emit('change', props.model, eventData);
};

const onAddDiffCount = () => emit('addDiffCount');
</script>
