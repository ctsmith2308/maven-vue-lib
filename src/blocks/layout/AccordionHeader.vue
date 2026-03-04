<script setup lang="ts">
import { inject } from 'vue';
import Icon from '../icon/Icon.vue';

const panel = inject<{
  isOpen: () => boolean;
  toggle: () => void;
  disabled: boolean;
}>('accordionPanel');
</script>

<template>
  <button
    type="button"
    :class="[
      'w-full flex items-center justify-between px-4 py-3 text-left transition-colors cursor-pointer hover:bg-surface-alt',
      {
        'opacity-50 cursor-not-allowed': panel?.disabled,
        'bg-surface-alt': panel?.isOpen(),
      },
    ]"
    :disabled="panel?.disabled"
    :aria-expanded="panel?.isOpen()"
    @click="panel?.toggle()"
  >
    <slot />

    <span
      :class="['transition-transform duration-200 text-content-text-secondary ml-auto', { 'rotate-180': panel?.isOpen() }]"
    >
      <Icon iconType="ChevronDownIcon" />
    </span>
  </button>
</template>
