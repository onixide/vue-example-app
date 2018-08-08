<template>
  <div class="search-wrapper">
    <div class="search">
      <label for="search">Search</label>
      <input 
        name="search" 
        id="search" 
        v-model="searchValue"
        @input="handleInput"
      />
    </div>
    <ul v-for="result in results" :key="result.data[0].nasa_id">
      <p>{{ result.data[0].description }}</p>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';
const API = "https://images-api.nasa.gov/search";
export default {
  name: 'Search', 
  data(){
    return {
      searchValue: "",
      results: [],
    };
  },
  methods: {
    handleInput: debounce(function(){
      axios.get(`${API}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
        })
        .catch((error) => {
          console.log(error);
        })
      console.log(this.searchValue);
  }, 500),
  },
};
</script>

<style lang="scss" scoped>

.search-wrapper {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0;
  padding: 30px;
}

.search {
  width: 300px;
  display: flex;
  flex-direction: column;
  width: 250px;

  label {
    font-family: Arial, Helvetica, sans-serif;
  }

  input {
    height: 30px;
    border: none;
    border-bottom: 1px solid #000;
  }
}

</style>

