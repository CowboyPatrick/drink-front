<template>
    <h1>Drinks</h1>
    <form id="new_drink_form" @submit.prevent="createDrink">
        <label for="new_drink">Drink Name</label>
        <input type="text" name="new_drink" v-model="new_drink">
        <label for="drink_category">Category</label>
        <input type="text" for="drink_category" v-model="category">
        <button>Add a Drink</button>
    </form>
    <div class="card-drink" v-for="drink in drinklist" :key=drink.id>
        <h2>Name: {{ drink.name }}</h2>
        <h3>Category: {{ drink.category}}</h3>
    </div>
</template>
    
<script>

export default {
    name: 'drinks',
    props : {
        drinklist: {
            type: Array
        },
        url_base: {
            type: String
        },
        email: {
            type: String
        }
        // new_drink: {
        //     type: String
        // },
        // category: {
        //     type: String
        // }
    },
    data() {
        return {
            // drinklist: [{}],
            // // url_base: 'https://drink-log-backend.herokuapp.com/api/v1/drinks',
            // url_base: 'http://localhost:3000/api/v1/drinks',
            new_drink: '',
            category: ''
        }
    },
    methods:{

        createDrink () {
            if (this.new_drink != "" && this.category != ""){
            const requestOptions = {
                method: "POST",
                headers: { 'Content-Type': 'application/json',
                'X-User-Email': localStorage.getItem('email'),
                'X-User-Token': localStorage.getItem('authentication_token') },
                body: JSON.stringify({ name: this.new_drink, category: this.category })
            };
              fetch(this.url_base, requestOptions)
                .then(response => response.json())
                .then(this.setResults)
            }
                
        },
        setResults() {
            this.new_drink = ""
            this.category = ""
            this.$emit('set-results')
        }
    }
}
</script>

<style scoped>
.card-drink {
    border: 1px solid grey;
    margin: 0 auto;
    width: 400px;
    margin-bottom: 8px;
    box-shadow: -2px 2px;
}

#new_drink_form{
    display: flexbox;
}
</style>
