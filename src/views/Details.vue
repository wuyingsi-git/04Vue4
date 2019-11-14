<template>
    <div class="details">
        <img :src="prod.imgurl" alt=""/>
        <div>
            <p>产品名称：{{prod.name}}</p>
            <p>产品价格：<span style="color: #ed4014;font-weight: bold;font-size: 18px">{{prod.price}}</span>元</p>
            <p>详细说明：{{prod.desc}}</p>
            <p><button type="primary" class="btn" @click="addCart()">点击购买</button></p>
        </div>
    </div>
</template>

<script>
    export default {
        name: "details",
        data(){
            return{
                prod:null
            }
        },
        created(){
            var id = this.$route.query.id;
            this.prod = this.getProdById(id);
        },
        methods:{
            getProById(id){
                var prod = this.$store.state.prodlist.find((item)=>{
                    return(item.id == id);
                });
                return prod;
            },
            addCart(){
                this.$store.commit('addCart',{
                    id:this.prod.id,
                    name:this.prod.name,
                    price:this.prod.price,
                    imgurl:this.prod.imgurl,
                    count:1
                });
                this.$router.push('/cart');
            }
        }
    }
</script>

<style scoped>
    .details{
        width:1100px; margin:20px auto;overflow:auto;
    }
    .details img{width:30%;float:left;}
    .details div{width:60%; float:right;}
    .details div p{line-height:3em;}
</style>