<template>
<div id="app">
  <!-- <signin :signed-in="signedIn"/> -->
  <signin
  :signedIn="signedIn"
  @login="login($event)"
  />
  <drinks  
  :drinklist="drinklist"
  :url_base="url_base"
  @update="update()"
  />
</div>
</template>

<script>
import drinks from './components/drinks';
import signin from './components/signin';

export default {
  name: 'App',
  components: {
    drinks,
    signin
  },
  data() {
    return{
    signedIn: !!localStorage.getItem('authentication_token'),
    drinklist: [{}],
    // url_base: 'https://drink-log-backend.herokuapp.com/api/v1/drinks',
    url_base: 'http://localhost:3000/api/v1/drinks/',
    // new_drink: '',
    // category: '',
    email: ''
    }
  },    
  mounted() {
    console.log(this.signedIn)
    if (localStorage.getItem('authentication_token')) {  
    this.email = localStorage.getItem('email')
    this.getData()
    }
  },
  methods: {
    getData() {
      console.log("from Update")
      fetch(`${this.url_base}`, {
        headers: {
            'Content-Type': 'application/json',
            'X-User-Email': localStorage.getItem('email'),
            'X-User-Token': localStorage.getItem('authentication_token')
        }
    })
    .then( res =>  res.json())
    .then(this.setResults)
    },      
    setResults(results) {
      console.log(results)
      this.drinklist = results
      console.log(this.drinklist)
    },
    login(status){
      this.signedIn = status
      this.getData()
    },
    update(){
      this.getData()
      console.log("update")
      console.log(this.drinklist)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
