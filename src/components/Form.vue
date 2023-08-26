<script setup lang="ts">

interface Props {
  schema?: object;
  model?: object;
  options?: any;
  default?: any;
  onSubmit?: () => void;
}

const props = defineProps<Props>();

const emit = defineEmits({
  submit: () => true
});

const onSubmit = () => {
  emit('submit');
}

</script>

<template>
  <FormKit
    type="form"
    :schema="props.schema"
    :model="props.model"
    :options="props.options"
    :default="props.default"
    @submit="onSubmit"
  >
    <slot />
  </FormKit>
</template>
