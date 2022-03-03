<script>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup

export default {

  data() {
    return {
      word: {id:0, word:"hebrews"},
      numberWordsRated: 0,
      words: []
    }
  },
  methods:{
    async loadNewWord() {
      const result = await fetch("http://localhost:3000/api/words/random");
      const word = await result.json();



      this.numberWordsRated++;
      //alert(word.word);

      this.word = word;
      this.words.push(word.word.slice(0, 7));
    },

    async submitRating(wordId, rating) {
      const result = await fetch(`http://localhost:3000/api/words/${wordId}/ratings`, 
      {
        method:"post", 
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify( {rating: rating} )
      });


    }
  },
  mounted() {
    this.loadNewWord();
  }
}

</script>

<template>

  <h1>Scripturater</h1>
  <h5>A random word will be put on the screen for you to rate. Please choose the like, dislike, or neutral faces to rate the word. Once you rate the word, a new word will be shown for you to rate. </h5>

  <h4>Word to rate:</h4>
  <h2> {{word.word}}</h2>


  <a href="" @click.prevent="submitRating(word.id, 'like'); loadNewWord()">🙁</a>
  <a href="" @click.prevent="submitRating(word.id, 'neutral'); loadNewWord()">😐</a>
  <a href="" @click.prevent="submitRating(word.id, 'dislike'); loadNewWord()">🙂</a>

  <h3>Number of Words Rated: {{numberWordsRated}}</h3>
  <h5> Words you have rated:</h5>
  <p>{{words}}</p>

</template>

<style>

body {
  font-size: 1.5em;
  background-color: black;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #e8ebee;

  margin-top: 60px;
}

a {
  font-size: 8em;
  text-decoration: none;
}
</style>
