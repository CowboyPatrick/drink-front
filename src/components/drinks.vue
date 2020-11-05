<template>
    
    <form id="new_drink_form" @submit.prevent="createDrink">
        <div>
        <label for="new_drink">Drink Name:</label>
        <input type="text" name="new_drink" v-model="new_drink">
        </div>
        <div>
        <label for="drink_category">Category:</label>
        <input type="text" for="drink_category" v-model="category">
        </div>
        <button>Add a Drink</button>
    </form>
    <div class="drinks">
        <div class="card-drink" v-for="drink in drinklist" :key=drink.id>
            <h2>Name: {{ drink.name }}</h2>
            <h3>Category: {{ drink.category}}</h3>
            <button @click="destroy(drink.id)">Delete</button>
            <button @click="collapse(drink.id, $event)" class="collapsible">Reviews</button>
            <div class="content" >
                <div v-for="review in drink.reviews" :key=review.id>
                {{review.content}}
                </div> 
            </div>
        </div>
        
    </div>
</template>
    
<script>

export default {
    name: 'drinks',
    emits: ['update'],
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
    },
    data() {
        return {
            new_drink: '',
            category: '',
        }
    },
    methods:{

        createDrink() {
            if (this.new_drink != "" && this.category != ""){
            const requestOptions = {
                method: "POST",
                headers: { 'Content-Type': 'application/json',
                'X-User-Email': localStorage.getItem('email'),
                'X-User-Token': localStorage.getItem('authentication_token') },
                body: JSON.stringify({ name: this.new_drink, category: this.category })
            };
              fetch(this.url_base, requestOptions)
                .then(response => {
                    if (response){
                        this.$emit("update")
                    }
                })
                this.new_drink = ''
                this.category = ''
            }
                
        },
        destroy(id) {
            const requestOptions = {
                method: "DELETE",
                headers: { 'Content-Type': 'application/json',
                'X-User-Email': localStorage.getItem('email'),
                'X-User-Token': localStorage.getItem('authentication_token')},
                };
              fetch(`${this.url_base}${id}`, requestOptions)
                .then(response => {
                    if (response){
                        this.$emit("update")
                    }
                })

        },
        collapse(id, event) {
            // let coll = document.querySelectorAll('.content')
            if(event.target.nextElementSibling.style.display === 'block'){
                event.target.nextElementSibling.style.display = 'none'
            } else {
                event.target.nextElementSibling.style.display = 'block'
            }
        }
    }
}
</script>

<style scoped>
.card-drink {
    border: 1px solid #1a1c20;
    margin: 0 auto;
    width: 400px;
    margin-bottom: 8px;
    box-shadow: -2px 2px;
    background-color: #ffd5cd;
    border-radius: 8px;
}

.drinks {
    display: flex;
    flex-wrap: wrap;
}

#new_drink_form{
    /* display: flexbox;
    flex-direction: column; */
    margin: 0 auto;
    text-align: left;
    width: 400px;
}

/* Style the button that is used to open and close the collapsible content */
.collapsible {
  background-color: #eee;
  color: #444;
  cursor: pointer;
  padding: 18px;
  width: 400px;
  border: none;
  text-align: left;
  outline: none;
  font-size: 15px;
}

/* Add a background color to the button if it is clicked on (add the .active class with JS), and when you move the mouse over it (hover) */
.active, .collapsible:hover {
  background-color: #ccc;
}

/* Style the collapsible content. Note: hidden by default */
.content {
  padding: 0 18px;
  display: none;
  overflow: hidden;
  background-color: #f1f1f1;
}

/* input{
    text-align: right;
} */
</style>
