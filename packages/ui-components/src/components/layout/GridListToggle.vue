<template>
  <button
    class="flex items-center justify-center rounded bg-foundation h-8 w-8 shadow cursor-pointer text-foreground"
    @click="onClick"
  >
    <Component :is="currentIcon" class="h-5 w-5" />
  </button>
</template>
<script setup lang="ts">
import { Bars3Icon } from '@heroicons/vue/24/solid'
import { Squares2X2Icon } from '@heroicons/vue/24/outline'
import { computed } from 'vue'
import { GridListToggleValue } from '~~/src/helpers/layout/components'

const emit = defineEmits<{
  (e: 'click', v: MouseEvent): void
  (e: 'update:modelValue', v: GridListToggleValue): void
}>()

const props = defineProps<{
  modelValue?: GridListToggleValue
}>()

const value = computed({
  get: () => props.modelValue || GridListToggleValue.Grid,
  set: (newVal) => emit('update:modelValue', newVal)
})

const currentIcon = computed(() =>
  value.value === GridListToggleValue.Grid ? Bars3Icon : Squares2X2Icon
)

const onClick = (e: MouseEvent) => {
  emit('click', e)

  const newVal =
    value.value === GridListToggleValue.Grid
      ? GridListToggleValue.List
      : GridListToggleValue.Grid
  value.value = newVal
}
</script>
