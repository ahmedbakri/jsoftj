<template>
  <div>
    <div class="catcontainer p-2 pr-3 pb-1 mb-12 mr--1">
      <div class="row">
        <div class="col-sm-6 col-xs-12">
          <h3> <i class="fab fa-windows text-3xl"></i> برامج الويندوز</h3>
          <Applist :data="windows"></Applist>
        </div>
        <div class="col-sm-6 col-xs-12">
          <h3 class="text-2xl text-right rtl font-bold p-2  text-blue-400"> <i class="fab fa-android text-3xl"></i> برامج الاندرويد</h3>
          <Applist :data="android"></Applist>
        </div>
      </div>
      <div class="row">
        <div class="col-sm-6 col-xs-12">
          <h3> <i class="fab fa-apple text-3xl"></i> برامج الماك </h3>
          <Applist :data="mac"></Applist>
        </div>
        <div class="col-sm-6 col-xs-12">
          <h3> <i class="fa fa-desktop text-3xl" aria-hidden="true"></i> احدث التعريفات</h3>
          <Applist :data="drivers"></Applist>
        </div>
      </div>
      <div class="row">
        <div class="col-sm-6 col-xs-12">
          <h3> <i class="fa fa-gamepad text-3xl"></i>  احدث الالعاب</h3>
          <Applist :data="games"></Applist>
        </div>
        <div class="col-sm-6 col-xs-12">
          <h3> <i class="fa fa-file text-3xl"></i>  احدث المقالات</h3>
          <Applist :data="articles"></Applist>
        </div>
      </div>
    </div>
  </div>

</template>

<script>

import Applist from '../components/Applist';
export default {
  components:{
    Applist
  },
  data() {
    return {
      windows: [],
      games: [],
      mac: [],
      android: [],
      drivers: [],
      errors: [],
      articles:[],
    }
  },
  asyncData({$axios}){
    return $axios.$get(process.env.apiUrl+'/home')
      .then(response => {
        return{
          windows : response.data.windows,
          games : response.data.games,
          mac : response.data.mac,
          android : response.data.android,
          drivers : response.data.drivers,
          articles : response.data.articles
        }

      })
  },
}
</script>
