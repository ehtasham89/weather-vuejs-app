<template>
      <div class="row">
          <form class="form-inline col-md-12 m-auto" @submit.prevent="Search">
              <input class="form-control w-65" v-model="keyword" type="search"
                      placeholder="Search" aria-label="Search">
              <button class="btn btn-outline-success w-35"
                      type="submit">
                  Search
              </button>
          </form>
      </div>
</template>

<script>
  export default {
    data() {
      return {
        query: this.$route.params.keyword,
        keyword: ''
      }
    },
    methods: {
      Search() {
        const self = this;
        this.$router.push(`/search/${self.keyword}`);
      }
    }
  }
</script>