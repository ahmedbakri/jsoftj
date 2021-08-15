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

            <CoolLightBox
              :items="gallery"
              :index="index"
              @close="index = null">
            </CoolLightBox>


            <v-carousel cycle auto hide-delimiter-background show-arrows-on-hover>
              <template v-slot:prev="{ on, attrs }">
                <v-btn color="success" v-bind="attrs" v-on="on"> السابق</v-btn>
              </template>
              <template v-slot:next="{ on, attrs }">
                <v-btn color="info" v-bind="attrs" v-on="on"> التالي</v-btn>
              </template>
              <div class="images-wrapper">
                <div v-for="(item,i) in gallery" :key="item">
                  <v-carousel-item @click="index = i" style="width:100%" :src="item" reverse-transition="fade-transition" transition="fade-transition"></v-carousel-item>
                </div>
              </div>

            </v-carousel>

            <img class="img-fluid mt-5" src="https://kinsta.com/wp-content/uploads/2020/06/leaderboard-1.png">
            <v-card class="mb-5 mt-5 pt-3 pb-3 pr-3 pl-3">
              <h3 class="mt-5">
                <v-icon>mdi-file</v-icon>
                تفاصيل البرنامج
              </h3>
              <v-row class="mt-1">
                <v-col v-html="app.description">
                </v-col>
              </v-row>
            </v-card>

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

            <v-card class="mb-5 pt-3 pb-3 pr-3 pl-3"  v-if="os_ver.length != 0">
              <h3 class="mb-5 ">
                <v-icon>mdi-monitor-cellphone-star</v-icon>
                انظمة التشغيل المدعومة
              </h3>
              <div class="w-full p-3">
                <v-chip class="ml-1 mb-2" v-for="os in os_ver[0].optnval.split('/') " :key="os.id"> {{ os }}</v-chip>
              </div>
            </v-card>
            <img class="img-fluid mb-3" src="https://kinsta.com/wp-content/uploads/2020/06/leaderboard-1.png">
            <v-card class="mb-5 pt-3 pb-3 pr-3 pl-3">
              <h3 class="mb-5">
                <v-icon>mdi-tag-multiple</v-icon>
                العلامات
              </h3>
              <div class="w-full p-3">
                <v-chip class="ml-1 mb-2" v-for="tag in tags" :key="tag.id">
                  <a :href="'../tags/'+tag">
                    <v-icon class="ml-1">mdi-tag</v-icon>
                    {{ tag }}</a>
                </v-chip>
              </div>
            </v-card>

            <a :href="'../download/' + encodeURIComponent(app.subdomain)" style="color: white">
            <v-btn class="mt-3" block color="success">

                <v-icon> mdi-download</v-icon>
                حمل {{ app.name }} الان
            </v-btn>
            </a>

            <v-card class="mb-5 mt-5 pt-3 pb-3 pr-3 pl-3">
              <h3 class="mb-5">
                <v-icon>mdi-share-variant</v-icon>
                شارك المحتوي
              </h3>
              <div class="p-3 mr-3">

                <template v-for="network in networks">
                  <ShareNetwork
                    :network="network.network"
                    :key="network.network"
                    :style="{backgroundColor: network.color, color:'white',margin:'5px',padding:'1px 5px 2px 5px',float:'right'}"
                    :url="sharing_url"
                    :title="sharing_title"
                    :description="sharing_description"
                    :quote="sharing_quote"
                    :hashtags="sharing_hashtags"
                    :twitterUser="sharing_twitterUser"
                  >
                    <v-icon color="white">{{ network.icon }}</v-icon>
                    {{ network.name }}
                  </ShareNetwork>
                </template>
              </div>
              <div style="clear: both"></div>
            </v-card>

            <div class="clear-both"></div>
          </div>
          <img class="img-fluid" src="https://kinsta.com/wp-content/uploads/2020/06/leaderboard-1.png">
          <v-card class="mt-5 pt-3 pb-3 pr-3 pl-3">
            <h3><v-icon>mdi-folder-heart-outline</v-icon>برامج بديلة</h3>
          </v-card>

          <Applist :data="alt"></Applist>

          <v-card class="mb-5 mt-5 pt-3 pb-3 pr-3 pl-3">
            <h3 class="mb-5">
              <v-icon>mdi-comment</v-icon>
              التعليقات
            </h3>
            <div class="comment">
              <v-textarea style="text-align: right;" label="اكتب اسمك" auto-grow outlined rows="1" row-height="15" shaped v-model="comment_name"></v-textarea>
              <v-textarea style="text-align: right;" label="اضف تعليق" auto-grow outlined rows="5" row-height="25" shaped @keyup.enter="store()" v-model="comment_content"></v-textarea>
              <v-rating v-model="rating_value" background-color="purple lighten-3" color="purple" large>hhhhhhh</v-rating>
              <div class="clearfix "></div>
              <v-btn class="mt-3" rounded color="success" dark @keyup.enter="store()" @click.prevent="store()">
                <v-icon>mdi-plus-circle</v-icon>   اضافة تعليق
              </v-btn>

              <v-snackbar v-model="snackbar" color="success">
                  شكرا لك ...سوف يتم اضافة التعليق الي قائمة التعليقات بعد مراجعته من الادارة
                <template v-slot:action="{ attrs }">
                  <v-btn color="indigo" text v-bind="attrs" @click="snackbar = false">
                   <v-icon>mdi-close-circle</v-icon>
                  </v-btn>
                </template>
              </v-snackbar>

              <v-snackbar v-model="errorsnackbar" color="danger">
                عفوا حدث خطأ اثناء ارسال البيانات الرجاء المحاولة فيما بعد
                <template v-slot:action="{ attrs }">
                  <v-btn color="indigo" text v-bind="attrs" @click="errorsnackbar = false">
                    <v-icon>mdi-close-circle</v-icon>
                  </v-btn>
                </template>
              </v-snackbar>
            </div>
          </v-card>

          <Comments :comments="reviews" class="mt-5"></Comments>

        </div>
        <div class="col-sm-3 col-xs-12">
          <a :href="'../download/' + encodeURIComponent(app.subdomain)" style="color: white">
          <v-btn block class="mt-3" color="success" dark><v-icon> mdi-download</v-icon>قم بالتحميل الان</v-btn>
          </a>
          <div class="mt-5">
            <img class="img-fluid" src="https://lh3.googleusercontent.com/-CvehGt9hgL--10_ND70iU0O6MB63SDGL5Uwr4kUy5gS7hNhURsLDnDugxHJyLlesZM">
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
import CoolLightBox from 'vue-cool-lightbox'
import 'vue-cool-lightbox/dist/vue-cool-lightbox.min.css'

export default {
  components: {
    Applist, VueSocialSharing , Comments, CoolLightBox
  },
  data() {
    return {
      index:'',
      snackbar:false,
      errorsnackbar:false,
      imageurl: process.env.IMAGE_URL + '/',
      GalleryURL: process.env.Gallery_URL + '/',
      comment_name: '',
      comment_content: '',
      rating_value: 1,
      app: [],
      alt: [],
      ver: [],
      latestver: [],
      gallery: [],
      options: [],
      reviews: [],
      picked: [],
      os_ver: [],
      tags: [],
      croute: this.$route,


      sharing: {
        url: '',
        title: '',
        description: '',
        quote: '',
        hashtags: '',
        twitterUser: ''
      },
      networks: [
        {network: 'facebook', name: 'فيسبوك', icon: 'mdi-facebook', color: '#1877f2'},
        {network: 'linkedin', name: 'لينكيدان', icon: 'mdi-linkedin', color: '#007bb5'},
        {
          network: 'messenger',
          name: 'ماسينجر',
          icon: 'mdi-facebook-messenger',
          color: '#0084ff'
        },
        {network: 'skype', name: 'سكايب', icon: 'mdi-skype', color: '#00aff0'},
        {network: 'twitter', name: 'تويتر', icon: 'mdi-twitter', color: '#1da1f2'}
      ],
      page_title:'',
      description:'',
      keywords:'',
      og_title:'',
      og_site_name:'',
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
          gallery : response.data.gallery,
          options : response.data.options,
          reviews : response.data.reviews,
          tags : response.data.appInfo.keywords.split(","),
          os_ver : response.data.options.filter(option => option.title == 'os_version'),
          page_title : response.data.appInfo.name,
          description : response.data.appInfo.seo_description,
          keywords : response.data.appInfo.seo_keywords,
          og_title : response.data.appInfo.name,
          og_site_name : response.data.appInfo.name,
          sharing_title : response.data.appInfo.name,
          sharing_url : env.BASE_URL+response.data.appInfo.name,
          sharing_description : response.data.appInfo.summary,
          sharing_quote : response.data.appInfo.summary,
          sharing_hashtags : response.data.appInfo.keywords,
          sharing_twitterUser : response.data.appInfo.name,
        }
      })
  },

  methods: {
    async store() {
      let data = {
        name: this.comment_name,
        text: this.comment_content,
        rate: this.rating_value,
        app_id: this.app.id,

      }
      this.$axios.post(process.env.apiUrl +'/comment/store', data , {
        headers: {
          Accept: "application/json"
        }
      })
        .then((response) => {
          this.comment_name = this.comment_content = '';
          this.rating_value = 0;
          this.snackbar = true;
        }).catch((reason)=>{
          this.errorsnackbar = true;
      });
    },
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
