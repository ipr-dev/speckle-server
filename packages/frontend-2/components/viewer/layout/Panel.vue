<template>
  <div class="bg-foundation sm:rounded-lg overflow-hidden shadow">
    <div class="sticky top-0 z-50 flex flex-col bg-foundation shadow-md">
      <div v-if="!hideClose" class="absolute top-2 right-2 sm:right-0 z-10">
        <FormButton size="sm" color="secondary" text @click="$emit('close')">
          <XMarkIcon class="h-4 w-4 sm:h-3 sm:w-3 text-primary sm:text-foreground" />
        </FormButton>
      </div>
      <div
        v-if="$slots.title"
        class="flex items-center h-10 px-3 border-b border-outline dark:border-foundation-2 bg-foundation rounded-t"
      >
        <div
          class="flex items-center h-full w-full pr-8 font-semibold sm:font-bold text-sm text-primary"
        >
          <span class="truncate">
            <slot name="title"></slot>
          </span>
        </div>
      </div>
      <div v-if="$slots.actions" class="flex items-center h-8 sm:h-10 gap-2 px-2">
        <slot name="actions"></slot>
      </div>
    </div>
    <div>
      <slot></slot>
    </div>
  </div>
</template>
<script setup lang="ts">
import { XMarkIcon } from '@heroicons/vue/24/solid'
defineEmits(['close'])
defineProps({
  hideClose: {
    type: Boolean,
    default: false
  }
})
</script>
