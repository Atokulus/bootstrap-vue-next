<template>
  <component :is="computedTag" class="badge" :class="computedClasses" v-bind="computedLinkProps">
    <slot />
  </component>
</template>

<script setup lang="ts">
import {isLink, pick} from '../../utils'
import {useBooleanish} from '../../composables'
import {computed} from 'vue'
import type {BLinkProps, Booleanish} from '../../types'
import BLink from '../BLink/BLink.vue'

defineSlots<{
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  default?: (props: Record<string, never>) => any
}>()

const props = withDefaults(
  defineProps<
    {
      pill?: Booleanish
      tag?: string
      textIndicator?: Booleanish
      dotIndicator?: Booleanish
    } & Omit<BLinkProps, 'event' | 'routerTag'>
  >(),
  {
    pill: false,
    tag: 'span',
    textIndicator: false,
    dotIndicator: false,
    variant: 'secondary',
    // Link props
    active: undefined,
    activeClass: undefined,
    append: false,
    disabled: false,
    href: undefined,
    // noPrefetch: {type: [Boolean, String] as PropType<Booleanish>, default: false},
    // prefetch: {type: [Boolean, String] as PropType<Booleanish>, default: null},
    rel: undefined,
    replace: false,
    routerComponentName: 'router-link',
    target: '_self',
    to: undefined,
    opacity: undefined,
    opacityHover: undefined,
    underlineVariant: null,
    underlineOffset: undefined,
    underlineOffsetHover: undefined,
    underlineOpacity: undefined,
    underlineOpacityHover: undefined,
    icon: false,
    // End link props
  }
)

const pillBoolean = useBooleanish(() => props.pill)
const textIndicatorBoolean = useBooleanish(() => props.textIndicator)
const dotIndicatorBoolean = useBooleanish(() => props.dotIndicator)
const activeBoolean = useBooleanish(() => props.active)
const disabledBoolean = useBooleanish(() => props.disabled)

const computedLink = computed<boolean>(() => isLink(props))

const computedTag = computed<string | typeof BLink>(() => (computedLink.value ? BLink : props.tag))

const computedClasses = computed(() => ({
  [`bg-${props.variant}`]: props.variant !== null,
  'active': activeBoolean.value,
  'disabled': disabledBoolean.value,
  'text-dark': props.variant !== null && ['warning', 'info', 'light'].includes(props.variant),
  'rounded-pill': pillBoolean.value,
  'position-absolute top-0 start-100 translate-middle':
    textIndicatorBoolean.value || dotIndicatorBoolean.value,
  'p-2 border border-light rounded-circle': dotIndicatorBoolean.value,
  'text-decoration-none': computedLink.value,
}))

const computedLinkProps = computed(() =>
  computedLink.value
    ? pick(props, [
        'active',
        'activeClass',
        'append',
        'disabled',
        'href',
        'rel',
        'replace',
        'routerComponentName',
        'target',
        'to',
        'variant',
        'opacity',
        'opacityHover',
        'underlineVariant',
        'underlineOffset',
        'underlineOffsetHover',
        'underlineOpacity',
        'underlineOpacityHover',
        'icon',
      ])
    : {}
)
</script>
