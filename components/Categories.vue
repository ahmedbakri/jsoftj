
<template>
  <div v-if="categories.length > 0">
    <v-row>
      <v-col cols="12 mt-5">
        <v-card class="pb-3">
          <v-card-title>  <v-icon>mdi-file-tree</v-icon> الاقسام الفرعية   </v-card-title>
          <v-chip-group active-class="primary--text" column>
            <v-chip v-for="category in categories" :key="category.short_name">
              <router-link :to="'../category/' + category.short_name" class="block">
                {{ category.name }}
              </router-link>
            </v-chip>
          </v-chip-group>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  props: {
    categories: {
      type: [],
      default: null
    },
    catName:{
      type: '',
      default: 'windows'
    }
  }
}
</script>
