<template>
 <v-card
    class="mx-auto"
    max-width="344"
  >
    <v-img
      :src="postData.data.thumbnail !== 'self' 
        ? postData.data.thumbnail 
        : require('@/assets/generic_lotr.jpg')"
      height="200px"
    ></v-img>
    <v-card-title>
      {{ postData.data.title }}
    </v-card-title>
    <v-card-actions v-if="postData.data.selftext">
      <v-btn
        color= "#0093FC"
        text
        @click="show = !show"
      >
        Ver mas..
      </v-btn>
    </v-card-actions>
    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>
        <v-card-text>
          {{postData.data.selftext}}
        </v-card-text>
      </div>
    </v-expand-transition>
  </v-card>
</template>

<script>
export default {
  name: 'Card',
  props: {
    postData: Object,
  },
  data() {
    return {
      show: false,
    }
  },
}
</script>
