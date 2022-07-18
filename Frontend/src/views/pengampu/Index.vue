
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
                        <h4>DATA PENGAMPU</h4>
                        <hr>
                        <router-link :to="{name: 'pengampu.create'}" class="btn btn-md btn-success">TAMBAH PENGAMPU</router-link>
 
                        <table class="table table-striped table-bordered mt-4">
                            <thead class="thead-dark">
                                <tr>
                                    <th scope="col">KODE MAHASISWA</th>
                                    <th scope="col">KODE DOSEN</th>
                                    <th scope="col">KELAS</th>
                                    <th scope="col">TAHUN AKADEMIK</th>
                                    <th scope="col">OPTIONS</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(pengampu, index) in pengampus" :key="index">
                                    <td>{{ pengampu.kodemk }}</td>
                                    <td>{{ pengampu.kodedosen}}</td>
                                    <td>{{ pengampu.kelas}}</td>
                                    <td>{{ pengampu.tahunakademik }}</td>
                                    <td class="text-center">
                                        <router-link :to="{name: 'pengampu.edit', params:{id: pengampu.id }}" class="btn btn-sm btn-primary mr-1">EDIT</router-link>
                                        <button @click.prevent="pengampuDelete(pengampu.id)" class="btn btn-sm btn-danger ml-1">DELETE</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
 
                    </div>
                </div>
            </div>
        </div>
        </div>
</template>
 
<script>
import axios from 'axios'
import { onMounted, ref } from 'vue'

 
export default {
 
    setup() {
 
        //reactive state
        let pengampus = ref([])
 
        //mounted
        onMounted(() => {

            //get API from Laravel Backend
            axios.get('http://localhost:8000/api/pengampu')
            .then(response => {
              
              //assign state posts with response data
              pengampus.value = response.data.data
 
            }).catch(error => {
                console.log(error.response.data)
            })
 
        })

                 //method delete
        function pengampuDelete(id) {
            
            //delete data post by ID
            let url= `http://localhost:8000/api/pengampu/${id}`
            if (window.confirm("Are you sure you want to delete this data?")) {
            axios.delete(url)
            .then(() => {
              
              //splice posts 
              pengampus.value.splice(pengampus.value.indexOf(id), 1);

            }).catch(error => {
                console.log(error.response.data)
            })

        }
        }
        //return
        return {
            pengampus,
            pengampuDelete
            
        }
 
    }
 
}
</script>
 
<style>
    body{
        background: lightgray;
    }
</style>
