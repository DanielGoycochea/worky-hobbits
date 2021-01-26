<template>
  <v-container>
    <v-overlay :value="overlay">
      <v-progress-circular
        indeterminate
        size="64"
      ></v-progress-circular>
    </v-overlay>
    <div class="">
      <p>Ordenar por:</p>
      <div>
        <input type="radio" id="all" value="allPosts" v-model="picked">
        <label for="all">Todos los post</label>
      </div>
      <div>
        <input type="radio" id="Hobbits" value="Hobbits" v-model="picked">
        <label for="Hobbits">Todos los Hobbits</label>
      </div>
      <div>
        <p>Ordrenar por Hobbit:</p>
        <div>
          <input type="radio" id="Gollum" value="Gollum" v-model="picked">
          <label for="Gollum">Gollum</label>
          <input type="radio" id="Frodo" value="Frodo" v-model="picked">
          <label for="Frodo">Frodo</label>
          <input type="radio" id="Bilbo" value="Bilbo" v-model="picked">
          <label for="Bilbo">Bilbo</label>
          <input type="radio" id="Sam" value="Sam" v-model="picked">
          <label for="Sam">Sam</label>
          <input type="radio" id="Merry" value="Merry" v-model="picked">
          <label for="Merry">Merry</label>
          <input type="radio" id="Pippin" value="Pippin" v-model="picked">
          <label for="Pippin">Pippin</label>
        </div>
      </div>
    </div>
    <v-row>
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
            this.filterHobbies(this.post)
          break;
        default:
          this.filterHobbit(this.post, this.picked)
          break;
      }
    }
  },
  created() {
  // GET request using fetch with async/await
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
    filterHobbies: function (data) {
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

