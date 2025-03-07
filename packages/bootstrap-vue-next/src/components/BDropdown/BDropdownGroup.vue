<template>
  <li role="presentation">
    <component
      :is="headerTag"
      :id="headerId"
      class="dropdown-header"
      :class="computedClasses"
      :role="headerRole"
    >
      <slot name="header">
        {{ header }}
      </slot>
    </component>
    <ul
      :id="id"
      role="group"
      class="list-unstyled"
      v-bind="$attrs"
      :aria-describedby="ariaDescribedby || headerId"
    >
      <slot />
    </ul>
  </li>
</template>

<script setup lang="ts">
import type {ClassValue, ColorVariant} from '../../types'
import {computed} from 'vue'

defineOptions({
  inheritAttrs: false,
})

const props = withDefaults(
  defineProps<{
    id?: string
    ariaDescribedby?: string
    header?: string
    headerClass?: ClassValue
    headerTag?: string
    headerVariant?: ColorVariant | null
  }>(),
  {
    headerTag: 'header',
    id: undefined,
    ariaDescribedby: undefined,
    header: undefined,
    headerClass: undefined,
    headerVariant: null,
  }
)

defineSlots<{
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  default?: (props: Record<string, never>) => any
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  header?: (props: Record<string, never>) => any
}>()

const headerId = computed<string | undefined>(() =>
  props.id ? `${props.id}_group_dd_header` : undefined
)

const headerRole = computed<'heading' | undefined>(() =>
  props.headerTag === 'header' ? undefined : 'heading'
)

const computedClasses = computed(() => [
  props.headerClass,
  {
    [`text-${props.headerVariant}`]: props.headerVariant !== null,
  },
])
</script>
