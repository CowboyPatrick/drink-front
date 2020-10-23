<template>
<div id="app">
  <!-- <signin :signed-in="signedIn"/> -->
  <signin/>
  <drinks  
  :drinklist="drinklist"
  :url_base="url_base"
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
    // signedIn: false
    // hello: 'hello world',
    drinklist: [{}],
    // url_base: 'https://drink-log-backend.herokuapp.com/api/v1/drinks',
    url_base: 'http://localhost:3000/api/v1/drinks/',
    // new_drink: '',
    // category: '',
    email: ''
    }
  },    
  mounted() {
    console.log('test')
    if (localStorage.getItem('authentication_token')) {  
    this.email = localStorage.getItem('email')
    this.getData()
    }
  },
  methods: {
    getData() {
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
    },
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
