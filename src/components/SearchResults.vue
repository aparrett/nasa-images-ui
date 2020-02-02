<template>
  <div>
    <ul class="search-results">
      <li v-for="result in results" :key="result.nasa_id">
        <h3>{{ result.title }}</h3>
        <img :src="result.href" />
        <p>{{ result.date }}</p>
        <p>{{ result.description }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["results"]
};
</script>

<style scoped>
.search-results li {
  list-style-type: none;
}
.search-results img {
  max-width: 300px;
  max-height: 300px;
}
</style>
