<!-- src/bookIndex.vue -->
<template>
           <div class="row">
            <div style="margin-top: 5%">
              <h2>{{title}}</h2>
              <table><thead>
               <tr><th>Name</th><th>Country</th><th>Founded</th>
                   <th>Genere</th><th>Movies</th></tr>
               </thead><tbody>
               <tr v-for='producer in producers' :key="producer.id"><td>{{producer.producer_name}}</td>
               <td>{{producer.country}}</td>
               <td>{{producer.founded}}</td>
               <td>{{producer.genere}}</td>
               <td>
                      <ul>
                              <li v-for='movies in producers.movies' :key="movie_id"> {{producer.movies.title}}
                      </li>    
                      </ul>
                     
           </td>
               <td>
               <router-link class="button"
                 :to="'/producer/show/'+producer.id">Show</router-link>
               <router-link class="button"
                 :to="'/producer/edit/'+producer.id">Edit</router-link>
               <a class="button"
                 v-on:click="deleteProducer(producer.id)">Erase</a>
               </td>
               </tr></tbody>
              </table>
              <router-link class="button button-primary" 
                to="/producer/create">New</router-link>
            </div>
           </div>
         </template>
         <script>
         export default {
          data() {
            return {
             producers: [],
              title: 'ProducerList'
    }
  },
           methods: {
             allProducer() {
              fetch('/.netlify/functions/producers',
                 { headers: {'Accept': 'application/json'}})
                 .then((response) => response.json())
                 .then((result) => {
                   this.producers = data;
                 })
              },
              deleteProducer(id) {
                fetch('/.netlify/functions/producers/'+id,
                  { headers: {'Content-Type': 'application/json'},
                              method: 'POST',
                    body: JSON.stringify({'_method':'DELETE'})})
                  .then((result) => {
                   this.allProducer();
                   }
                 )
              }
           },
           mounted(){
            this.allProducer()
           }
         }
         </script>