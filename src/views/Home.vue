<template>
  <div class="home">
    <h1>All Actors</h1>
    <div v-for="actor in actors">
      <h2>{{ actor.first_name }}</h2>
      <h2>{{ actor.last_name }}</h2>
      <h2>{{ actor.gender }}</h2>
      <h2>{{ actor.known_for }}</h2>
      <button v-on:click="showActor(actor)">Show more</button>
      <button v-on:click="destroysctor(actor)">destroy</button>

      <div v-if="currentActor === actor">
        <h2>{{ actor.age }}</h2>
        <h2>{{ actor.movie_plot }}</h2>
      </div>
    </div>
    <div>
      First_name:
      <input type="text" v-model="actor.first_name" />
      <br />
      Last_name: <input type="text" v-model="actor.last_name />
      <br />
      Gender:
      <input type="text" v-model="actor.gender" />
      <br />
      Known For:
      <input type="text" v-model="actor.known_for" />
      <br />
      Age:
      <input type="text" v-model="actor.age" />
      <br />
      Movie Plot:
      <input type="text" v-model="actor.movieplot" />
      <br />
      <button v-on:click="updateActor(actor)">Update Photo</button>
    </div>
    <div>
      First_name:
      <input type="text" v-model="newFirstName" />
      <br />
      Last_name:
      <input type="text" v-model="newLastName" />
      <br />
      Gender:
      <input type="text" v-model="newGender" />
      <br />
      Known For:
      <input type="text" v-model="newKnownFor" />
      <br />
      Age:
      <input type="text" v-model="newAge" />
      <br />
      Movie Plot:
      <input type="text" v-model="newMoviePlot" />
      <br />
      <button v-on:click="createActor()"></button>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      actors: [],
      newFirstName: "",
      newLastName: "",
      newGender: "",
      newKnownFor: "",
      newAge: "",
      newMoviePlot: "",
      currentActor: {}
    };
  },
  created: function() {
    axios.get("/api/actors").then(response => {
      this.actors = response.data;
    });
  },
  methods: {
    createActor: function() {
      var params = {
        first_name: this.newFirstName,
        last_name: this.newLastName,
        Gender: this.newGender,
        knownfor: this.newKnownFor,
        age: this.newAge,
        MoviePlot: this.newMoviePlot
      };
      axios.post("/api/actors", params).then(response => {
        this.actors.push(response.data);
        this.newFirstName = "";
        this.newLastName = "";
        this.newGender = "";
        this.newKnownFor = "";
        this.newAge = "";
        this.newMoviePlot = "";
      });
    },
    showActor: function(actor) {
      if (this.currentActor === actor) {
        this.currentActor = {};
      } else {
        this.currentActor = actor;
      }
    },
    updateActor: function(actor) {
      var params = {
        first_name: actor.first_Name,
        last_name: actor.first_name,
        Gender: actor.gender,
        knownfor: actor.knownFor,
        age: actor.age,
        MoviePlot: actor.MoviePlot
      };
      axios.patch("/api/actors/" + actor.id + params).then(response => {
        this.currentActor = {};
      });
    },
    destroyActor: function(actor) {
      axios.delete("/api/actors/" + actor.id).then(response => {
        var index = this.actors.indexOf(actor);
        this.actors.splice(index, 1);
      });
    }
  }
};
</script>
