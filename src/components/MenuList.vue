<template>
    <div>
        <ul>
            <li v-for="product in products" :key="product.id">
                {{product.id}} | {{product.name}} | 
                <button v-on:click="deleteProduct(product.id)">Delete</button>
            </li> 
        </ul>
        <div>
            <button v-on:click="loadProducts()">Load products</button>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            products:[]
        }
    },
    methods:{
        deleteProduct(id){

            let index = this.products.findIndex(item => item.id==id);

            if(index!=-1)
            {
                this.products.splice(index, 1);
            }
        },
        async loadProducts(){
            let response = await fetch("https://localhost:44368/api/Products");

            if(response.ok==true)
            {
                this.products = await response.json();
            }
            else
            {
                console.log("errrror");
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