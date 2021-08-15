<template>
    <v-autocomplete
      v-model="select"
      :loading="loading"
      :items="items"
      :search-input.sync="search"
      cache-items
      class="mx-4 mr-8"
      flat
      hide-no-data
      hide-details
      label="ابحث عن برنامجك المفضل هنا..."
      solo
    ></v-autocomplete>
</template>

<script>
export default {
  data () {
    return {
      loading: false,
      items: [],
      search: null,
      select: null,
      apps: [],
    }
  },
  watch: {
    search (val) {
      val && val !== this.select && this.querySelections(val)
    },
  },
  methods: {
    querySelections (v) {

      if (this.search.length >= 3){

        this.loading = true

        this.$axios.get(process.env.apiUrl+'/search/'+this.search)
          .then(response => {
              this.apps = response.data.apps;
          });

        // Simulated ajax query
          this.items = this.apps.filter(e => {
            return (e || '').toLowerCase().indexOf((v || '').toLowerCase()) > -1
          })
          this.loading = false


      }
    },
  },
}
</script>
