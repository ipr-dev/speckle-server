<template>
  <FormSelectBase
    v-model="selectedValue"
    :items="roles"
    :multiple="multiple"
    :disabled-item-predicate="disabledItemPredicate"
    name="serverRoles"
    label="Server roles"
    class="min-w-[110px]"
    :fully-control-value="fullyControlValue"
  >
    <template #nothing-selected>
      {{ multiple ? 'Select roles' : 'Select role' }}
    </template>
    <template #something-selected="{ value }">
      <template v-if="isMultiItemArrayValue(value)">
        <div ref="elementToWatchForChanges" class="flex items-center space-x-0.5">
          <div
            ref="itemContainer"
            class="flex flex-wrap overflow-hidden space-x-0.5 h-6"
          >
            <div v-for="(item, i) in value" :key="item" class="text-foreground">
              {{ RoleInfo.Server[item] + (i < value.length - 1 ? ', ' : '') }}
            </div>
          </div>
          <div v-if="hiddenSelectedItemCount > 0" class="text-foreground-2 normal">
            +{{ hiddenSelectedItemCount }}
          </div>
        </div>
      </template>
      <template v-else>
        <div class="truncate text-foreground">
          {{ RoleInfo.Server[firstItem(value)] }}
        </div>
      </template>
    </template>
    <template #option="{ item }">
      <div class="flex items-center">
        <span class="truncate">{{ RoleInfo.Server[firstItem(item)] }}</span>
      </div>
    </template>
  </FormSelectBase>
</template>
<script setup lang="ts">
import { Nullable, Roles, ServerRoles, RoleInfo } from '@speckle/shared'
import { useFormSelectChildInternals } from '@speckle/ui-components'
import { PropType } from 'vue'

type ValueType = ServerRoles | ServerRoles[] | undefined

const emit = defineEmits<{
  (e: 'update:modelValue', v: ValueType): void
}>()

const props = defineProps({
  multiple: Boolean,
  modelValue: {
    type: [String, Array] as PropType<ValueType>,
    default: undefined
  },
  allowGuest: Boolean,
  allowAdmin: Boolean,
  allowArchived: Boolean,
  fullyControlValue: Boolean
})

const elementToWatchForChanges = ref(null as Nullable<HTMLElement>)
const itemContainer = ref(null as Nullable<HTMLElement>)

const { selectedValue, isMultiItemArrayValue, hiddenSelectedItemCount, firstItem } =
  useFormSelectChildInternals<ServerRoles>({
    props: toRefs(props),
    emit,
    dynamicVisibility: { elementToWatchForChanges, itemContainer }
  })

const roles = computed(() =>
  Object.values(Roles.Server).filter((r) => {
    if (r === Roles.Server.Admin) return props.allowAdmin
    if (r === Roles.Server.ArchivedUser) return props.allowArchived
    return true
  })
)

const disabledItemPredicate = (item: ServerRoles) => {
  if (item === Roles.Server.Guest) return !props.allowGuest
  return false
}
</script>
