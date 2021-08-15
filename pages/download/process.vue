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
              <v-chip class="ml-1 mb-2" v-for="option in options" :key="option.title" v-if="option.title != 'os_version' && option.title !='Websitename' && option.title !='Website'">
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
            <img class="img-fluid mb-3" src="https://kinsta.com/wp-content/uploads/2020/06/leaderboard-1.png">
            <v-card class="mb-5 pt-3 pb-3 pr-3 pl-3">
              <div class="appinfo rounded mb-5">
                <h3 class="text-center">  جاري تجهيز رابط التحميل من فضلك انتظر قليلاً</h3>
                <h4 class="text-center"> سيتم اظهار الرابط بعد </h4>
                <no-ssr>
                <div class="text-center  rtl p-2 ml-2 p-3">
                  <flip-countdown :deadline="nowtime" :showDays="false" :showHours="false" :showMinutes="false"></flip-countdown>
                </div>
                </no-ssr>
                <a :href="download_url" style="color: white">
                <v-btn  v-if="showDownloadLink == true" class="mt-3" block color="success">

                    <v-icon> mdi-download</v-icon>
                    قم بالتحميل الان
                </v-btn>
                </a>

              </div>
            </v-card>
          </div>

        </div>
        <div class="col-sm-3 col-xs-12">
          <div>
            <img class="img-fluid" src="https://lh3.googleusercontent.com/-CvehGt9hgL--10_ND70iU0O6MB63SDGL5Uwr4kUy5gS7hNhURsLDnDugxHJyLlesZM">
          </div>
        </div>
      </div>
    </div>
  </div>


</template>

<script>

import FlipCountdown from 'vue2-flip-countdown'

export default {
  components: {
    FlipCountdown
  },
  data() {
    return {
      nowtime:'',
      imageurl: process.env.IMAGE_URL + '/',
      app: [],
      options: [],
      page_title:'',
      description:'',
      keywords:'',
      og_title:'',
      og_site_name:'',
      mirrors:[],
      link_id:'',
      download_url:'',
      showDownloadLink:false
    }
  },

  asyncData({$axios,params,env,query}){
    return $axios.$get(process.env.apiUrl + '/app-info-byname/'+encodeURIComponent(query.app))
      .then(response => {
        return {
          link_id : query.link,
          app : response.data.appInfo,
          options : response.data.options,
          page_title : response.data.appInfo.name,
          description : response.data.appInfo.seo_description,
          keywords : response.data.appInfo.seo_keywords,
          og_title : response.data.appInfo.name,
          og_site_name : response.data.appInfo.name,
        }
      })
  },

  created() {
    this.getNow();
    this.getDownloadLInk();
  },
  // fetch ({$axios, redirect , query }) {
  //   return $axios.$get(process.env.apiUrl + '/download/'+query.link)
  //     .then(response => {
  //      // redirect(response.download.url);
  //       setTimeout(() => redirect(response.download.url), 100)
  //     });
  //
  //   //
  // },
  methods: {
    getNow: function() {
      const today = new Date();
      today.setSeconds(today.getSeconds() + 10);
      const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
      const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
      const dateTime = date +' '+ time;
      this.nowtime = dateTime;
    },
    getDownloadLInk(){
      this.$axios.get(process.env.apiUrl + '/download/'+this.link_id)
        .then(response => {
            this.download_url = response.data.download.url
            setTimeout(() => this.showDownloadLink = true , 10000)
        });
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
