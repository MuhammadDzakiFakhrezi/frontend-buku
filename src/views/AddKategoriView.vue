<template>
    <div class="container pt-2">
        <h2>Tambah Kategori</h2>
        <!-- Tambahkan logika atau tampilan lain sesuai kebutuhan -->
        <table class="table table-responsive">
            <tbody>
                <tr>
                    <td>Kode</td>
                    <td><input type="text" v-model="kategori.kode" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Kategori</td>
                    <td><input type="text" v-model="kategori.kategori" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td></td>
                    <td><button class="btn btn-success" @click="tambahKategori">Tambah</button></td>
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
      kategori: {
        kode : '',
        kategori: '',
      }
    };
  },
  created() {
    
  },
  methods: {
    tambahKategori() {
        let formData = new FormData();
        formData.append('kode', this.kategori.kode)
        formData.append('kategori', this.kategori.kategori)
        var bk = {};
        formData.forEach(function(value, key){
            bk[key] = value;
        });
        console.log(this.buku)
        axios({
            method: 'post',
            url: 'https://icemuls.my.id/uaspweb/insertKategori.php',
            data: formData,
            config: { headers: {'Content-Type': 'multipart/form-data' }}
        })
        .then(response => {
        // Berhasil menambahkan buku, lakukan tindakan yang sesuai
            console.log('Kategori berhasil ditambahkan:', response.data);
            this.$router.push('/kategori');
        })
        .catch(error => {
        // Terjadi kesalahan saat menambahkan buku, tangani kesalahan
        console.error('Gagal menambahkan kategori:', error);
        });
    }
  }
}
</script>