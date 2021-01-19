<template>
    <div>
        <ul>
            <li v-for="product in products" :key="product.id">
                {{product.id}} | {{product.name}} | 
                <button v-on:click="deleteProduct(product.id)">Delete</button>
            </li> 
        </ul>
        <div>
            <button v-on:click="getProducts()">Load products</button>
        </div>

        <div>
            <label><input type="text" v-model="product.name"></label>
        </div>

        <div>
            <button v-on:click="postProduct()">Add new product</button>
        </div>

    </div>
</template>

<script>
export default {
    data(){
        return{
            products:[],

            product:{
                name:"",
                IdCategory:1
            }
        }
    },
    methods:{
        async getProducts()
        {
            try
            {
                let response = await fetch("https://localhost:44368/api/Products",{
                    method:"GET"
                });

                if(response.ok==true)
                {
                    this.products = await response.json();
                }
                else
                {
                    alert(response.status+": "+response.statusText)
                }
            }
            catch(error)
            {
                console.log("Error: "+error);
            }
        },

        async deleteProduct(id)
        {
            try
            {
                let response = await fetch("https://localhost:44368/api/Products/"+id,{
                    method:"DELETE"
                });

                if(response.ok==true)
                {
                    let recievedId = await response.json();

                    //this.products.filter(item=>item.id != recievedId);

                    let index = this.products.findIndex(item => item.id==recievedId);
                    this.products.splice(index, 1);                   
                }
                else
                {
                    alert(response.status+": "+response.statusText)
                }
            }
            catch (error)
            {
                console.log("Error: "+error);
            }            
        },

        async postProduct()
        {
            try
            {
                let response = await fetch("https://localhost:44368/api/Products/",{
                    method:"POST",
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(this.product)
                });

                if(response.ok==true)
                {
                    let recievedProduct = await response.json();

                    this.products.push(recievedProduct);

                    this.product.name = "";
                }
                else
                {
                    alert(response.status+": "+response.statusText)
                }
            }
            catch (error) 
            {
                console.log("Error: "+error);   
            }
        }
    }
}
</script>

<style scoped>
    li{
        color:royalblue;
    }
</style>>