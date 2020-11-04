<template>
   <div id="view-employee">
       <ul class="collection with-hearder">
           <li class="collection-header">
               <h4>{{name}}</h4>
           </li>
           <li class="collection-item">PEG # {{peg}}</li>
           <li class="collection-item">Pass #{{pass}}</li>
           <li class="collection-item">Position: {{position}}</li>
       </ul>
       <input type="text">
       <router-link to="/" class="btn blue"> Leave Choices </router-link>
       <router-link to="/" class="btn grey"> Back </router-link>
       <button @click="deleteEmployee" class="btn red"> Delete </button>
       
   </div> 
   
</template>

<script>
import db from './firebaseInit'
export default {
    name: 'view-employee',
    data (){
        return {
        peg: null,
        name: null,
        pass: null,
        position: null
            
        }
    },
    beforeRouteEnter (to, from, next){
        db.collection('Operators').where ('peg', '==', to.params.peg).get().then(querySnapshot => {
            querySnapshot.forEach(doc => {
                next(vm => {
                    vm.peg = doc.data().peg
                    vm.name = doc.data().name
                    vm.pass = doc.data().pass
                    vm.position = doc.data().position
                })
            })
        })

    },
    watch: {
        '$route': 'fetchData'
    },
    methods: {
        fetchData () {
            db.collection('Operators').where('peg','==', this.$route.params.peg).get()
            .then(querySnapshot => {
                querySnapshot.forEach(doc => {
                    this.peg = doc.data().peg
                    this.name = doc.data().name
                    this.pass = doc.data().pass
                    this.position = doc.data().position
                })
            })
        },
        deleteEmployee () {
        if(confirm ('Are you sure?')) {
          db.collection('Operators').where('peg', '==', this.$route.params.peg).get().then((querySnapshot) => {
            querySnapshot.forEach((doc) => {
              doc.ref.delete();
              this.$router.push('/')
            })
          })
        }
      }

    }
    

}
</script>