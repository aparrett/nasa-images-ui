<template>
  <div>
    <h1>Search NASA Images</h1>
    <app-search :submitSearch="submitSearch" />
    <br />
    <label
      >Sort By
      <select name="sortBy" id="sortBy" v-model="sortBy">
        <option
          v-for="option in sortByOptions"
          :value="option.id"
          :key="option.id"
        >
          {{ option.label }}
        </option>
      </select>
    </label>
    <app-search-results :results="results" />
  </div>
</template>

<script>
import Search from "../components/Search";
import SearchResults from "../components/SearchResults";
import axios from "axios";
import moment from "moment";

const apiKey = "DglybVJSp9MFnXUnxbv7QXmNgBSpK4ZCzgF7RWfx";
const url = "https://images-api.nasa.gov/search";

export default {
  data: function() {
    return {
      sortByOptions: [
        { id: "dateNewest", label: "Date: Newest to Oldest" },
        { id: "dateOldest", label: "Date: Oldest to Newest" }
      ],
      results: [],
      sortBy: "dateNewest"
    };
  },
  components: {
    "app-search": Search,
    "app-search-results": SearchResults
  },
  methods: {
    submitSearch: async function(searchText) {
      const {
        data: {
          collection: { items }
        }
      } = await axios.get(`${url}?q=${searchText}`, {
        api_key: apiKey
      });

      this.results = items.map(item => {
        return {
          href: item.links[0].href,
          ...item.data[0],
          date: new moment(item.data[0].date_created).format("LL")
        };
      });

      this.sortResults(this.sortBy);
    },
    sortResults: function(val) {
      this.results.sort((a, b) => {
        if (val === "dateNewest") {
          return new Date(b.date_created) - new Date(a.date_created);
        } else if (val === "dateOldest") {
          return new Date(a.date_created) - new Date(b.date_created);
        }
      });
    }
  },
  watch: {
    sortBy: function(val) {
      this.sortResults(val);
    }
  }
};
</script>
