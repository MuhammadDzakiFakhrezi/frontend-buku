<template>
    <div class="container pt-2">
        <h1>Daftar Buku</h1>
        <a href="/addbuku" class="btn btn-primary my-2">Tambah</a>
        <table class="table">
            <thead>
                <tr>
                <th scope="col">ID</th>
                <th scope="col">Judul</th>
                <th scope="col">Penerbit</th>
                <th scope="col">Pengarang</th>
                <th scope="col">Tahun</th>
                <th scope="col">Aksi</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="b in buku" :key="b.id">
                    <td>{{ b.id }}</td>
                    <td>{{ b.judul }}</td>
                    <td>{{ b.penerbit }}</td>
                    <td>{{ b.pengarang }}</td>
                    <td>{{ b.tahun }}</td>
                    <td class="d-flex justify-content-start">
                        <a :href="'/buku/' + b.kode" class="btn btn-primary me-2">View</a>
                        <a class="btn btn-danger" @click="hapusBuku(b.kode)">Delete</a>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
  
<script>
import axios from 'axios';
import api from '../setting/api'

export default {
  data() {
    return {
      buku: [],
      errors: []
    }
  },
  created() {
    this.getData()
  },
  methods : {
    getData(){
      axios.get( 'https://icemuls.my.id/uaspweb/selectBuku.php')
      .then(response => {
        // JSON responses are automatically parsed.
        this.buku = response.data
        console.log(this.buku)
      })
      .catch(e => {
        this.errors.push(e)
      })
    },
    hapusBuku(kode) {
      // Menghapus buku berdasarkan kode
        console.log(kode)
        let formData = new FormData();
        formData.append('kode', kode)
        var bk = {};
        formData.forEach(function(value, key){
            bk[key] = value;
        });
        axios({
                method: 'post',
                url: 'https://icemuls.my.id/uaspweb/deleteBukubyKode.php',
                data: formData,
                config: { headers: {'Content-Type': 'multipart/form-data' }}
            })
        .then(response => {
            console.log('Buku berhasil dihapus:', response.data);
            // Refresh daftar buku setelah penghapusan
            this.getData()
        })
        .catch(error => {
            console.error('Gagal menghapus buku:', error);
        });
    }
  }
}
</script>
  