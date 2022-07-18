<template>
 <div class="container-fluid mt-5">
        <div class="row">
            <div class="col-md-4">
                <div class="card">
                    <div class="card-body">
                           MAIN MENU 
                        <hr>
                        <ul class="list-group">
                            <router-link :to="{name: 'dashboard'}"
                            class="list-group-item text-dark text-decoration-none">DASHBOARD</router-link>
                        </ul>
                        <ul class="list-group">
                            <router-link :to="{name: 'dosen.index'}"
                            class="list-group-item text-dark text-decoration-none">DOSEN</router-link>
                        </ul>
                         <ul class="list-group">
                            <router-link :to="{name: 'hari.index'}"
                            class="list-group-item text-dark text-decoration-none">HARI</router-link>
                        </ul>
                          <ul class="list-group">
                            <router-link :to="{name: 'jadwal.index'}"
                            class="list-group-item text-dark text-decoration-none">JADWAL</router-link>
                        </ul>
                         <ul class="list-group">
                            <router-link :to="{name: 'matkul.index'}"
                            class="list-group-item text-dark text-decoration-none">MATA KULIAH</router-link>
                        </ul>
                         <ul class="list-group">
                            <router-link :to="{name: 'jam.index'}"
                            class="list-group-item text-dark text-decoration-none">JAM</router-link>
                        </ul>
                        <ul class="list-group">
                            <router-link :to="{name: 'pengampu.index'}"
                            class="list-group-item text-dark text-decoration-none">PENGAMPU</router-link>
                        </ul>
                        <ul class="list-group">
                            <router-link :to="{name: 'ruang.index'}"
                            class="list-group-item text-dark text-decoration-none">RUANG</router-link>
                        </ul>
                        <ul class="list-group">
                            <router-link :to="{name: 'waktu.index'}"
                            class="list-group-item text-dark text-decoration-none">WAKTU</router-link>
                        </ul>
                         <ul class="list-group">
                            <li @click.prevent="logout" class="list-group-item text-dark text-decoration-none"
                            style="cursor:pointer">LOGOUT</li>
                        </ul>
                    </div>
                </div>
            </div>
            <div class="col-md-8">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>EDIT PENGAMPU</h4>
                        <hr>
 
                        <form @submit.prevent="update">
                            <div class="form-group">
                                <label for="kodemk" class="font-weight-bold">KODE MAHASISWA</label>
                                <input type="text" class="form-control" v-model="pengampu.kodemk" placeholder="Masukkan Kode Mahasiswa">
                                <!-- validation -->
                                <div v-if="validation.kodemk" class="mt-2 alert alert-danger">
                                    {{ validation.kodemk[0] }}
                                </div>
                            </div>
                            
                              <div class="form-group">
                                <label for="kodedosen" class="font-weight-bold">KODE DOSEN</label>
                                <input type="text" class="form-control" v-model="pengampu.kodedosen" placeholder="Masukkan Kode Dosen">
                                <!-- validation -->
                                <div v-if="validation.kodedosen" class="mt-2 alert alert-danger">
                                    {{ validation.kodedosen[0] }}
                                </div>
                            </div>

                              <div class="form-group">
                                <label for="kelas" class="font-weight-bold">KELAS</label>
                                <input type="text" class="form-control" v-model="pengampu.kelas" placeholder="Masukkan Kelas">
                                <!-- validation -->
                                <div v-if="validation.kelas" class="mt-2 alert alert-danger">
                                    {{ validation.kelas[0] }}
                                </div>
                            </div>

                              <div class="form-group">
                                <label for="tahunakademik" class="font-weight-bold">TAHUN AKADEMIK</label>
                                <input type="text" class="form-control" v-model="pengampu.tahunakademik" placeholder="Masukkan Tahun Akademik">
                                <!-- validation -->
                                <div v-if="validation.tahunakademik" class="mt-2 alert alert-danger">
                                    {{ validation.tahunakademik[0] }}
                                </div>
                            </div>

                            <button type="submit" class="btn btn-primary">SIMPAN</button>
                        </form>                        
 
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
 
<script>
import { reactive, ref, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import axios from 'axios'
 
export default {
 
    setup() {
 
        //state posts
        const pengampu = reactive({
            kodemk: '',
            kodedosen: '',
            kelas: '',
            tahunakademik: ''
        })
 
        //state validation
        const validation = ref([])
 
        //vue router
        const router = useRouter()
 
        //vue router
        const route = useRoute()
 
        //mounted
        onMounted(() => {
 
            //get API from Laravel Backend
            axios.get(`http://localhost:8000/api/pengampu/${route.params.id}`)
            .then(response => {
              
              //assign state posts with response data
              pengampu.kodemk    = response.data.data.kodemk
              pengampu.kodedosen = response.data.data.kodedosen
              pengampu.kelas   = response.data.data.kelas
              pengampu.tahunakademik = response.data.data.tahunakademik
 
            }).catch(error => {
                console.log(error.response.data)
            })
 
        })
 
        //method update
        function update() {
 
            let kodemk   = pengampu.kodemk
            let kodedosen = pengampu.kodedosen
            let kelas = pengampu.kelas
            let tahunakademik = pengampu.tahunakademik
 
            axios.put(`http://localhost:8000/api/pengampu/${route.params.id}`, {
                kodemk: kodemk,
                kodedosen: kodedosen,
                kelas: kelas,
                tahunakademik: tahunakademik,
            }).then(() => {
 
                //redirect ke pengampu index
                router.push({
                    name: 'pengampu.index'
                })
 
            }).catch(error => {
                //assign state validation with error 
                validation.value = error.response.data
            })
 
        }
 
        //return
        return {
            pengampu,
            validation,
            router,
            update
        }
 
    }
 
}
</script>
 
<style>
    body{
        background: lightgray;
    }
</style>
