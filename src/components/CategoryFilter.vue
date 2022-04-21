<script setup>
  defineProps(['categories'])
</script>

<template>
  <ul class="entries-filter-categories">
    <li v-for="(category, index) in categories" :key="index"  class="entries-filter-categories-item">
      <label>
        <input
          type="checkbox"
          :value="category.name"
          v-model="category.checked"
          @change="$emit('update', category)"
        />
        <span>{{ category.name }}</span>
      </label>
    </li>
  </ul>
</template>

<style scoped>
.entries-filter-categories {
  list-style: none;
  margin: 0;
  padding: 0;
}
.entries-filter-categories-item {
  margin-bottom: 1em;
}
.entries-filter-categories-item:first-child {
  margin-top: 1.5em;
}
.entries-filter-categories label {
  cursor: pointer;
  display: flex;
  align-items: flex-start;
}
.entries-filter-categories label>input {
  margin-top: 0.15em;
}
.entries-filter-categories label>span {
  margin-left: 0.5em;
  line-height: 1.1;
}
</style>