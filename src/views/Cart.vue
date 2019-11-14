<template>
    <div class="cart">
        <nav>
            <router-link to="/productlist">继续购物</router-link>
            <span>当前用户：{{this.$store.state.loginuser}}</span>
        </nav>
        <table border="1" cellspacing="0" cellpadding="0">
            <thead>
                <tr>
                    <th>序号</th>
                    <th>产品图片</th>
                    <th>产品名称</th>
                    <th>产品单价</th>
                    <th>产品数量</th>
                    <th>小计</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item,index) in this.$store.state.carlist" :key="index">
                    <td>{{index+1}}</td>
                    <td><img :src="item.imgurl"/></td>
                    <td>{{item.name}}</td>
                    <td>{{item.price}}</td>
                    <td>
                        <button @click="modify(index,1)">+</button>
                        <input type="text" v-model="item.count"/>
                        <button @click="modify(index,-1)">-</button>
                    </td>
                    <td>{{item.price*item.count}}</td>
                    <td><button @click="del(index)">删除</button></td>
                </tr>
            </tbody>
            <tfoot>
                <tr>
                    <td colspan="7">总计：{{total}}</td>
                </tr>
            </tfoot>
        </table>
    </div>
</template>

<script>
    export default {
        name: "cart",
        computed:{
            total(){
                let total = 0;
                this.$store.state.cartlist.forEach((item)=>{
                    total += item.price*item.count;
                })
                return total;
            }
        },
        methods:{
            modify(index,n){
                this.$store.commit('modifyCartCount',{index:index,num:n});
            },
            del(index){
                this.$store.commit('deleteCart',index);
            }
        }
    }
</script>

<style scoped>
    .cart{width:1100px; margin:0 auto;}
    .cart nav{width:90%; height:30px; margin:0 auto; display:flex; justify-content: space-between; align-items: center;}
    .cart table{width:90%;border-collapse: collapse; margin:0  auto;}
    .cart thead td{font-weight:900;text-align:center;}
    .cart tbody td{text-align:center;}
    .cart tr td img{width:100px; }
    .cart tr td input{width:30px;}
</style>