<template >
   <HeaderComp></HeaderComp>
   <div class="container pt-3">
      <div class="row mb-3">
         <div class="col-12 col-lg-4 mb-3 mb-lg-0 me-auto">
            <div class="card p-3 shadow">
               <router-link
                  class="btn btn-primary w-100 mb-3"
                  v-if="isAdmin == true"
                  v-bind:to="{ name: 'AdminPage' }"
               >kembali ke halaman admin</router-link>

               <h3 class="fw-bold">Preview Ijazah SDIT Harapan Umat Jember.</h3>
               <h4 class>Tahun Ajaran 2021/2022</h4>
               <p class="mt-3">
                  data berikut berdasarkan akte ananda,
                  jika data sudah benar klik upload
                  <b>tanda tangan</b> sebagai bukti persetujuan lalu klik tombol
                  <b>tombol verifikasi</b>
               </p>
               <p>
                  jika terdapat
                  <b>data yang berbeda</b> silahkan hubungi wali kelas terlebih dahulu
               </p>

               <hr />
               <UploadTtd></UploadTtd>
               <hr />
               <div class="btn-group">
                  <!-- <button
                     type="button"
                     class="btn btn-outline-secondary mb-2 mb-sm-0"
                     data-bs-toggle="modal"
                     data-bs-target="#ModalTtd"
                  >upload tanda tangan</button>-->
                  <a
                     class="btn btn-primary"
                     data-bs-toggle="modal"
                     data-bs-target="#ModalIjazah"
                     href="#"
                  >Verifikasi Ijazah</a>

                  <button
                     type="button"
                     class="btn btn-outline-secondary"
                     data-bs-toggle="modal"
                     data-bs-target="#ModalVerval"
                     v-if="isAdmin == true"
                  >Edit Identitas</button>
               </div>

               <hr />
               <div>
                  <p class="fw-bold">Catatan Persetujuan</p>
                  <p>
                     Jika dikemudian hari terjadi
                     <b>kesalahan atau perubahan data</b> setelah verifikasi data, maka resiko
                     <b>ditanggung Orang Tua / Wali</b> siswa-siswi yang bersangkutan. Sekian dari kami, mohon maaf dan terima kasih
                  </p>
               </div>
            </div>
         </div>

         <div class="col-12 col-lg-8">
            <div class="card p-3 mb-3 shadow">
               <h4 class>Nama Siswa : {{ student.name }}</h4>
            </div>

            <div class="card p-3 mb-3 shadow">
               <h4 v-if="student.isVerified == true">Status : Data Sudah Diverifikasi</h4>
               <h4 v-else>Status : Belum Terverifikasi</h4>
            </div>

            <div class="card p-3 cardijazah shadow">
               <img class="imgijazah" src="../assets/img/ijazah.png" alt="ijazah" />
               <p class="textijazah textkepala">SDIT Harapan Umat Jember</p>
               <p class="textijazah textnpsn">20554128</p>
               <p class="textijazah textkab">Jember</p>
               <p class="textijazah textprov">Jawa Timur</p>
               <p class="textijazah textnamasiswa">{{ student.name }}</p>
               <p
                  class="textijazah textttl"
               >{{ student.place_of_birth }}, {{ student.date_of_birth }}</p>
               <p class="textijazah textnamaortu">{{ student.parents_name }}</p>
               <p class="textijazah textnis">{{ student.nis }}</p>
               <p class="textijazah textnisn">{{ student.nisn }}</p>
               <p class="textijazah textdate">Jember, Juni</p>
               <p class="textijazah textkepsek">Elly Nuzulianti, S.S</p>
            </div>
            <!-- <small class="d-block mt-2">*scroll/sentuh gambar untuk menggeser</small> -->
         </div>
      </div>
   </div>
   <!-- <button v-on:click="cek">cekkkk</button> -->
   <FooterComp></FooterComp>
   <ModalEditComp
      v-bind:url="url"
      v-bind:token="token"
      v-bind:nis="nis"
      v-bind:nisn="nisn"
      v-on:clickSave="getDataIjazah"
   ></ModalEditComp>
   <ModalVerIjazah v-bind:url="url" v-bind:token="token" v-on:clickVerified="tombolverified"></ModalVerIjazah>
</template>

<script>
import HeaderComp from '../components/HeaderComp.vue'
import FooterComp from '../components/FooterComp.vue'
import ModalEditComp from '../components/ModalEditComp.vue'
import ModalVerIjazah from '../components/ModalVerIjazah.vue'
import UploadTtd from '../components/UploadTtd.vue'
import axios from 'axios'


export default {
   name: "IjazahPage",
   components: {
      HeaderComp,
      FooterComp,
      ModalEditComp,
      ModalVerIjazah,
      UploadTtd
   },
   props: {
      url: String
   },
   data() {
      return {
         // url: "http://127.0.0.1:8000/api/student",
         student: "",
         userId: this.$route.params.id,
         isAdmin: "",
         token: "",
         nis: "",
         nisn: "",
      }
   },
   methods: {

      getLocal() {
         const saveData = JSON.parse(localStorage.getItem("localData"));
         this.token = saveData.token;
         this.isAdmin = saveData.isAdmin;
      },
      getDataIjazah() {
         axios.get(`${this.url}/student/${this.userId}`, {
            headers: {
               "Authorization": `Bearer ${this.token}`,
            }
         }).then((result) => {
            const results = result.data.data.student;
            this.student = results;
            this.nis = results.nis;
            this.nisn = results.nisn;
            // console.log(results);
         }).catch((err) => {
            console.log(err)
         })
      },
      tombolverified() {
         this.getDataIjazah()
      },

   },
   mounted() {
      this.getLocal();
      this.getDataIjazah();
      // console.log(this.token)
   }
}
</script>