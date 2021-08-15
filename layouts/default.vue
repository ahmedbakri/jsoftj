<template>
  <v-app>

    <v-navigation-drawer v-model="drawer" :mini-variant="miniVariant" :clipped="clipped" fixed app right>
      <v-list>
        <v-list-item v-for="(item, i) in menu" :key="i" :to="item.href" router exact>
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-right="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title v-text="title" />
      <SiteLogo></SiteLogo>
      <search></search>
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container style="background-color: whitesmoke !important;">
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer padless color="white">
      <v-col
        class="text-center"
        cols="12"
      >
        {{ new Date().getFullYear() }} — <strong>جي سوفت</strong>
      </v-col>
    </v-footer>
  </v-app>
</template>

<script>
import SiteLogo from "../components/SiteLogo";
import search from "../components/search";
export default {
  components: {SiteLogo,search},
  data () {
    return {
      clipped: true,
      drawer: true,
      fixed: true,
      menu: [
        {
          href: '/',
          title: 'الرئيسية',
          icon: 'mdi-home'
        },
        {
          href: '/windows',
          title: 'برامج ويندوز',
          icon: 'mdi-microsoft-windows'
        },
        {
          href: '/mac',
          title: 'برامج ماك',
          icon: 'mdi-apple-finder'
        },
        {
          href: '/android',
          title: 'برامج اندرويد',
          icon: 'mdi-android'
        },
        {
          href: '/phones',
          title: ' برامج جوال',
          icon: 'mdi-cellphone-text'
        },
        {
          href: '/iphone',
          title: 'ايفون وايباد',
          icon: 'mdi-apple'
        },
        {
          href: '/games',
          title: 'ألعاب',
          icon: 'mdi-google-controller'
        },
        {
          href: '/drivers',
          title: 'تعريفات الأجهزة',
          icon: 'mdi-application-cog'
        },
        {
          href: '/articles',
          title: 'مقالات',
          icon: 'mdi-newspaper-variant-outline'
        },
        {
          href: '/reviews',
          title: 'الاخبار',
          icon: 'mdi-newspaper-plus'
        },
        {
          href: '/mixes',
          title: 'المنوعات',
          icon: 'mdi-movie-open-star'
        },

      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title:null
    }
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400&display=swap');
*{font-family: 'Cairo', sans-serif;}
a{text-decoration: none;}
.v-avatar{border-radius: 0 !important;}
.v-avatar{width: 70px !important; min-width: 70px !important; height: 70px !important;}
img {max-width: 100% !important;max-height: 500px !important;}
</style>
