<template>
  <div>
    <div>
      <input
        type="text"
        v-model="searchText"
        @keyup.enter="submitSearch(searchText)"
      />
      <button @click="submitSearch(searchText)">Search</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "search",
  data: function() {
    return {
      searchText: ""
    };
  },
  props: ["submitSearch"]
};
</script>

<style scoped>
input,
button {
  font-size: 16px;
}
</style>
