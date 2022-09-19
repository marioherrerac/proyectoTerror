<!-- bookDetails.vue -->
<template>
           <div class="row">
            <div class="eleven column" style="margin-top: 5%">
             <h2>{{title}}</h2>
              <form>
              <div class="row">
               <div class="six columns">
                <label for="titleInput">Title</label>
                <input class="u-full-width" type="text"
                  v-model="movie.title">
               </div>
               <div class="six columns">
                <label for="editionInput">Country</label>
                <input class="u-full-width" type="text"
                   v-model="movie.country">
               </div>
               <div class="six columns">
                <label for="copyrightInput">Year</label>
                <input class="u-full-width" type="text"
                   v-model="movie.year">
               </div>
              </div>
              <div class="row">
               <div class="six columns">
                <label for="emailInput">Director</label>
                <input class="u-full-width" type="email"
                   v-model="movie.director">
               </div>
               <div class="six columns">
                <label for="phoneInput">Producer</label>
                <input class="u-full-width" type="tel"
                  v-model="movie.producer">
               </div>
               <router-link class="button button-primary" 
                 to="/movie">Back</router-link>
               <a v-if='edit' class="button button-primary" style="float: right"
                  v-on:click="updateMovie()">Update</a>
               <a v-if='create' class="button button-primary" style="float: right"
                  v-on:click="createMovie()">Create</a>
              </div>
             </form>
            </div>
           </div>
         </template>
         <script>


import { useRoute } from 'vue-router'

         export default {
           props: ['create','edit','show'],
           data: function() {
             return {
               title: "Movie Data",
               prof: {'title':'','country':'','year':'','language':'','duration':'','director':"",'producer':''},
             }
           },
           created () {
            const route = useRoute();  
            this.findMovie(route.params.id)
           },
           methods: {
             findMovie: function(id) {
              fetch('/.netlify/functions/movies/'+id,
        { headers: {'Accept': 'application/json'}})
        .then((response) => response.json())
        .then((result) => {
          this.movie = result;
        })
             },
             updateMovie: function() {
              this.prof['_method'] = 'PUT';
      const route = useRoute(); 
      var id = route.params.id;
      fetch('/.netlify/functions/movies/'+id,
        { headers: {'Content-Type':'application/json'},
          method: 'POST',
          body: JSON.stringify(this.movie)})
        .then((data) => {
          router.push('/movie');
        }
      )
             },
             createMovie: function() {
               fetch('/.netlify/functions/movies',
                 { headers: {'Content-Type':'application/json'},
                   method: 'POST',
                   body: JSON.stringify(this.movie)})
                 .then((data) => {
                    router.push('/movie');
                 }
               )
             }
           }
         }
         </script>