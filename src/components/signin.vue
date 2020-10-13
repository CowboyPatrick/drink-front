<template>
<h1>Sign in test</h1>
    <form id="sign_in_form" @submit.prevent="logIn">
        <label for="email">Email: </label>
        <input type="text" name="email" v-model="email">
        <label for="password">Password: </label>
        <input type="password" name="password" v-model="password">
        <button>Log In</button>
    </form>
</template>

<script>
export default {
    name: 'signin',
    data() {
        return{
        email: '',
        password: '',
        url_base: 'http://localhost:3000/api/v1/sign_in',
        apiResponse: {}
        }
    },
    methods:{
        logIn() {
            const requestOptions = {
            method: "POST",
            headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({user: { email: this.email, password: this.password }})
            };
            fetch(this.url_base, requestOptions)
            .then(response => response.json())
            .then(data => {
                this.apiResponse = data
                console.log(data)
                });
            this.email = ''
            this.password = ''
        }
    }
    
}
</script>