<template >
    <div class="container py-5 h-100">
         <div class="row d-flex justify-content-center align-items-center h-100">
             <div class="col col-xl-10">
                 <div class="card" style="border-radius: 1rem;">
                     <div class="row g-0">
                         <div class="col-md-6 col-lg-5 d-none d-md-block">
                             <!-- <img src="D:\Capstone\CAPSTONE_FE\src\assets\images\Login.png" alt="login form"
                                 class="img-fluid" style="border-radius: 1rem 0 0 1rem;" /> -->
                         </div>
                         <div class="col-md-6 col-lg-7 d-flex align-items-center">
                             <div class="card-body p-4 p-lg-5 text-black">
 
                                 <form>
 
                                     <div class="d-flex align-items-center mb-3 pb-1">
                                         <i class="fas fa-cubes fa-2x me-3" style="color: #ff6219;"></i>
                                         <span class="h1 fw-bold mb-0">Home Hero</span>
                                     </div>
 
                                     <h5 class="fw-normal mb-3 pb-3" style="letter-spacing: 1px;">Quên Mật Khẩu</h5>
 
                                     <div data-mdb-input-init class="form-outline mb-4">
                                         <label class="form-label" for="form2Example17">Email address</label>
                                         <input v-model="nhan_vien.email" type="email" id="form2Example17"
                                             class="form-control form-control-lg"/>
                                     </div>
                                     <div class="pt-1 mb-4">
                                         <button v-on:click="guiMail()" data-mdb-button-init data-mdb-ripple-init
                                             class="btn btn-dark btn-lg btn-block" type="button">Xác Nhận
                                         </button>
                                         <router-link to="/nhan-vien/dangnhap">
                                         <a class="btn btn-light w-100"><i class="bx bx-arrow-back mr-1"></i>Quay lại trang
                                             đăng nhập</a>
                                     </router-link>
                                     </div>
                                 </form>
                             </div>
                         </div>
                     </div>
                 </div>
             </div>
         </div>
     </div>
 </template>
 <script>
 import axios from 'axios'
 
 export default {
     data() {
         return {
             nhan_vien : {},
         }
     },
     methods: {
         guiMail(){
             axios
                 .post("http://127.0.0.1:8000/api/nhan-vien/quen-mat-khau", this.nhan_vien)
                 .then((res) => {
                     if (res.data.status) {
                         var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                         this.$toast.success(thong_bao);
                         this.$router.push('/nhan-vien/dangnhap')
                     } else {
                         var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                         this.$toast.error(thong_bao);
                         this.$router.push('/nhan-vien/dangky');
 
                     }
                 })
         }
     },
 }
 </script>
 <style >
 </style>