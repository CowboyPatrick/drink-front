<template>
    <h1>My Drinks</h1>
    {{ drinklist[0] }}
    {{ drinklist}}
</template>
    
<script>
export default {
    name: 'drinks',
    data() {
        return {
            drinklist: {},
            url_base: 'https://drink-log-backend.herokuapp.com/api/v1/drinks',
        }
    },
    created() {
        fetch(`${this.url_base}`)
        .then( res =>  res.json())
        .then(this.setResults)
    },
    methods:{
        setResults (results) {
            console.log(results)
            this.drinklist = results
        }
    }
}
</script>

<style scoped>

</style>
