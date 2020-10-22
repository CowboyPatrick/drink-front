<template>
<div id="app">
  <!-- <signin :signed-in="signedIn"/> -->
  <signin />
  <drinks  
  :drinklist="drinklist"
  :url_base="url_base"
  :new_drink="new_drink"
  :category="category"
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
    url_base: 'http://localhost:3000/api/v1/drinks',
    new_drink: '',
    category: ''
    }
  },    
  mounted() {
    console.log('test')
    if (localStorage.getItem('authentication_token'))
    console.log('test2')
    fetch(`${this.url_base}`, {
        headers: {
            'Content-Type': 'application/json',
            'X-User-Email': 'doug@doug.com',
            'X-User-Token': localStorage.getItem('authentication_token')
        }
    })
    .then( res =>  res.json())
    .then(this.setResults)
  },
  methods: {      
    setResults(results) {
      console.log('test3')
      console.log(results)
      this.drinklist = results
      this.new_drink = ""
      this.category = ""
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
