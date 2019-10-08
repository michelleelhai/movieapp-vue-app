<template>
  <div class="about">
    <div>
      Title:
      <input type="text" v-model="newTitle" />
      <br />
      Year:
      <input type="text" v-model="newYear" />
      <br />
      Plot:
      <input type="text" v-model="newPlot" />
      <br />
      Director:
      <input type="text" v-model="newDirector" />
      <br />
      English:
      <input type="text" v-model="newEnglish" />
      <br />
      <button v-on:click="createMovie()">Create Movie</button>
    </div>
    <div v-for="movie in movies">
      <h2>{{ movie.title }}</h2>
      <h2>{{ movie.year }}</h2>
      <h2>{{ movie.plot }}</h2>
      <button v-on:click="showMovie(movie)">Show more</button>
      <div v-if="currentMovie === movie">
        <h2>{{ movie.director }}</h2>
        <h2>{{ movie.english }}</h2>
        <div>
          Title:
          <input type="text" v-model="movie.title" />
          Year:
          <input type="text" v-model="movie.year" />
          Plot:
          <input type="text" v-model="movie.plot" />
          Director:
          <input type="text" v-model="movie.director" />
          English:
          <input type="text" v-model="movie.english" />
          <button v-on:click="updateMovie(movie)">Update Movie</button>
          <button v-on:click="destroyMovie(movie)">Destroy Movie</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      movies: [],
      newTitle: "",
      newYear: "",
      newPlot: "",
      newDirector: "",
      newEnglish: "",
      currentMovie: {}
    };
  },
  created: function() {
    axios.get("/api/movies").then(response => {
      this.movies = response.data;
    });
  },
  methods: {
    createMovie: function() {
      var params = {
        Title: this.newTitle,
        Year: this.newYear,
        Plot: this.newPlot,
        Director: this.newDirector,
        English: this.newEglish
      };
      axios.post("/api/movies", params).then(response => {
        this.movies.push(response.data);
        this.newTitle = "";
        this.newYear = "";
        this.newPlot = "";
        this.newDirector = "";
        this.newEnglish = "";
      });
    },
    showMovie: function(movie) {
      if (this.currentMovie === movie) {
        this.currentMovie = {};
      } else {
        this.currentMovie = movie;
      }
    },
    updateMovie: function(movie) {
      var params = {
        Title: movie.title,
        Year: movie.year,
        Plot: movie.plot,
        Director: movie.director,
        English: movie.english
      };
      axios.patch("/api/movies/" + movie.id, params).then(response => {
        this.currentMovie = {};
      });
    },
    destroyMovie: function(movie) {
      axios.delete("/api/movies/" + movie.id).then(response => {
        var index = this.movies.indexOf(movie);
        this.movies.splice(index, 1);
      });
    }
  }
};
</script>
