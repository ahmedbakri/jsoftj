<template>
  <div>
    <div class="container-fluid" dir="rtl">
      <div class="row">
        <v-container class="mt-3">
          <v-card color="white">
            <div class="row">
              <div class="col-12">
                <v-card-title class="text-h5" v-text="category.name "></v-card-title>
                <v-card-subtitle v-text="category.description"></v-card-subtitle>

              </div>
            </div>
          </v-card>
          <Categories :categories="categories" :cat-name="catname"></Categories>
        </v-container>
      </div>

      <div class="row">
        <div class="col-sm-6 col-xs-12">
          <h2 class="text-2xl text-right rtl font-bold p-2 text-blue-400"> <v-icon>mdi-star</v-icon> برامج مميزة</h2>
          <Applist :data="apps"></Applist>
        </div>
        <div class="col-sm-6 col-xs-12">
          <h2 class="text-2xl text-right rtl font-bold p-2 text-blue-400"> <v-icon>mdi-plus-circle</v-icon>  أحدث الإضافات</h2>
          <Applist :data="latest"></Applist>
        </div>
      </div>
    </div>
    <no-ssr>
    <infinite-loading @infinite="infiniteHandler" spinner="waveDots">
      <div slot="no-more">هذا كل شئ</div>
      <div slot="no-results">لا يوجد نتائج</div>
    </infinite-loading>
    </no-ssr>
  </div>

</template>

<script>
import Applist from '../../components/Applist';
import Categories from '../../components/Categories';
import InfiniteLoading from 'vue-infinite-loading';
export default {
  components:{
    Applist,Categories,InfiniteLoading
  },
  data() {
    return {
      category:null,
      catname:'',
      isShow:'',
      buttonshow:'',
      categories: [],
      picked: [],
      latest: [],
      apps:[],
      page:2
    }
  },

  asyncData({$axios,params}){
    return $axios.$get(process.env.apiUrl+'/category-info/'+params.slug)
      .then(response => {
        return{
          categories : response.data.subcategories,
          latest : response.data.latestAdded,
          apps : response.data.apps.data,
          category : response.data.cat,
          catname : response.data.cat.short_name,
        }
      })
  },
  methods:{
    infiniteHandler($state) {
      this.$axios.get(process.env.apiUrl+'/category-info/'+this.catname+ '?page=' + this.page)
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
