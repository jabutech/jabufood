<template>
    <div class="keranjang">
        <Navbar :updateKeranjang="keranjangs"/>
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
                        <li class="breadcrumb-item active font-weight-bold text-dark" aria-current="page">Keranjang</li>
                    </ol>
                    </nav>
                </div>
            </div>
            <!-- end breadcrumb -->

            <div class="row">
                <div class="col">
                    <h2>Keranjang <strong>Saya</strong></h2>
                    <div class="table-responsive mt-3">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th scope="col">#</th>
                                    <th scope="col">Foto</th>
                                    <th scope="col">Makanan</th>
                                    <th scope="col">Keterangan</th>
                                    <th scope="col">Jumlah</th>
                                    <th scope="col">Harga</th>
                                    <th scope="col">Total Harga</th>
                                    <th scope="col">Hapus</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(keranjang, index) in keranjangs" :key="keranjang.id">
                                    <th>{{ index+1}}</th>
                                    <td><img :src="'../assets/images/'+ keranjang.products.gambar" width="250px" style="border-radius: 15px;" class="shadow" alt=""></td>
                                    <td><strong>{{ keranjang.products.nama }}</strong></td>
                                    <td>{{ keranjang.keterangan ? keranjang.keterangan : "-" }}</td>
                                    <td>{{ keranjang.jumlah_pemesanan }}</td>
                                    <td align="right">Rp. {{ keranjang.products.harga }}</td>
                                    <td align="right"><strong>Rp. {{ keranjang.products.harga*keranjang.jumlah_pemesanan }}</strong></td>
                                    <td align="center" class="text-danger">
                                        <b-icon-trash @click.prevent="hapusKeranjang(keranjang.id)"></b-icon-trash>
                                    </td>
                                </tr>
                                <!-- total keseluruhan -->
                                <tr>
                                    <td colspan="6" align="right">
                                        <strong>Total Harga : </strong>
                                    </td>
                                    <td align="right"><strong>Rp. {{ totalHarga }} </strong></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
            <!-- end keranjang -->
            <!-- form checkout -->
            <div class="row justify-content-end">
                <div class="col-md-4">
                    <form class="mt-1" @submit.prevent>
                        <div class="form-group">
                            <label for="nama">Nama :</label>
                            <input v-model="pesan.nama" type="text" class="form-control">
                        </div>

                        <div class="form-group">
                            <label for="noMeja">Nomer Meja :</label>
                            <input v-model="pesan.noMeja" type="number" class="form-control">
                        </div>

                        <button type="button" class="btn btn-success float-right" @click="checkout"> <b-icon-cart></b-icon-cart> Pesan</button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Navbar from '@/components/Navbar'
import axios from 'axios'

export default {
    name: "Keranjang",
    components: {
        Navbar,
        
    }, 
    data(){
        return{
            keranjangs: [],
            pesan: {}
        }
    },
    methods:{
        setKeranjang(data){
            this.keranjangs = data
        },
        hapusKeranjang(id){
            axios
            .delete("http://localhost:3000/keranjangs/"+id)
            .then(() => {
                // toast
                this.$toast.error('Pesanan berhasil dihapus!', {
                    type: 'error',
                    position: 'top-right',
                    duration: 3000,
                    dismissible: true
                })
                // end toast
                // panggil datanya lagi
            axios
            .get("http://localhost:3000/keranjangs")
            .then((response) => this.setKeranjang(response.data))
            .catch((error) => console.log(error));
            })
            .catch((error) => console.log(error));
        },
        checkout(){
            if(this.pesan.nama && this.pesan.noMeja){
                this.pesan.keranjangs = this.keranjangs
                axios.post('http://localhost:3000/pesans', this.pesan)
                    .then(() => {
                        // delete pesanan setelah dipesan
                        this.keranjangs.map(function(item){
                            return axios
                            .delete("http://localhost:3000/keranjangs/"+item.id) 
                            .catch((error) => console.log(error));
                        })
                        // input pesan
                        this.$router.push({ path: "/pesanan-sukses"})
                        this.$toast.success('Sukses dipesan', {
                            type: 'success',
                            position: 'top-right',
                            duration: 3000,
                            dismissible: true
                        })
                    })
            }else{
                // toast
                this.$toast.error('Nama dan nomer meja harus diisi!', {
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
            .get("http://localhost:3000/keranjangs")
            .then((response) => this.setKeranjang(response.data))
            .catch((error) => console.log(error));
    },
    computed:{
        totalHarga(){
            return this.keranjangs.reduce(function(items, data){
                return items+(data.jumlah_pemesanan*data.products.harga)
            }, 0)
        }
    }
}
</script>

<style>

</style>