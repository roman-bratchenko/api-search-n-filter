<script setup>
import { ref, computed, watch } from 'vue'
import CategoryFilter from './components/CategoryFilter.vue'
import EntriesList from './components/EntriesList.vue'

const fetchError = ref(null)
const data = ref(null)
const entriesData = ref([])
const categoriesChecked = ref([])
const query = ref('')

async function fetchData() {
  try {
    const response = await fetch(
      `https://api.publicapis.org/entries`
    )
    data.value = await response.json()
    entriesData.value = data.value.entries
  } catch (reason) {
    fetchError.value = reason
  }
}

fetchData()

const entriesFiltered = computed(() => {
  const checkedCategories = categoriesFilteredByQuery.value.filter(entry => entry.checked)
  if (checkedCategories.length === 0) {
    return entriesFilteredByQuery.value
  }
  
  return entriesFilteredByQuery.value.filter(entry => {
    return checkedCategories.filter(category => category.name === entry.Category).length > 0
  })
})

const entriesFilteredByQuery = computed(() => {
  if (query.value !== '') {
    return entriesData.value.filter(entry => {
      const entryTitle = entry.API.toLowerCase()
      const queryValue = query.value.toLowerCase()
      return entryTitle.includes(`${queryValue}`)
    })
  }
  return entriesData.value
})

const categoriesFilteredByQuery = computed(() => {
  const allCategories = [];
  entriesFilteredByQuery.value.map(item => allCategories.push(item.Category))

  const uniqueCategories = [...new Set(allCategories)]
  return uniqueCategories.map(category => ({name: category, checked: categoriesChecked.value.indexOf(category) > -1}))
})

const updateCheckedCategories = category => {
  const { name, checked } = category
  if (checked) {
    categoriesChecked.value.push(name)
  } else {
    categoriesChecked.value = categoriesChecked.value.filter(category => category !== name)
  }
}

const searchInput = ref(null)

watch(searchInput, () => {
  searchInput.value.focus()
})

</script>

<template>
  <p class="preloader" v-if="!data">Waiting for data...</p>
  <p class="preloader error" v-else-if="fetchError">{{fetchError}}</p>
  <main v-else>
    <div class="entries-filter-query">
      <input type="search" placeholder="Search query" v-model="query" ref="searchInput" />
    </div>
    <div  class="entries-container">
      <div class="entries-main">
        <EntriesList :entries="entriesFiltered" />
      </div>
      <aside class="entries-aside">
        <CategoryFilter
          :categories="categoriesFilteredByQuery"
          @update="updateCheckedCategories"
        />
      </aside>
    </div>
  </main>
</template>

<style>
@import '@/assets/base.css';
.preloader {
  height: 85px;
  border-bottom: 1px solid rgba(0,0,0,0.1);
  display: flex;
  justify-content: center;
  align-items: center;
}
.entries-container {
  max-width: 800px;
  display: flex;
  align-items: stretch;
  flex-direction: row;
  margin-left: auto;
  margin-right: auto;
}
.entries-main {
  flex: 1;
  max-width: 500px;
  padding: 1.5em;
}
.entries-aside {
  flex: 0.25;
  max-width: 300px;
  border-left: 1px solid rgba(0,0,0,0.1);
  padding-left: 1.5em;
  min-height: calc(100vh - 86px);
}
.entries-filter-query {
  padding: 1.5em;
  border-bottom: 1px solid rgba(0,0,0,0.1);
  display: flex;
  justify-content: center;
}
.entries-filter-query > input {
  width: 100%;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
  border: 1px solid rgba(0,0,0,0.1);
  height: 40px;
  border-radius: 20px;
  padding: 0 20px;
}
</style>
