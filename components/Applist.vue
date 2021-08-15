<template>
  <div>
    <v-list style="background-color: transparent !important;">
      <template v-for="(item, index) in data">
        <nuxt-link :to="'../app/'+encodeURIComponent(item.subdomain)">
        <v-card class="mb-2">
          <v-list-item :key="item.subdomain">
            <v-list-item-avatar>
              <v-img class="app-img" :src="imageurl+ item.image" :alt="item.name" :title="item.name"></v-img>
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
        </v-card>
        </nuxt-link>
      </template>
    </v-list>
  </div>
</template>

<script>
export default {
  props: {
    data: {
      type: [],
      default: null
    }
  },
  data(){
    return{
      imageurl:process.env.IMAGE_URL+'/',
    }
  }
}
</script>
