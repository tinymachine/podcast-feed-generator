<template>
  <label
    :class="{ 'error' : !value }"
  >
    {{ label }}:

    <slot name="beforeField"/>

    <textarea
      v-if="type === 'textarea'"
      v-bind="$attrs"
      :value="value"
      @input="$emit('input', $event.target.value)"
    />

    <input
      v-else
      :value="value"
      @input="$emit('input', $event.target.value)"
    />

    <slot name="afterField"/>
  </label>
</template>

<script>
export default {
  inheritAttrs: false,
  props: {
    label: { type: String, required: true },
    required: { type: Boolean, default: false },
    type: { type: String, default: 'text' },
    value: { type: String, required: true }
  }
}
</script>

