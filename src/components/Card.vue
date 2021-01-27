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
    <v-card-text><b>Creado por: </b>{{postData.data.author}}</v-card-text>
    <v-card-actions v-if="postData.data.selftext">
      <v-btn
        color= "#00D389"
        class="white--text"
        @click="show = !show"
      >
        <span>Ver mas..</span>
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
