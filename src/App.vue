<template>
<div id="app">
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
    email: ''
    }
  },    
  mounted() {
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
      this.drinklist = results
      console.log(results)
    },
    login(status){
      this.signedIn = status
      this.getData()
    },
    update(){
      this.getData()
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fa7d09; 
  height: 100vh;
  /* background-color: #ffac81;
  background-image: linear-gradient(to left top, #ffac81 50%, #ff928b 50%); */
  background-image: url(assets/background.jpg);
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  background-attachment: fixed;
}
</style>
