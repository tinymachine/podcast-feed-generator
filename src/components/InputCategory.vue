<template>
  <label>
    Apple Podcast Category:
    <select
      v-model="selectedOption"
      @input="$emit('input', $event.target.value)"
      style="
        max-width: 100%;
        text-align: left;
      "
    >
      <template v-for="(category, index) in categories">
        <option
          :key="index"
          :value="category.name"
        >
          {{ category.name.toUpperCase() }}
        </option>

        <option
          v-for="(subcategory, subindex) in category.subcategories"
          :key="index + '-' + subindex"
          :value="category.name + '|' + subcategory"
        >
          &nbsp;&nbsp;&nbsp;&middot; {{ subcategory }}
        </option>
      </template>
    </select>
  </label>
</template>

<script>
import Categories from '../data/apple-podcast-categories.json'

export default {
  props: {
    value: { type: String, required: true }
  },
  data() {
    return {
      categories: Categories,
      selectedOption: null
    }
  },
  watch: {
    value: {
      handler: function (val, oldVal) {
        this.selectedOption = val
      },
      immediate: true
    }
  }
}
</script>
