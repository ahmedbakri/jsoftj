<template>
  <div>
    <div class="container-fluid" dir="rtl">
      <div class="row">
        <v-container class="mt-3">
          <v-card color="white">
            <div class="row">
              <div class="col-12">
                <v-card-title class="text-h5"> <v-icon>mdi-microsoft-windows</v-icon> {{ category.name }}</v-card-title>
                <v-card-subtitle>{{category.description}}</v-card-subtitle>
              </div>
            </div>
          </v-card>
          <Categories :categories="categories" :cat-name="catname"></Categories>
        </v-container>
      </div>

      <div class="row">
        <div class="col-sm-6 col-xs-12">
          <h2 class="text-2xl text-right rtl font-bold p-2 text-blue-400"> <v-icon>mdi-star</v-icon> برامج مميزة</h2>
          <Applist :data="featured"></Applist>
        </div>
        <div class="col-sm-6 col-xs-12">
          <h2 class="text-2xl text-right rtl font-bold p-2 text-blue-400"> <v-icon>mdi-plus-circle</v-icon>  أحدث الإضافات</h2>
          <Applist :data="latest"></Applist>
        </div>

        <div class="col-sm-6 col-xs-12">
          <h2 class="text-2xl text-right rtl font-bold p-2  text-blue-400"> <v-icon>mdi-update</v-icon> برامج محدثة</h2>
          <Applist :data="updated"></Applist>
        </div>


        <div class="col-sm-6 col-xs-12">
          <h2 class="text-2xl text-right rtl font-bold p-2 text-blue-400"> <v-icon>mdi-heart</v-icon> إختيارات محررينا</h2>
          <Applist :data="picked"></Applist>
        </div>

        <div class="col-sm-6 col-xs-12">
          <h2 class="text-2xl text-right rtl font-bold p-2 text-blue-400"> <v-icon>mdi-medal</v-icon> الأعلى تقييما</h2>
          <Applist :data="topReviews"></Applist>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Applist from '../components/Applist';
import Categories from '../components/Categories';
export default {
  components:{
    Applist,Categories
  },
  data() {
    return {
      category : [],
      catname:'windows',
      isShow:'',
      buttonshow:'',
      categories: [],
      featured: [],
      picked: [],
      latest: [],
      updated: [],
      topReviews: [],
      errors: []
    }
  },
  asyncData({$axios}){
    return $axios.$get(process.env.apiUrl+'/home-api')
      .then(response => {
        return{
          categories : response.data.subcategories,
          featured : response.data.featuredApps,
          picked : response.data.editorPicks,
          latest : response.data.latestAdded,
          updated : response.data.latestUpdated,
          topReviews : response.data.topReviews,
          category : response.data.cat
        }

      })
  }
  ,
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
