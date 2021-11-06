<template>
  <div class="all">
    <div class="judul"><marquee  width="500px"><span><h1>SELAMAT DATANG DI CATATAN PINJAMAN BUKU PERPUSTAKAAN</h1></span></marquee></div><br><br>
    <div>
      <div class="katapenting">
          Untuk Menambahkan/Memperpanjang Pinjaman
        </div>
      <form @submit.prevent="add">
      <div class="untukinput">
        <div style="margin-top: 10px;">
        <input type="hidden" v-model="form.id" required placeholder="Masukan Nomor..."><br>
        </div>
        <div style="margin-top: 10px;" >
          <span>Nama Siswa :</span><br>
        <input type="text" v-model="form.nama" required placeholder="Masukan Nama Siswa..."><br>
        </div>
        <div style="margin-top: 10px;" >
          <span>Judul Buku :</span><br>
        <input type="text" v-model="form.judul" required placeholder="Masukan Judul Buku..."><br>
        </div>
        <div style="margin-top: 10px;" >
          <span>Tanggal Pinjam :</span><br>
        <input type="text" v-model="form.tpinjam" required placeholder="Masukan Tanggal Pinjam..."><br>
        </div>
        <div style="margin-top: 10px;" >
          <span>Tanggal Pengembalian :</span><br>
        <input type="text" v-model="form.tpengembalian" placeholder="Masukan Tanggal Pengembalian...">
        </div><br>
        <div class="untukbutton">
        <button type="submit" v-show="!updateSubmit">Tambah Daftar Pinjaman Buku</button>  
        <button type="button" v-show="updateSubmit" @click="update(form)">Ubah Daftar Pinjaman Buku</button>
        </div>
        </div>
        <div class="untuktable">
          <h3>Table Daftar Buku yang Dipinjam</h3>
        <table border="1px" style="border-radius: 10px; background-color: greenyellow;">
      <tr style="height:1cm;">
        <th style="text-align: center; border-radius: 15px; background-color: yellow;">No</th>
        <th style="text-align: center; border-radius: 15px; background-color: yellow;">Nama Siswa</th>
        <th style="text-align: center; border-radius: 15px; background-color: yellow;">judul Buku</th>
        <th style="text-align: center; border-radius: 15px; background-color: yellow;">Tanggal Pinjam</th>
        <th style="text-align: center; border-radius: 15px; background-color: yellow;">Tanggal Pengembalian</th>
        <th style="text-align: center; border-radius: 15px; background-color: yellow;">Ubah Table</th>
      <tr v-for="user in users" :key="user.id" style="height:1cm;">
        <td style="text-align: center; border-radius: 15px; background-color: yellow;">{{ user.id }}</td>
        <td style="text-align: center; border-radius: 15px; background-color: yellow;">{{ user.nama }}</td>
        <td style="text-align: center; border-radius: 15px; background-color: yellow;">{{ user.judul }}</td>
        <td style="text-align: center; border-radius: 15px; background-color: yellow;">{{ user.tpinjam }}</td>
        <td style="text-align: center; border-radius: 15px; background-color: yellow;">{{ user.tpengembalian }}</td>
        <td style="text-align: center; border-radius: 15px; background-color: yellow;"><a href="#" @click="edit(user)">Perpanjang Pinjaman Buku</a> <br> <a href="#" @click="del(user)">Buku Telah Dikembalikan</a></td>
      </tr> 
      </table>
      </div>
    </form>
    </div>
  </div>
</template>

<script>
/* eslint-disable */ 
import axios from 'axios'
export default {
  data(){
    return{
        form: {
          id: '',
          nama: '',
          judul: '',
          tpinjam: '',
          tpengembalian: ''
        },
        users: [],
        updateSubmit: false
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load(){
        axios.get('http://localhost:3000/users').then(res => {
        this.users = res.data
      }).catch ((err) => {
        console.log(err);
        
      })
    },
      add(){
      axios.post('http://localhost:3000/users/', this.form).then(res => {
          this.load()
          this.form.id = ''
          this.form.nama = ''
          this.form.judul = ''
          this.form.tpinjam = ''
          this.form.tpengembalian = ''
      })
    },
    edit(user){ 
        this.updateSubmit = true
        this.form.id = user.id 
        this.form.nama = user.nama
        this.form.judul = user.judul 
        this.form.tpengembalian = user.tpengembalian 
        this.form.tpinjam = user.tpinjam 
    },
    update(form){
       return axios.put('http://localhost:3000/users/' + form.id , {nama: this.form.nama, 
       tpinjam: this.form.tpinjam, 
       tpengembalian: this.form.tpengembalian, 
       judul: this.form.judul}).then(res => {
        this.load()
        this.form.id = ''
        this.form.nama = ''
        this.form.tpinjam = ''
        this.form.tpengembalian = ''
        this.form.judul = ''
        this.updateSubmit = false
      }).catch((err) => {
        console.log(err);
      })
    },
    del(user){
      axios.delete('http://localhost:3000/users/' + user.id).then(res =>{
          this.load()
          let index = this.users.indexOf(form.name)
          this.users.splice(index,1)
      })
    }
  }
}
</script>

<style scoped>
.judul {
  margin-left: 440px;
  background-color: rgb(255, 217, 0);
  border-radius: 15px;
  margin-right: 410px;
}
.untuktable {
  position: absolute;
  width: 20cm;
  margin-left: 10cm;
}
.katapenting {
  position: absolute;
  background-color: white;
  margin-left: 522px;
  height: 25px;
}
.untukinput {
  border: 1px solid black;
  text-align: center;
  width: 10cm;
  margin-left: 13cm;
  padding-top: 10px;
}
.form {
  width: 6cm;
}
.untukbutton {
  width: 4cm;
}
body:before{
  content:'';
  height:100%;
  display:inline-block;
  vertical-align:middle;
}
button{
  background:green;
  color:#fff;
  border:none;
  position:relative;
  height:auto;
  cursor:pointer;
  transition:800ms ease all;
  border-radius: 10px;
  outline:none;
}
button:hover{
  background:#fff;
  color:green;
}
button:before,button:after{
  content:'';
  position:absolute;
  top:0;
  right:0;
  height:2px;
  width:0;
  background: green;
  transition:400ms ease all;
}
button:after{
  right:inherit;
  top:inherit;
  left:0;
  bottom:0;
}
button:hover:before,button:hover:after{
  width:100%;
  transition:800ms ease all;
}
</style>