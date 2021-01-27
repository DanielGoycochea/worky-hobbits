<template>
  <v-container>
    <v-overlay :value="overlay">
      <v-progress-circular
        indeterminate
        size="64"
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
      <v-col
        cols="12"
        sm="4" 
        v-for="post in postData" 
        :key="post.data.id">
        <card :postData = "post" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Card from './Card'
export default {
  name: 'HelloWorld',
  components: {
    Card
  },
  data() {
    return {
      overlay: false,
      post:{},
      postData: {},
      postHobbits:{},
      picked: 'allPosts'
    }
  },
  watch: {
    picked: function(){
      switch (this.picked) {
        case 'allPosts':
            this.getPost()
          break;
        case 'Hobbits':
            this.filterHobbits(this.post)
          break;
        default:
          this.filterHobbit(this.post, this.picked)
          break;
      }
    }
  },
  created() {
   this.getPost()
  },
  methods: {
    getPost: async function () {
      this.overlay = true
      const response = await fetch("https://www.reddit.com/r/lotr/new.json?limit=100");
      const data = await response.json();
      this.post = data.data.children;
      this.postData = this.post
      this.overlay = false
      console.log(this.postData)
    },
    filterHobbits: function (data) {
      this.postHobbits = data.filter((item)=>{
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
      })
      console.log('data', this.postHobbits)
      this.postData = this.postHobbits
    },
    filterHobbit: function (data, hobbit) {
      const postHobbit = data.filter((item)=>{
          return item.data.selftext.includes(hobbit)
        })
        this.postData = postHobbit
        console.log('data', postHobbit)
    }
  },
}
</script>

