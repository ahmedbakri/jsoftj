<template>
  <div>
    <v-list style="background-color: transparent !important;">
      <template v-for="(comment, index) in comments">
          <v-card class="mb-2">
            <v-list-item :key="comment.id">
              <v-list-item-content>
                <v-list-item-title >{{ comment.name }}</v-list-item-title>
                <span class="group">
                  <v-icon v-for="n in comment.rate" :key="n" color="orange"> mdi-star </v-icon>
                  <v-icon v-for="x in 5-comment.rate" :key="x" color="grey darken-1"> mdi-star </v-icon>
                </span>

                {{comment.text}}
                <v-list-item-subtitle class="mt-3" dir="rtl">
                  <span class="badge badge-pill badge-primary"> <v-icon small>mdi-calendar-month</v-icon> {{ comment.created_at }}  </span>
                </v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-card>
      </template>
    </v-list>
  </div>
</template>

<script>
export default {
  props: {
    comments: {
      type: [],
      default: null
    }
  }
}
</script>
