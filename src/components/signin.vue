<template>
<h1>Sign in</h1>
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
    props: {
        // signedIn: Boolean
    },
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
            // .then(response => console.log(response.headers))
            .then(response => response.json())
            .then(data => {
                this.apiResponse = data
                localStorage.setItem('authentication_token', this.apiResponse.data.user.authentication_token)
                
                // this.$emit(signedIn)
                });
            this.email = ''
            this.password = ''
        }
    }
    
}
</script>