<template>
  <div>
    <div class="catcontainer bg-blue-800 p-2 pr-3 pb-1 mb-12 mr--1">
      <h2 class="text-3xl rtl text-center p-2 text-white"> <v-icon>mdi-movie-open-star</v-icon> احدث المنوعات </h2>
    </div>

    <v-list three-line>
      <template v-for="(item, index) in mixes">
        <v-divider v-if="index != 0" :key="index" :inset="item.inset"></v-divider>
        <a :href="'http://jsoft.test/mix/' + item.subdomain" :key="item.id">
          <v-list-item :key="item.name">
            <v-list-item-avatar>
              <v-img class="app-img" :src="'http://jsoft.test/uploads/mixs/' + item.image" :alt="item.name" :title="item.name"></v-img>
            </v-list-item-avatar>
            <v-list-item-content>
              <v-list-item-title v-html="item.name"></v-list-item-title>
              <v-list-item-subtitle v-html="item.summary"></v-list-item-subtitle>
              <v-list-item-subtitle class="mt-3" dir="rtl">
                <span class="badge badge-pill badge-primary ml-2"> <v-icon small>mdi-eye</v-icon> {{ item.views }}  </span>
                <span class="badge badge-pill badge-primary ml-2"> <v-icon small>mdi-thumb-up</v-icon> {{ item.likes }} </span>
                <span class="badge badge-pill badge-primary"> <v-icon small>mdi-calendar-month</v-icon> {{ item.updated_at }}  </span>
              </v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </a>
      </template>
    </v-list>
    <infinite-loading @infinite="infiniteHandler" spinner="waveDots">
      <div slot="no-more">هذا كل شئ</div>
      <div slot="no-results">لا يوجد نتائج</div>
    </infinite-loading>
  </div>

</template>
<script>

export default {
  data() {
    return {
      mixes: [],
      page: 1
    }
  },
  created() {
    this.$axios.get(process.env.apiUrl+'/get-all-mixes')
      .then(response => {
        this.mixes = response.data.data.mixes.data,
          this.page = response.data.data.mixes.current_page + 1
      })
      .catch(e => {
        this.errors.push(e)
      })
  },
  methods: {
    infiniteHandler($state) {
      this.$axios.get(process.env.apiUrl+'/get-all-mixes'+ '?page=' + this.page)
        .then(response => {
          if (response.data.data.mixes.data.length) {
            this.page += 1;
            this.mixes.push(...response.data.data.mixes.data);
            $state.loaded();
          } else {
            $state.complete();
          }
        });
    },
  },
}
</script>
