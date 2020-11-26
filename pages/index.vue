<template>
    <div class="container">
        <Search></Search>
        <h2 class="text-success text-center" v-if="loading">Loading Data</h2>
        <div  v-for="city in cities">
            <Weather :city="city" @notLoading="removeLoading"></Weather>
        </div>
    </div>
</template>

<script>
  export default {
    data() {
      return {
        cities: ["Istanbul", "Berlin", "London", "Helsinki", "Dublin", "Vancouver"],
        loading : true
      };
    },
    methods:{
      removeLoading(){
        const self = this;
        self.loading = false
      }
    }
  }
</script>

<style>
.container {
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
