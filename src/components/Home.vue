<template>
  <v-container>
    <v-overlay :value="overlay">
      <v-progress-circular
        indeterminate
        size="64"
        color="#0093FC"
      ></v-progress-circular>
    </v-overlay>
    <v-radio-group v-model="picked">
      <p> Mostar por:</p>
      <v-radio label="Los últimos post" value="allPosts"></v-radio>
      <v-radio label="Los últimos post de los Hobbits" value="Hobbits"
      ></v-radio>
    </v-radio-group>
    <p> Filtrar por:</p>
    <v-radio-group
      v-model="picked"
      row
    >
      <v-radio label="Gollum" value="Gollum"></v-radio>
      <v-radio label="Frodo"  value="Frodo"></v-radio>
      <v-radio label="Bilbo"  value="Bilbo"></v-radio>
      <v-radio label="Sam"    value="Sam"></v-radio>
      <v-radio label="Merry"  value="Merry"></v-radio>
      <v-radio label="Pippin" value="Pippin"></v-radio>
    </v-radio-group>
    <v-row class="mt-9">
      <div v-if="postData.length === 0" > 
        No hay nada que mostar
      </div>
      <v-col
        cols="12"
        sm="4" 
        v-for="item in postData" 
        :key="item.data.id">
        <card :postData = "item" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>

import Card from './Card';

export default {
  name: 'Home',
  components: {
    Card
  },
  data() {
    return {
      overlay: false,
      posts:{},
      postData: {},
      picked: 'allPosts'
    };
  },
  watch: {
    picked: function(){
      switch (this.picked) {
        case 'allPosts':
            this.getPost()
          break;
        case 'Hobbits':
            this.filterHobbits(this.posts)
          break;
        default:
          this.filterHobbit(this.posts, this.picked)
          break;
      }
    }
  },
  created() {
   this.getPost();
  },
  methods: {
    getPost: async function () {
      this.overlay = true
      const response = await fetch("https://www.reddit.com/r/lotr/new.json?limit=100");
      const {data:{children}} = await response.json();
      this.posts = children;
      this.postData = this.posts;
      this.overlay = false;
      console.log('all Posts', this.postData);
    },
    filterHobbits: function (data) {
       this.postData = data.filter((item)=>{
        return  item.data.selftext.includes('Gollum')
            || item.data.selftext.includes('Frodo') 
            || item.data.selftext.includes('Bilbo')
            || item.data.selftext.includes('Sam')
            || item.data.selftext.includes('Samsagaz')
            || item.data.selftext.includes('Meriadoc')
            || item.data.selftext.includes('Merry')
            || item.data.selftext.includes('Peregrin')
            || item.data.selftext.includes('Pippin')
            || item.data.selftext.includes('Smeagol')
      });
      console.log('Hobbits Posts', this.postData);
    },
    filterHobbit: function (data, hobbit) {
      this.postData = data.filter((item)=>{
        return item.data.selftext.includes(hobbit)
      });
      console.log('Hobbit Posts', this.postData);
    }
  },
}
</script>

