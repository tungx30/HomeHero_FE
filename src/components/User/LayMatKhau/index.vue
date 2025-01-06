<template>
    <div class="container py-5 h-100">
        <div class="row d-flex justify-content-center align-items-center h-100">
            <div class="col col-xl-10">
                <div class="card" style="border-radius: 1rem;">
                    <div class="row g-0">
                        <div class="col-md-6 col-lg-5 d-none d-md-block">
                            <img src="/src/assets/images/2.png" alt="login form"
                                class="img-fluid" style="border-radius: 1rem 0 0 1rem;" />
                        </div>
                        <div class="col-md-6 col-lg-7 d-flex align-items-center">
                            <div class="card-body p-4 p-lg-5 text-black">

                                <form>

                                    <div class="d-flex align-items-center mb-3 pb-1">
                                        <i class="fas fa-cubes fa-2x me-3" style="color: #ff6219;"></i>
                                        <span class="h1 fw-bold mb-0">Home Hero</span>
                                    </div>

                                    <h5 class="fw-normal mb-3 pb-3" style="letter-spacing: 1px;">Lấy Lại Mật Khẩu</h5>

                                    <div data-mdb-input-init class="form-outline mb-4">
                                        <label class="form-label" for="form2Example17">Mật Khẩu Mới</label>
                                        <div style="position: relative;">
                                            <input :type="showNewPassword ? 'text' : 'password'"
                                                v-model="nguoi_dung.password" id="form2Example17"
                                                class="form-control form-control-lg" />
                                            <button type="button" @click="showNewPassword = !showNewPassword"
                                                style="position: absolute; right: 10px; top: 50%; transform: translateY(-50%); background: none; border: none; cursor: pointer;">
                                                <i :class="showNewPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
                                            </button>
                                        </div>
                                    </div>

                                    <div data-mdb-input-init class="form-outline mb-4">
                                        <label class="form-label" for="form2Example17_confirm">Xác Nhận Mật Khẩu</label>
                                        <div style="position: relative;">
                                            <input :type="showConfirmPassword ? 'text' : 'password'"
                                                v-model="nguoi_dung.re_password" id="form2Example17_confirm"
                                                class="form-control form-control-lg" />
                                            <button type="button" @click="showConfirmPassword = !showConfirmPassword"
                                                style="position: absolute; right: 10px; top: 50%; transform: translateY(-50%); background: none; border: none; cursor: pointer;">
                                                <i :class="showConfirmPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
                                            </button>
                                        </div>
                                    </div>
                                    <div class="pt-1 mb-4">
                                        <button v-on:click="doiMatKhau()" data-mdb-button-init data-mdb-ripple-init
                                            class="btn btn-dark btn-lg btn-block" type="button">Thay Đổi Mật
                                            Khẩu</button>
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
import axios from 'axios';

export default {
    props: ['id_nguoi_dung'],
    data() {
        return {
            nguoi_dung: {
                'id': this.$route.params.id_nguoi_dung
            },
            showNewPassword: false,
            showConfirmPassword: false
        }
    },
    methods: {
        doiMatKhau() {
            axios
                .post("http://127.0.0.1:8000/api/nguoi-dung/doi-mat-khau", this.nguoi_dung)
                .then((res) => {
                    if (res.data.status) {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                        this.$toast.success(thong_bao);
                        this.$router.push('/nguoi-dung/dangnhap')
                    } else {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                        this.$toast.error(thong_bao);
                        this.$router.push('/nguoi-dung/dangnhap');
                    }
                })
                .catch((res) => {
                    var list = Object.values(res.response.data.errors);
                    list.forEach((v, k) => {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + v[0] + '<span>';
                        this.$toast.error(thong_bao);
                    });
                });
        },
    },
}
</script>
<style></style>