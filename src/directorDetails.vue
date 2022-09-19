<!-- bookDetails.vue -->
<template>
           <div class="row">
            <div class="eleven column" style="margin-top: 5%">
             <h2>{{title}}</h2>
              <form>
              <div class="row">
               <div class="six columns">
                <label for="titleInput">Name</label>
                <input class="u-full-width" type="text"
                  v-model="director.director_name">
               </div>
               <div class="six columns">
                <label for="editionInput">Nationality</label>
                <input class="u-full-width" type="text"
                   v-model="director.nationality">
               </div>
               <div class="six columns">
                <label for="copyrightInput">Birth Year</label>
                <input class="u-full-width" type="text"
                   v-model="director.birth_year">
               </div>
              </div>
              <div class="row">
               <div class="six columns">
                <label for="emailInput">Fields</label>
                <input class="u-full-width" type="email"
                   v-model="director.fields">
               </div>
               <div class="six columns">
                <label for="phoneInput">Movies</label>
                <input class="u-full-width" type="tel"
                  v-model="director.movies.title">
               </div>
               <router-link class="button button-primary" 
                 to="/director">Back</router-link>
               <a v-if='edit' class="button button-primary" style="float: right"
                  v-on:click="updateDirector()">Update</a>
               <a v-if='create' class="button button-primary" style="float: right"
                  v-on:click="createDirector()">Create</a>
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
               title: "Director Data",
               prof: {'director_name':'','nationality':'','birth_year':'','fields':'','title':''},
             }
           },
           created () {
            const route = useRoute();  
            this.findDirector(route.params.id)
           },
           methods: {
             findDirector: function(id) {
              fetch('/.netlify/functions/director/'+id,
        { headers: {'Accept': 'application/json'}})
        .then((response) => response.json())
        .then((result) => {
          this.director = result;
        })
             },
             updateDirector: function() {
              this.prof['_method'] = 'PUT';
      const route = useRoute(); 
      var id = route.params.id;
      fetch('/.netlify/functions/director/'+id,
        { headers: {'Content-Type':'application/json'},
          method: 'POST',
          body: JSON.stringify(this.director)})
        .then((data) => {
          router.push('/director');
        }
      )
             },
             createDirector: function() {
              fetch('/.netlify/functions/director',
                 { headers: {'Content-Type':'application/json'},
                   method: 'POST',
                   body: JSON.stringify(this.director)})
                 .then((data) => {
                    router.push('/director');
                 }
               )
             }
           }
         }
         </script>