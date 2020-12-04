<template>
    <div class="food-ddetail">
        <Navbar/>
        <div class="container">
            <!-- breadcrumb -->
            <div class="row mt-4">
                <div class="col">
                    <nav aria-label="breadcrumb">
                    <ol class="breadcrumb p-0" style="background-color: transparent">
                        <li class="breadcrumb-item">
                            <router-link to="/" class="text-dark">Home</router-link>
                        </li>
                        <li class="breadcrumb-item">
                            <router-link to="/foods" class="text-dark">Foods</router-link>
                        </li>
                        <li class="breadcrumb-item active font-weight-bold text-dark" aria-current="page">Food Order</li>
                    </ol>
                    </nav>
                </div>
            </div>
            <!-- end breadcrumb -->

            <!-- food detail -->
            <div class="row mt-3">
                <div class="col-md-6">
                    <img :src="'../assets/images/'+ product.gambar" class="img-fluid shadow" style="border-radius: 15px" alt="">
                </div>
                <div class="col-md-6">
                    <h2><Strong>{{ product.nama }}</Strong></h2>
                    <hr>
                    <h4>Harga: <strong>Rp. {{ product.harga }}</strong></h4>
                    <form class="mt-4" @submit.prevent>
                        <div class="form-group">
                            <label for="jumlah_pemesanan">Jumlah Pesan</label>
                            <input v-model="pesan.jumlah_pemesanan" type="number" class="form-control">
                        </div>

                        <div class="form-group">
                            <label for="keterangan">Keterangan</label>
                            <textarea v-model="pesan.keterangan" class="form-control" placeholder="Keterangan Contoh: Pedas, Banyak kuah ..."></textarea>
                        </div>

                        <button type="button" class="btn btn-success" @click="pemesanan"> <b-icon-cart></b-icon-cart> Pesan</button>
                    </form>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
import Navbar from "@/components/Navbar"
import axios from 'axios'

export default {
    name: "Food Detail",
    components:{
        Navbar
    },
    data(){
        return{
            product: {},
            pesan: {}
        }
    },

    methods:{
        setProduct(data){
            this.product = data
        },

        pemesanan(){
            if(this.pesan.jumlah_pemesanan){
                this.pesan.products = this.product
                    axios.post('http://localhost:3000/keranjangs', this.pesan)
                    .then(() => {
                        this.$router.push({ path: "/keranjang"})
                        this.$toast.success('Pesanan sudah dimasukkan ke keranjang', {
                            type: 'success',
                            position: 'top-right',
                            duration: 3000,
                            dismissible: true
                        })
                    })
                    .catch((err) =>console.log(err))
                }else{
                    this.$toast.error('Jumlah pesanan harus diisi!', {
                        type: 'error',
                        position: 'top-right',
                        duration: 3000,
                        dismissible: true
                    })
                }
        }
    },

    mounted() {
    axios
        .get("http://localhost:3000/products/"+ this.$route.params.id)
        .then((response) => this.setProduct(response.data))
        .catch((error) => console.log(error));
    },
}
</script>

<style>

</style>