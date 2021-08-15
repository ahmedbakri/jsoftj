<template>
  <div>
    <div class="container-fluid">
      <div class="row">
        <v-container class="mt-3">
          <v-card color="white">
            <div class="row">
              <div class="col-1">
                <v-avatar class="ma-3" size="125" tile>
                  <v-img :src="imageurl+ app.image"></v-img>
                </v-avatar>
              </div>
              <div class="col-11">
                <v-card-title class="text-h5" v-text="app.name"></v-card-title>
                <v-card-subtitle v-text="app.summary"></v-card-subtitle>
              </div>
            </div>
          </v-card>
        </v-container>
      </div>

      <div class="row">
        <div class="col-sm-9 col-xs-12">
          <div class="appinfo bg-white rounded">

            <v-card class="mb-5 pt-3 pb-3 pr-3 pl-3">
              <h3 class="mb-5">
                <v-icon>mdi-application-cog</v-icon>
                المعلومات الفنية
              </h3>
              <v-chip class="ml-1 mb-2" v-for="option in options" :key="option.title" v-if="option.length != 0 && option.title != 'os_version' && option.title !='Websitename' && option.title !='Website'">
                <v-icon class="ml-1">mdi-star</v-icon>
                {{ option.ar_title }} : {{ option.optnval }}
              </v-chip>
              <v-chip>
                <v-icon class="ml-1">mdi-eye</v-icon>
                المشاهدات : {{ app.views }}
              </v-chip>
              <v-chip>
                <v-icon class="ml-1">mdi-thumb-up</v-icon>
                الاعجابات : {{ app.likes }}
              </v-chip>
              <v-chip>
                <a :href="'../category/'+app.short_name">
                  <v-icon class="ml-1">mdi-folder</v-icon>
                  التصنيف : {{ app.category_name }}
                </a>
              </v-chip>
            </v-card>

            <v-card class="mb-5 pt-3 pb-3 pr-3 pl-3 " v-if="os_ver.length != 0">
              <h3 class="mb-5 ">
                <v-icon>mdi-monitor-cellphone-star</v-icon>
                انظمة التشغيل المدعومة
              </h3>
              <div class="w-full p-3">
                <v-chip class="ml-1 mb-2" v-for="os in os_ver[0].optnval.split('/') " :key="os.id"> {{ os }}</v-chip>
              </div>
            </v-card>

            <img class="img-fluid mb-3" src="https://kinsta.com/wp-content/uploads/2020/06/leaderboard-1.png">
            <v-card class="mb-1 pt-3 pb-3 pr-3 pl-3">

                <h3> <v-icon>mdi-download</v-icon> روابط التحميل </h3>

            </v-card>

            <v-expansion-panels class="mb-5">
              <v-expansion-panel v-for="link in mirrors" :key="link.id">
                <v-expansion-panel-header>
                  <span v-if="link.title"> {{ link.title }}</span>
                  <span v-else>{{link.url}} حمل الان</span>

                  <span class="float-left text-left ml-5" v-if="link.hits_num"> {{ link.hits_num }}  <v-icon>mdi-download</v-icon></span>
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                  <a :href="'../download/process?app='+app.subdomain+'&link=' + link.id" style="color: white">
                  <v-btn class="mt-3" block color="success">

                      <v-icon> mdi-download</v-icon>
                      التوجه الي رابط التحميل
                  </v-btn>
                  </a>
                </v-expansion-panel-content>
              </v-expansion-panel>
            </v-expansion-panels>

          </div>
          <img class="img-fluid mb-3" src="https://kinsta.com/wp-content/uploads/2020/06/leaderboard-1.png">
          <v-card class="mt-5 pt-3 pb-3 pr-3 pl-3">
            <h3>
              <v-icon>mdi-folder-heart-outline</v-icon>
              برامج بديلة
            </h3>
          </v-card>
          <Applist :data="alt"></Applist>
        </div>
        <div class="col-sm-3 col-xs-12">
          <div>
            <img class="img-fluid" src="https://lh3.googleusercontent.com/-CvehGt9hgL--10_ND70iU0O6MB63SDGL5Uwr4kUy5gS7hNhURsLDnDugxHJyLlesZM">
            <img class="img-fluid" src="https://lh3.googleusercontent.com/-CvehGt9hgL--10_ND70iU0O6MB63SDGL5Uwr4kUy5gS7hNhURsLDnDugxHJyLlesZM">
          </div>
        </div>
      </div>
    </div>
  </div>


</template>

<script>
import Applist from '../../components/Applist';
import Comments from '../../components/comments'
import VueSocialSharing from 'vue-social-sharing'

export default {
  components: {
    Applist, VueSocialSharing , Comments
  },
  data() {
    return {
      imageurl: process.env.IMAGE_URL + '/',
      app: [],
      alt: [],
      ver: [],
      latestver: [],
      options: [],
      os_ver: [],
      page_title:'',
      description:'',
      keywords:'',
      og_title:'',
      og_site_name:'',
      mirrors:[],
    }
  },

  asyncData({$axios,params,env}){
    return $axios.$get(process.env.apiUrl + '/app-info-byname/'+encodeURIComponent(params.slug))
      .then(response => {
        return {
          app : response.data.appInfo,
          alt : response.data.alternatives,
          ver : response.data.versions,
          latestver : response.data.versions[0],
          options : response.data.options,
          os_ver : response.data.options.filter(option => option.title == 'os_version'),
          page_title : response.data.appInfo.name,
          description : response.data.appInfo.seo_description,
          keywords : response.data.appInfo.seo_keywords,
          og_title : response.data.appInfo.name,
          og_site_name : response.data.appInfo.name,
        }
      })
  },

  created() {
    this.getlnks();
  },
  methods: {
    getlnks(){
      this.$axios.get(process.env.apiUrl +'/version-mirror/' + this.$route.params.slug)
        .then(response => {
            this.mirrors = response.data.data.mirrors,
            this.current_ver = response.data.current_ver
        })
    }
  },

  head() {
    return {
      title: this.app.name,
      meta: [
        {name: 'description', content: this.app.seo_description},
        {name: 'keywords', content: this.app.seo_keywords},
        {property: 'og:title', content: this.app.name + ' - جي سوفت'},
        {property: 'og:site_name', content: 'جي سوفت'},
        {property: 'og:type', content: 'website'},
        {name: 'robots', content: 'index,follow'}
      ]
    }
  }
}
</script>
