<template>
    <div class="pesanan-sukses">
        <Navbar/>
        <div class="container">
            <div class="row justify-content-center mt-5">
                <div class="col text-center">
                    <img src="@/assets/images/wait.png" width="250px" alt="">
                    <h2><strong>Pesanan Sukses!!</strong></h2>
                    <h4>Pesanan anda segera diproses <br>Selamat Menunggu.</h4>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Navbar from "@/components/Navbar"
export default {
    name: "PesananSukses",
    components:{
        Navbar
    },
}
</script>

<style>

</style>