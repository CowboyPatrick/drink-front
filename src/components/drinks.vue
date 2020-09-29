<template>
    <h1>My Drinks</h1>
    <div class="card-drink" v-for="drink in drinklist" :key=drink.id>
        <h2>Name: {{ drink.name }}</h2>
        <h3>Category: {{ drink.category}}</h3>
    </div>
</template>
    
<script>
export default {
    name: 'drinks',
    data() {
        return {
            drinklist: [{}],
            url_base: 'https://drink-log-backend.herokuapp.com/api/v1/drinks',
        }
    },
    mounted() {
        fetch(`${this.url_base}`)
        .then( res =>  res.json())
        .then(this.setResults)
    },
    methods:{
        setResults (results) {
            console.log(results)
            this.drinklist = results
            console.log(this.drinklist[0].name);
        }
    }
}
</script>

<style scoped>
.card-drink {
    border: 1px solid grey;
    margin: 0 auto;
    width: 400px;
}

</style>
