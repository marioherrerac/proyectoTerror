<!-- src/bookIndex.vue -->
<template>
           <div class="row">
            <div style="margin-top: 5%">
              <h2>{{title}}</h2>
              <table><thead>
               <tr><th>Name</th><th>Nationality</th><th>Birth Year</th>
                   <th>Fields</th><th>Movies</th></tr>
               </thead><tbody>
               <tr v-for='director in directors' :key="director.id"><td>{{director.director_name}}</td>
               <td>{{director.nationality}}</td>
               <td>{{director.birth_year}}</td>
               <td>{{director.fields}}</td>
               <td>
                      <ul>
                              <li v-for='director in directors' :key="director.movies.movie_id"> {{director.movies.title}}
                      </li>    
                      </ul>
                     
           </td>
               <td>
               <router-link class="button"
                 :to="'/director/show/'+director.id">Show</router-link>
               <router-link class="button"
                 :to="'/director/edit/'+director.id">Edit</router-link>
               <a class="button"
                 v-on:click="deleteDirector(director.id)">Erase</a>
               </td>
               </tr></tbody>
              </table>
              <router-link class="button button-primary" 
                to="/director/create">New</router-link>
            </div>
           </div>
         </template>
         <script>
         export default {
          data() {
            return {
             directors: [],
              title: 'DirectorList'
    }
  },
           methods: {
             allDirector() {
               fetch('/.netlify/functions/director',
                 { headers: {'Accept': 'application/json'}})
                 .then((response) => response.json())
                 .then((result) => {
                   this.directors = result;
                 })
              },
              allDirMovie() {
               fetch('/.netlify/functions/director/'+id+'/movies',
                 { headers: {'Accept': 'application/json'}})
                 .then((response) => response.json())
                 .then((result) => {
                   this.directors.movies = result;
                 })
              },
              deleteDirector(id) {
                fetch('/.netlify/functions/director/'+id,
                  { headers: {'Content-Type': 'application/json'},
                              method: 'POST',
                    body: JSON.stringify({'_method':'DELETE'})})
                  .then((result) => {
                   this.allDirector();
                   }
                 )
              }
           },
           mounted(){
            this.allDirector()
           }
         }
         </script>