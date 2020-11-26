<template>
    <b-container>
      <Loader v-if="loading" />
      <b-row class="mt-25">
        <b-col class="hp-5">
          <Weather :woeid="this.woeid" :consolidated="true" @notLoading="removeLoading"></Weather>
        </b-col>
      </b-row>
    </b-container>
</template>

<script>
export default {
    data() {
        return {
            woeid: this.$route.params.woeid,
            loading : true
        }
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