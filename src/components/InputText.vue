<script setup lang="ts">
interface Props {
  label: string;
  value: string;
  help?: string;
  validation?: string;
}

const props = defineProps<Props>();
</script>

<template>
  <FormKit
    type="text"
    :label="props.label"
    :value="props.value"
    :help="props?.help"
    :validation="props.validation"
  />
</template>