<template>
  <div id="edit-employee">
    <h3>Edit Employee</h3>
    <div class="row">
    <form @submit.prevent="updateEmployee" class="col s12">
      <div class="row">
        <div class="input-field col s12">Peg:
          <input type="text" v-model="peg" required>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">Name:
          <input type="text" v-model="name" required>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">Pass:
          <input type="text" v-model="pass" required>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">Choices:
          <input type="text" v-model="choices" required>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">Position:
          <input type="text" v-model="position" required>
        </div>
      </div>
      <button type="submit" class="btn">Submit</button>
      <router-link to="/" class="btn grey">Cancel</router-link>
    </form>
  </div>
  </div>
</template>

<script>
  import db from './firebaseInit'
  export default {
    name: 'edit-employee',
    data () {
      return {
        peg: null,
        name: null,
        pass: null,
        choices: null,
        position: null
      }
    },
    beforeRouteEnter (to, from, next) {
      db.collection('Operators').where('peg', '==', to.params.peg).get().then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
          next(vm => {
            vm.peg = doc.data().peg
            vm.name = doc.data().name
            vm.pass = doc.data().pass
            vm.choices = doc.data().choices
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
        db.collection('Operators').where('peg', '==', this.$route.params.peg).get().then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
            this.peg = doc.data().peg
            this.name = doc.data().name
            this.pass = doc.data().pass
            this.choices = doc.data().choices
            this.position = doc.data().position
          })
        })
      },
      updateEmployee () {
        db.collection('Operators').where('peg', '==', this.$route.params.peg).get().then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
            doc.ref.update({
              peg: this.peg,
              name: this.name,
              pass: this.pass,
              choices: this.choices,
              position: this.position
            })
            .then(() => {
              this.$router.push({ name: 'view-employee', params: { peg: this.peg }})
            });
          })
        })
      }
    }
  }
</script>