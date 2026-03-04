<script setup lang="ts">
import { computed, useSlots } from 'vue';

import type { ButtonProps } from './types';

import ButtonSpinner from './ButtonSpinner.vue';

const props = withDefaults(defineProps<ButtonProps>(), {
  color: 'primary',
  size: 'md',
  loading: false,
  disabled: false,
  raised: false,
  rounded: false,
  as: 'button',
  iconPosition: 'left',
});

const slots = useSlots();

const colorKeys = {
  primary: 'bg-brand text-brand-contrast hover:bg-brand-solid-hover',
  secondary: 'bg-secondary text-secondary-contrast hover:bg-secondary-solid-hover',
  success: 'bg-success text-success-contrast hover:bg-success-solid-hover',
  danger: 'bg-danger text-danger-contrast hover:bg-danger-solid-hover',
};

const outlinedKeys = {
  primary: 'bg-transparent border border-brand text-brand hover:bg-brand-ghost-hover',
  secondary: 'bg-transparent border border-secondary text-secondary hover:bg-secondary-ghost-hover',
  success: 'bg-transparent border border-success text-success hover:bg-success-ghost-hover',
  danger: 'bg-transparent border border-danger text-danger hover:bg-danger-ghost-hover',
};

const textKeys = {
  primary: 'bg-transparent border-none text-brand hover:bg-brand-ghost-hover',
  secondary: 'bg-transparent border-none text-secondary hover:bg-secondary-ghost-hover',
  success: 'bg-transparent border-none text-success hover:bg-success-ghost-hover',
  danger: 'bg-transparent border-none text-danger hover:bg-danger-ghost-hover',
};

const sizeKeys = {
  sm: 'h-6 text-sm',
  md: 'h-8 text-sm',
  lg: 'h-10 text-base',
  xl: 'h-12 text-base',
  jumbo: 'h-16 text-lg',
};

// Wrapper span sizing for slotted icons — matches Icon size prop scale
const iconSizeKeys = {
  sm: 'size-4',
  md: 'size-5',
  lg: 'size-6',
  xl: 'size-8',
  jumbo: 'size-8', // no Icon size beyond xl, fallback to xl
};

const colorClass = computed(() => {
  if (props.disabled) return 'bg-disabled-bg text-disabled-text border-disabled-border cursor-not-allowed pointer-events-none';
  if (props.variant === 'ghost') return 'bg-surface border border-surface-border text-content-text hover:border-brand hover:text-brand';
  if (props.variant === 'outlined') return outlinedKeys[props.color];
  if (props.variant === 'text') return textKeys[props.color];
  if (props.variant === 'link') return 'bg-transparent border-none text-content-text hover:underline';
  return colorKeys[props.color];
});

const iconPositionClass = computed(() => (props.iconPosition === 'right' ? 'flex-row-reverse' : 'flex-row'));

const isIconOnly = computed(() => !!slots.icon && !props.label);

const iconWrapperClass = computed(() => iconSizeKeys[props.size]);

const classes = computed(() => [
  'inline-flex items-center justify-center gap-2 rounded-md cursor-pointer transition-colors px-4',
  colorClass.value,
  sizeKeys[props.size],
  isIconOnly.value ? 'aspect-square !p-0' : '',
  props.raised ? 'shadow-md/30' : '',
  props.rounded ? 'rounded-full' : '',
  iconPositionClass.value,
  props.loading ? 'cursor-progress' : '',
]);
</script>

<template>
  <button v-if="props.as === 'button'" :class="classes" :disabled="loading || disabled" type="button">
    <ButtonSpinner v-if="loading" :size="size" color="currentColor" />

    <template v-else>
      <span v-if="slots.icon" :class="['flex items-center justify-center', iconWrapperClass]">
        <slot name="icon" />
      </span>

      <span v-if="label" class="whitespace-nowrap">
        {{ label }}
      </span>
    </template>
  </button>

  <a v-else :href="href" :class="classes" target="_blank">
    <span v-if="slots.icon" :class="['flex items-center justify-center', iconWrapperClass]">
      <slot name="icon" />
    </span>

    <span v-if="label" class="whitespace-nowrap">
      {{ label }}
    </span>
  </a>
</template>
