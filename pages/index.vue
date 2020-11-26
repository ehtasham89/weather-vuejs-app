<template>
    <b-container> 
      <Loader v-if="loading" />
      
      <b-row class="mt-25">
        <b-col class="hp-5" v-for="city in cities">
          <Weather :city="city" v-bind:key="city.woeid" @notLoading="removeLoading" />
        </b-col>
      </b-row>
    </b-container>
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
.mt-25 {
  margin-top: 25px;
}
.hp-5 {
  padding-left: 5px !important;
  padding-right: 5px !important;
}
</style>
