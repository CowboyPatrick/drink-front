<template>
    <form id="new_drink_form" refs="drinkForm" enctype="multipart/form-data" @submit.prevent="createDrink(event)">
        <div>
        <label for="name">Drink Name:</label>
        <input type="text" name="drink[name]" v-model="drink">
        </div>
        <div>
        <label for="category">Category:</label>
        <input type="text" name="drink[category]" v-model="category">
        </div>
        <div>
        <label for="photo">Upload an Image:</label>
        <input
            type="file"
            name="drink[photo]"
            ref="inputFile" 
            accept="image/*"
        />
        </div>
        <button>Add a Drink</button>
    </form>
    <div class="drinks">
        <div v-for="(drink, index) in drinklist" 
        :key=drink.id class="card-drink" 
        v-bind:style="{backgroundImage: 'url(' + drinklist[index].image_url + ')'}">
            <h2>Name: {{ drink.name }}</h2>
            <h3>Category: {{ drink.category}}</h3>
            <button @click="destroy(drink.id)">Delete</button>
            <!-- <button @click="collapse(drink.id, $event)" class="collapsible">Reviews</button>
            <div class="content" >
                <div v-for="review in drink.reviews" :key=review.id>
                {{review.content}}
                </div> 
            </div> -->
        <!-- </div> -->
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
            drink: '',
            category: '',
            photo: null,
            formData: null,
            // drink: {}
        }
    },
    methods:{

        createDrink() {
            // const params = {
            // 'name': this.name,
            // 'category': this.category,
            // 'photo': this.photo
            // }

            let formData = new FormData(event.currentTarget)

            const requestOptions = {
                method: "POST",
                headers: { 
                "Accept": 'application/json',
                'X-User-Email': localStorage.getItem('email'),
                'X-User-Token': localStorage.getItem('authentication_token') },
                body: formData
            };
              fetch(this.url_base, requestOptions)
                .then(response => {
                    if (response){
                        this.$emit("update")
                    }
                })
                this.drink = ''
                this.category = ''
                this.$refs.inputFile.value = ''
            // }
                
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
        },
        // uploadFile(){
        //     this.photo = this.$refs.inputFile.files[0];
        // }
    }
}
</script>

<style scoped>
.card-drink {
    border: 1px solid #1a1c20;
    margin: 0 auto;
    width: 350px;
    height: 200px;
    margin-bottom: 8px;
    box-shadow: -2px 2px;
    /* background-color: #ffd5cd; */
    /* background-image: url(); */
    border-radius: 8px;
     background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
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
