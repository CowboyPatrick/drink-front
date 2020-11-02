<template>  
<h1>Sign in</h1>
    <form id="sign_in_form" @submit.prevent="logIn" v-if="signedIn === false">
        <label for="email">Email: </label>
        <input type="text" name="email" v-model="email">
        <label for="password">Password: </label>
        <input type="password" name="password" v-model="password">
        <button>Log In</button>
    </form>
    <button v-else @click="logOut">Log Out</button>
</template>

<script>
export default {
    name: 'signin',
    props: {
        signedIn: Boolean
    },
    data() {
        return{
        email: '',
        password: '',
        sign_in_url: 'http://localhost:3000/api/v1/sign_in',
        apiResponse: {},
        }
    },
    emits: ['login'],
    methods:{
        logIn() {
            const requestOptions = {
            method: "POST",
            headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({user: { email: this.email, password: this.password }})
            };
            fetch(this.sign_in_url, requestOptions)
            .then(response => response.json())
            .then(data => {
                this.apiResponse = data
                localStorage.setItem('authentication_token', this.apiResponse.data.user.authentication_token)
                localStorage.setItem('email', this.apiResponse.data.user.email)
                this.$emit('login', true)
                });
            this.email = ''
            this.password = ''
        },
        logOut() {
            localStorage.clear()
            this.$emit('login', false)
        },
    }
    
}
</script>