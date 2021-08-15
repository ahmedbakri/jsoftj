<template>
  <div>
    <div class="container-fluid" dir="rtl">
      <div class="row">

        <div class="col-sm-9 col-xs-12">
          <h3 class="p-2 mb-3">   جميع البرامج والتطبيقات التي تحتوي علي الوسم : <v-icon>mdi-tag</v-icon> {{slug}}  </h3>
          <Applist :data="apps"></Applist>
        </div>

        <div class="col-sm-3 col-xs-12">
            <img class="img-fluid" src="https://lh3.googleusercontent.com/-CvehGt9hgL--10_ND70iU0O6MB63SDGL5Uwr4kUy5gS7hNhURsLDnDugxHJyLlesZM">
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
      apps:[],
      slug : '',
      page:2
    }
  },

  asyncData({$axios,params}){
    return $axios.$get(process.env.apiUrl+'/tags/'+encodeURIComponent(params.slug))
      .then(response => {
        return{
          apps : response.data.apps.data,
          slug : response.data.tag,
        }
      })
  },
  methods:{
    infiniteHandler($state) {
      this.$axios.get(process.env.apiUrl+'/tags/'+this.slug+ '?page=' + this.page)
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
      title: this.slug,
      meta: [
        {name: 'description', content: this.slug},
        {name: 'keywords', content: this.slug},
        {property: 'og:title', content: this.slug + ' - جي سوفت'},
        {property: 'og:site_name', content: 'جي سوفت'},
        {property: 'og:type', content: 'website'},
        {name: 'robots', content: 'index,follow'}
      ]
    }
  }
}
</script>
