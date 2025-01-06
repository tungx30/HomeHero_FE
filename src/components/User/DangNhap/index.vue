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
                                        <label class="form-label" for="form2Example17">Email</label>
                                        <input v-model="tai_khoan.email" type="email" id="form2Example17"
                                            class="form-control form-control-lg" />
                                    </div>

                                    <div data-mdb-input-init class="form-outline mb-4">
                                        <label class="form-label" for="form2Example27">Mật Khẩu</label>
                                        <div style="position: relative;">
                                            <input :type="showPassword ? 'text' : 'password'"
                                                v-model="tai_khoan.password" id="form2Example27"
                                                class="form-control form-control-lg" />
                                            <button type="button" @click="showPassword = !showPassword"
                                                style="position: absolute; right: 10px; top: 50%; transform: translateY(-50%); background: none; border: none; cursor: pointer;">
                                                <i :class="showPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
                                            </button>
                                        </div>
                                    </div>
                                    <div class="pt-1 mb-4">
                                        <button v-on:click="DangNhap()" data-mdb-button-init data-mdb-ripple-init
                                            class="btn btn-dark btn-lg btn-block" type="button">Đăng Nhập</button>
                                    </div>
                                    <router-link to="/nguoi-dung/quenmatkhau">
                                        <a class="small text-muted" href="/nguoi-dung/quenmatkhau">Quên Mật Khẩu?</a>
                                    </router-link>
                                    <p class="mb-5 pb-lg-2" style="color: #393f81;">
                                        Chưa Có Tài Khoản?
                                        <a href="#!" style="color: #393f81;" @click.prevent="navigateToRegister">Đăng Ký
                                            Tại Đây</a>
                                    </p>
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
            tai_khoan: {},
            showPassword: false // Chứa dữ liệu đăng nhập
        };
    },
    methods: {
        navigateToRegister() {
            // Điều hướng đến trang đăng ký
            this.$router.push('/nguoi-dung/dangky');
        },
        DangNhap() {
            axios
                .post("http://127.0.0.1:8000/api/nguoi-dung/dang-nhap", this.tai_khoan)
                .then((res) => {
                    if (res.data.status) {
                        // Hiển thị thông báo thành công
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                        this.$toast.success(thong_bao);

                        // Lưu token vào localStorage
                        localStorage.setItem('token_nguoi_dung', res.data.token);

                        // Điều hướng người dùng đến trang chủ sau khi đăng nhập thành công
                        this.$router.push('/');
                    } else {
                        // Hiển thị thông báo lỗi từ backend (nếu có)
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                        this.$toast.error(thong_bao);
                    }
                })
                .catch((errors) => {
                    if (errors.response && errors.response.data.errors) {
                        const listErrors = errors.response.data.errors;
                        Object.values(listErrors).forEach((value) => {
                            // Hiển thị thông báo lỗi nếu có lỗi xác thực từ API
                            var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + value + '<span>';
                            this.$toast.error(thong_bao);
                        });
                    } else {
                        // Hiển thị thông báo lỗi chung nếu có lỗi khác
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">Đã có lỗi xảy ra. Vui lòng thử lại.</span>';
                        this.$toast.error(thong_bao);
                    }
                });
        }
    }
};
</script>
<style scoped>

</style>