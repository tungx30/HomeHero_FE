<template>
    <div class="container py-2 h-100">
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
                                    <h5 class="fw-normal mb-3 pb-3" style="letter-spacing: 1px;">Đăng Kí Tài Khoản</h5>
                                    <div class="d-flex justify-content-between">
                                        <div data-mdb-input-init class="form-outline mb-4">
                                            <label class="form-label" for="form2Example17">Họ Và Tên</label>
                                            <input v-model="dangKy.ho_va_ten" type="text" id="form2Example17"
                                                class="form-control form-control-lg" />
                                        </div>
                                        
                                        <div data-mdb-input-init class="form-outline mb-4">
                                            <label class="form-label" for="form2Example18">Email</label>
                                            <input v-model="dangKy.email" type="email" id="form2Example18"
                                                class="form-control form-control-lg" />
                                        </div>
                                    </div>
                                    <div data-mdb-input-init class="form-outline mb-4">
                                        <label class="form-label" for="form2Example27">Mật Khẩu</label>

                                        <!-- Wrapper cho input và icon -->
                                        <div style="position: relative;">
                                            <input :type="showPassword ? 'text' : 'password'" v-model="dangKy.password"
                                                id="form2Example27" class="form-control form-control-lg" />
                                            <!-- Icon con mắt để hiện/ẩn mật khẩu -->
                                            <button type="button" @click="showPassword = !showPassword"
                                                style="position: absolute; right: 10px; top: 50%; transform: translateY(-50%); background: none; border: none; cursor: pointer;">
                                                <i :class="showPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
                                            </button>
                                        </div>
                                    </div>

                                    <div data-mdb-input-init class="form-outline mb-4">
                                        <label class="form-label" for="form2Example27_confirm">Xác Nhận Mật Khẩu</label>

                                        <!-- Wrapper cho input và icon -->
                                        <div style="position: relative;">
                                            <input :type="showRePassword ? 'text' : 'password'"
                                                v-model="dangKy.re_password" id="form2Example27_confirm"
                                                class="form-control form-control-lg" />
                                            <!-- Icon con mắt để hiện/ẩn mật khẩu -->
                                            <button type="button" @click="showRePassword = !showRePassword"
                                                style="position: absolute; right: 10px; top: 50%; transform: translateY(-50%); background: none; border: none; cursor: pointer;">
                                                <i :class="showRePassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
                                            </button>
                                        </div>
                                    </div>
                                    <div class="d-flex justify-content-between">
                                        <!-- Trường Số Điện Thoại -->
                                        <div data-mdb-input-init class="form-outline mb-4"
                                            style="flex: 1; margin-right: 10px;">
                                            <label class="form-label" for="form2Example19">Số Điện Thoại</label>
                                            <input v-model="dangKy.so_dien_thoai" type="tel" id="form2Example19"
                                                class="form-control form-control-lg" />
                                        </div>


                                        <div data-mdb-input-init class="form-outline mb-4"
                                            style="flex: 1; margin-left: 10px;">
                                            <label class="form-label" for="form2Example20">Ngày Sinh</label>
                                            <input v-model="dangKy.ngay_sinh" type="date" id="form2Example20"
                                                class="form-control form-control-lg" />
                                        </div>
                                    </div>
                                    <div data-mdb-input-init class="form-outline mb-4">
                                        <label class="form-label" for="form2Example19">Giới Tính</label>
                                        <select v-model="dangKy.gioi_tinh" id="form2Example19"
                                            class="form-control form-control-lg">
                                            <option value="1">Nam</option>
                                            <option value="0">Nữ</option>
                                            <option value="2">Khác</option>
                                        </select>
                                    </div>
                                    <div class="pt-1 mb-4">
                                        <button v-on:click="DangKyUser()" data-mdb-button-init data-mdb-ripple-init
                                            class="btn btn-dark btn-lg btn-block" type="button">Đăng Kí</button>
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
    data() {
        return {
            dangKy: {},
            showPassword: false,
            showRePassword: false
        };
    },
    methods: {
        DangKyUser() {
            axios
                .post("http://127.0.0.1:8000/api/nguoi-dung/dang-ky", this.dangKy)
                .then((res) => {
                    if (res.data.status) {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                        this.$toast.success(thong_bao);
                        localStorage.setItem('token_nguoi_dung', res.data.token);
                        this.dangKy = {};
                        this.$router.push('/nguoi-dung/dangnhap');
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
<style></style>