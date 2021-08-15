<template>
  <div>
    <div class="container-fluid">
      <div class="row">
        <v-container class="mt-3">
          <v-card color="white">
            <div class="row">
              <div class="col-12">
                <v-card-title class="text-h5"> <v-icon>mdi-android</v-icon> {{category.name}} </v-card-title>
                <v-card-subtitle>{{category.description}}</v-card-subtitle>

              </div>
            </div>
          </v-card>
          <Categories :categories="categories" :cat-name="catname"></Categories>
        </v-container>
      </div>

      <div class="row">
        <div class="col-sm-7 col-xs-12">
          <h2 class="text-2xl text-right rtl font-bold p-2 text-blue-400"> <v-icon>mdi-android</v-icon> تصفح احدث وافضل تطبيقات الاندرويد </h2>
          <Applist :data="apps"></Applist>
        </div>
        <div class="col-sm-5 col-xs-12">
          <h2 class="text-2xl text-right rtl font-bold p-2 text-blue-400"> <v-icon>mdi-plus-circle</v-icon> أحدث الإضافات</h2>
          <Applist :data="latest"></Applist>
          <h2 class="text-2xl text-right rtl font-bold p-2 text-blue-400"> <v-icon>mdi-update</v-icon> تطبيقات محدثة</h2>
          <Applist :data="updated"></Applist>
          <h2 class="text-2xl text-right rtl font-bold p-2 text-blue-400"> <v-icon>mdi-medal</v-icon> الأعلى تقييما</h2>
          <Applist :data="topReviews"></Applist>
        </div>

      </div>
    </div>
    <infinite-loading @infinite="infiniteHandler" spinner="waveDots">
      <div slot="no-more">هذا كل شئ</div>
      <div slot="no-results">لا يوجد نتائج</div>
    </infinite-loading>
  </div>

</template>
<script>

import Applist from '../components/Applist';
import InfiniteLoading from 'vue-infinite-loading';
export default {
  components:{
    Applist,InfiniteLoading
  },
  data() {
    return {
      category : [],
      categories: [],
      featured: [],
      picked: [],
      latest: [],
      updated: [],
      topReviews: [],
      apps: [],
      errors: [],
      isShow: '',
      buttonshow: '',
      page:1,
    }
  },
  methods: {
    infiniteHandler($state) {
      this.$axios.get(process.env.apiUrl+'/android'+ '?page=' + this.page)
        .then(response => {
          if (response.data.data.apps.data.length) {
            this.page += 1;
            this.apps.push(...response.data.data.apps.data);
            $state.loaded();
          } else {
            $state.complete();
          }
        });
    },
  },
  asyncData({$axios}) {
    return $axios.$get(process.env.apiUrl+'/android')
      .then(response => {
        return{
          categories : response.data.subcategories,
          featured : response.data.featuredApps,
          latest : response.data.latestAdded,
          updated : response.data.latestUpdated,
          topReviews : response.data.topReviews,
          apps : response.data.apps.data,
          category : response.data.cat
        }
      })
  },
  head() {
    return {
      title: this.category.name,
      meta: [
        {name: 'description', content: this.category.seo_description},
        {name: 'keywords', content: this.category.seo_keywords},
        {property: 'og:title', content: this.category.name + ' - جي سوفت'},
        {property: 'og:site_name', content: 'جي سوفت'},
        {property: 'og:type', content: 'website'},
        {name: 'robots', content: 'index,follow'}
      ]
    }
  }
}

</script>
