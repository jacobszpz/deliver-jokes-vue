<template>
  <div class="container mt-3">
    <h1>Joke Generator</h1>
    <div class="form-group">
      <label for="jokeCount">Number of Jokes:</label>
      <input type="number" class="form-control" id="jokeCount" v-model="jokeCount" />
    </div>
    <div class="form-group">
      <label for="jokeType">Type of Jokes:</label>
      <select class="form-control" id="jokeType" v-model="jokeType">
        <option v-for="type in jokeTypes" :value="type">{{ type }}</option>
      </select>
    </div>
    <button class="btn btn-primary" @click="getJokes">Get Jokes</button>
    <hr>
    <div v-if="jokes.length">
      <h2>Jokes:</h2>
      <ul>
        <li v-for="(joke, index) in jokes" :key="index">{{ joke.setup }} {{ joke.punchline }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'JokeGenerator',
  data() {
    return {
      jokeCount: 1,
      jokeType: '',
      jokeTypes: [],
      jokes: [],
    };
  },
  created() {
    axios.get('http://localhost:3000/jokes/types')
      .then(response => {
        this.jokeTypes = response.data.map(jokeType => {
          return jokeType.name
        });
      }).catch(error => {
        console.error(error);
      });
  },
  methods: {
    getJokes() {
      axios.get(`http://localhost:3000/jokes?count=${this.jokeCount}&type=${this.jokeType}`)
        .then(response => {
          this.jokes = response.data;
        })
        .catch(error => {
          console.error(error);
        });
    },
  },
};
</script>
