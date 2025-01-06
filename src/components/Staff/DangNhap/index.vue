<template>
    <div class="container py-5 h-100">
        <div class="row d-flex justify-content-center align-items-center h-100">
            <div class="col col-xl-10">
                <div class="card" style="border-radius: 1rem;">
                    <div class="row g-0">
                        <div class="col-md-6 col-lg-5 d-none d-md-block">
                            <img src="/src/assets/images/Login.png" alt="login form"
                                class="img-fluid" style="border-radius: 1rem 0 0 1rem;" />
                        </div>
                        <div class="col-md-6 col-lg-7 d-flex align-items-center">
                            <div class="card-body p-4 p-lg-5 text-black">

                                <form>

                                    <div class="d-flex align-items-center mb-3 pb-1">
                                        <i class="fas fa-cubes fa-2x me-3" style="color: #ff6219;"></i>
                                        <span class="h1 fw-bold mb-0">Home Hero</span>
                                    </div>

                                    <h5 class="fw-normal mb-3 pb-3" style="letter-spacing: 1px;">Đăng Nhập</h5>

                                    <div data-mdb-input-init class="form-outline mb-4">
                                        <label class="form-label" for="form2Example17">Email address</label>
                                        <input type="email" v-model="tai_khoan.email" id="form2Example17"
                                            class="form-control form-control-lg" />

                                    </div>
                                    <div data-mdb-input-init class="form-outline mb-4">
                                        <label class="form-label" for="form2Example27">Password</label>
                                        <input type="password" v-model="tai_khoan.password" id="form2Example27"
                                            class="form-control form-control-lg" />
                                    </div>
                                    <div class="pt-1 mb-4">
                                        <button v-on:click="dangNhap()" data-mdb-button-init data-mdb-ripple-init
                                            class="btn btn-dark btn-lg btn-block" type="button">Đăng Nhập</button>
                                    </div>
                                    <router-link to="/nhan-vien/quenmatkhau">
                                        <a class="small text-muted" href="/nhan-vien/quenmatkhau">Quên Mật Khẩu?</a>
                                    </router-link>
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
import axios from 'axios';
export default {
    data() {
        return {
            tai_khoan: {}
        }
    },
    methods: {
        dangNhap() {
            axios
                .post("http://127.0.0.1:8000/api/nhan-vien/dang-nhap", this.tai_khoan)
                .then((res) => {
                    if (res.data.status) {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                        this.$toast.success(thong_bao);
                        localStorage.setItem('token_nhan_vien', res.data.token);
                        this.$router.push('/nhan-vien/trangchu');
                    }
                    else {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                        this.$toast.error(thong_bao);
                    }
                })
                .catch((errors) => {
                    const listErrors = errors.response.data.errors;
                    Object.values(listErrors).forEach((value) => {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + value + '<span>';
                        this.$toast.error(thong_bao);
                    })
                });
        }
    },
}
</script>
<style scoped></style>