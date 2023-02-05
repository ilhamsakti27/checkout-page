<template>
    <div class="container">
        <div>
            <h1>Semua Produk</h1>
            <table class="table table-striped table-hover">
                <thead>
                    <th>Name</th>
                    <th>Description</th>
                    <th>Stock</th>
                    <th>Price</th>
                    <th></th>
                </thead>
                <tbody>
                    <tr v-for="(item,index) in listProduk" :key="index">
                        <td>{{ item.name }}</td>
                        <td>{{ item.description }}</td>
                        <td>{{ item.stock }}</td>
                        <td>{{ item.price }}</td>
                        <td>
                            <button 
                                type="button" 
                                @click="addCart(item)" 
                                class="btn btn-primary"
                                :disabled="item.stock === 0"
                            >
                                Add to chart
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <div>
            <h1>Keranjang Belanja</h1>
            <table class="table table-striped table-hover">
                <thead>
                    <th>Name</th>
                    <th>Quantity</th>
                    <th>Price</th>
                    <th></th>
                </thead>
                <tbody>
                    <tr v-for="item in listKeranjang" :key="item.index">
                        <td>{{ item.name }}</td>
                        <td>{{ item.stock }}</td>
                        <td>{{ item.price }}</td>
                        <td><button type="button" class="btn btn-danger" v-on:click="deleteItem(item.index)">Delete</button></td>    
                    </tr>
                    <tr>
                        <td colspan="2">Total:</td>
                        <td>Rp. {{ total }}</td>
                    </tr>
                </tbody>
            </table>
        </div>

        <div>
            <button type="button" class="btn btn-success" v-on:click="checkout">Checkout</button>
        </div>
    </div>
</template>

<script>
import { assertDebuggerStatement, functionExpression } from '@babel/types';

    export default {
        data() {
            return{
                listProduk: [
                    {
                        index: 1,
                        name: "Indomie Goreng Rendang",
                        description: "Masakan instan terenak di dunia",
                        stock: 10,
                        price: 3900
                    },
                    {
                        index: 2,
                        name: "Mie Gelas Rendang",
                        description: "Mie instant khusus anak kosan",
                        stock: 3,
                        price: 1500
                    },
                    {
                        index: 3,
                        name: "Bakmi mewah",
                        description: "Kalau anak kosan jangan macam2 deh",
                        stock: 80,
                        price: 10000
                    }
                ],
                listKeranjang: [],
                total: 0,
                totalPembayaran: 0
            }
        },
        methods: {
            alert: function() {
                alert("Haii");
            },
            addCart: function(item) {
                const productIndex = this.listKeranjang.findIndex((produk) => {
                    if (produk.index === item.index) {
                        return true
                    } else {
                        return false
                    }
                })

                

                if (productIndex !== -1) {
                    this.listKeranjang[productIndex].stock = this.listKeranjang[productIndex].stock + 1;
                    this.total = this.total + this.listKeranjang[productIndex].price;
                } else {
                    let jumlahSementara = item.price;
                    this.total = this.total + jumlahSementara;
                    this.listKeranjang.push({
                        ...item,
                        stock: 1
                    });
                }

                // Remove stock
                this.listProduk = this.listProduk.map((barang) => {
                    if(item.index === barang.index) {
                        return {
                            ...barang,
                            stock: barang.stock - 1
                        }
                    }
                    else {
                        return barang;
                    }
                });

                // this.listKeranjang.push({
                //     index: item.index,
                //     stock: item.sto
                // });
                
            },
            deleteItem: function(index) {
                this.listKeranjang = this.listKeranjang.filter(item => {
                    if(item.index == index) {
                        const indexItem = this.listProduk.findIndex(produk => {
                            if(produk.index === index) {
                                return true;
                            }
                            else {
                                return false;
                            }
                        })
                        this.listProduk[indexItem].stock += item.stock 
                        return false;
                    }
                    else {
                        return true;
                    }
                })

                let jumlah = 0;
                for(let item of this.listKeranjang) {
                    jumlah = jumlah + item.price * item.stock;
                }
                
                this.total = jumlah;
            },
            checkout: function() {
                alert("Total Pembayaran " + this.total);
            }
        },

        computed: {
            formatKeranjang() {
                
            }
        }
    }
</script>
