<template>
  <div class="wrapper">
    <Heroimage />
    <Claim/>
    <SearchInput v-model="searchValue" @input="handleInput" />
  </div>
</template>

<script>
// eslint-disable-next-line
import axios from 'axios';
import debounce from 'lodash.debounce';
import Heroimage from '@/components/Heroimage.vue';
import Claim from '@/components/Claim.vue';
import SearchInput from '@/components/SearchInput.vue';

const API = 'https://images-api.nasa.gov';

export default {
  name: 'App',
  components: {
    Heroimage,
    Claim,
    SearchInput,
  },
  data() {
    return {
      searchValue: '',
      results: [],
    };
  },
  methods: {
    // eslint-disable-next-line
    handleInput: debounce(function() {
      console.log(this.searchValue);
      axios.get(`${API}/search?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
        })
        .catch((error) => {
          console.log(error);
        });
    }, 500),
  },
};
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css?family=Montserrat:300,400,600,800&display=swap');

  * {
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  body {
    font-family: 'Montserrat', sans-serif;
    margin:0;
    padding:0;
    color:white;
  }

  .wrapper{
    display:flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin:0;
    height: 100vh;
    padding:30px;
    width:100%;
  }
</style>
