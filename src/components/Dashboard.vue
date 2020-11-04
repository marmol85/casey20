<template>
   <div id="dashboard">
       <ul class="collection with-header">
           <li class="collection-header">
               <h4>Bus Operators</h4>
           </li>
           <li v-for="Operators in Operators" 
           v-bind:key="Operators.id"
           class="collection-item"> <div class="chip">
               {{Operators.peg}}
           </div>
           {{Operators.name}}: {{Operators.pass}} 
           <router-link class="secondary-content" v-bind:to="{name:'view-employee', params: {peg: Operators.peg} }">
               <svg width="2em" height="2em" viewBox="0 0 16 16" class="bi bi-eye-fill" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
  <path d="M10.5 8a2.5 2.5 0 1 1-5 0 2.5 2.5 0 0 1 5 0z"/>
  <path fill-rule="evenodd" d="M0 8s3-5.5 8-5.5S16 8 16 8s-3 5.5-8 5.5S0 8 0 8zm8 3.5a3.5 3.5 0 1 0 0-7 3.5 3.5 0 0 0 0 7z"/>
</svg>
           </router-link>
           </li>
          
       </ul>
    <div class="fixed-action-btn">
        <router-link to= "/new" >
        <svg width="8em" height="8em" viewBox="0 0 16 16" class="bi bi-patch-plus-fll" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
  <path fill-rule="evenodd" d="M10.067.87a2.89 2.89 0 0 0-4.134 0l-.622.638-.89-.011a2.89 2.89 0 0 0-2.924 2.924l.01.89-.636.622a2.89 2.89 0 0 0 0 4.134l.637.622-.011.89a2.89 2.89 0 0 0 2.924 2.924l.89-.01.622.636a2.89 2.89 0 0 0 4.134 0l.622-.637.89.011a2.89 2.89 0 0 0 2.924-2.924l-.01-.89.636-.622a2.89 2.89 0 0 0 0-4.134l-.637-.622.011-.89a2.89 2.89 0 0 0-2.924-2.924l-.89.01-.622-.636zM8.5 6a.5.5 0 0 0-1 0v1.5H6a.5.5 0 0 0 0 1h1.5V10a.5.5 0 0 0 1 0V8.5H10a.5.5 0 0 0 0-1H8.5V6z"/>
</svg>
        </router-link>
    </div>

   </div> 
</template>


<script>
  import db from './firebaseInit'
  export default {
    name: 'dashboard',
    data () {
      return {
        Operators: [],
        loading: true
      }
    },
   
    created () {
      db.collection('Operators').orderBy('peg').get().then((querySnapshot) => {
        this.loading = false
        querySnapshot.forEach((doc) => {
          const data = {
            'id': doc.id,
            'peg': doc.data().peg,
            'name': doc.data().name,
            'pass': doc.data().pass,
            'choices': doc.data().choices,
            'position': doc.data().position
          }
          this.Operators.push(data)
        })
      })
    }
  }
</script>