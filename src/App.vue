<template>
  <div>
    <div class="container-fluid p-0">
      <nav class="navbar-light bg-light p-3 d-flex gap-5">
        <h1 class="fs-2">CRUD mahasiswa</h1>

        <div class="btn-group gap-2">
          <button type="button" class="btn btn-link" @click="ChangePage('mahasiswa-page')">Data mahasiswa</button>
          <button type="button" class="btn btn-link" @click="ChangePage('akun-page')">Data akun</button>
        </div>
      </nav>

      <div class="container">
        <component :is="activePage"></component>
      </div>

    </div>
  </div>
</template>
<script>
import MahasiswaPage from '@/pages/Mahasiswa/Index.vue'
import AkunPage from '@/pages/Akun/Index.vue'

export default {
  data() {
    return {
      activePage: 'mahasiswa-page'
    }
  },
  components: {
    MahasiswaPage, AkunPage
  },
  methods: {
    ChangePage: function (page) {
      this.activePage = page
    }
  }
}
</script>

<style>
@import '~bootstrap/dist/css/bootstrap.css';
</style>