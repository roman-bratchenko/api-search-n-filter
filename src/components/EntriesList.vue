<script setup>
  defineProps(['entries'])
</script>

<template>
  <ul class="entries-list">
    <li class="entries-list-item" v-for="{ API, Description, Auth, HTTPS, Cors, Link, Category } in entries">
      <h2>{{ API }}</h2>
      <div class="entries-list__category">{{ Category }}</div>
      <p>{{ Description }}</p>
      <a class="entries-list__link" :href="Link" target="_blank">{{ Link }}</a>
      <ul class="entries-list__props">
        <li v-if="Auth">Auth: {{Auth}}</li>
        <li>HTTPS: {{HTTPS}}</li>
        <li>Cors: {{Cors}}</li>
      </ul>
    </li>
  </ul>
</template>

<style scoped>
  .entries-list {
    list-style: none;
    margin: 0;
    padding: 0;
  }
  .entries-list-item {
    margin-bottom: 1.5em;
  }
  .entries-list__category {
    color: rgb(14, 156, 47);
    font-size: 0.9em;
  }
  .entries-list__props {
    padding-left: 1.25em;
  }
  .entries-list__link {
    text-overflow: ellipsis;
    overflow: hidden;
    display: block;
  }
</style>