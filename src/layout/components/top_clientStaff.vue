<template>
    <header>
        <div class="topbar d-flex align-items-center">
            <nav class="navbar navbar-expand">
                <div class="topbar-logo-header">
                    <div class="">
                        <img src="/src//assets//images/logo.png" class="logo-icon" alt="logo icon">
                    </div>
                    <div class="">
                        <h4 class="logo-text">Home Hero</h4>
                    </div>
                </div>
                <div class="mobile-toggle-menu"><i class="bx bx-menu"></i></div>
                <div class="search-bar flex-grow-1">
                    <div class="position-relative search-bar-box">
                        <!-- <input type="text" class="form-control search-control" placeholder="Type to search..."> <span
                            class="position-absolute top-50 search-show translate-middle-y"><i
                                class="bx bx-search"></i></span>
                        <span class="position-absolute top-50 search-close translate-middle-y"><i
                                class="bx bx-x"></i></span> -->
                    </div>
                </div>
                <div class="user-box dropdown">
                    <a class="d-flex align-items-center nav-link dropdown-toggle" href="#" role="button"
                        data-bs-toggle="dropdown" aria-expanded="false">
                        <div class="user-avatar">
                            <img v-if="thongtin.hinh_anh" :src="thongtin.hinh_anh" class="user-img" alt="user avatar">
                            <i v-else class="fi fi-br-circle-user user-icon"></i>
                        </div>

                        <div class="user-info ps-3">
                            <p class="user-name mb-0">{{ thongtin.ho_va_ten }}</p>
                            <p class="designattion mb-0">Nhân Viên</p>
                        </div>
                    </a>
                    <ul class="dropdown-menu dropdown-menu-end">
                        <li><a class="dropdown-item" href="#" @click="dangxuat_nv"><i class="bx bx-user"></i>Đăng
                                Xuất</a></li>
                        <li>
                            <div class="dropdown-divider mb-0"></div>
                        </li>
                        <li><a class="dropdown-item" href="#" @click="dangxuat_nv_all"><i
                                    class="bx bx-log-out-circle"></i>Đăng Xuất Tất Cả</a></li>
                    </ul>
                </div>

            </nav>
        </div>
        <div class="nav-container primary-menu">
            <div class="mobile-topbar-header">
                <div>
                    <img src="" class="logo-icon" alt="logo icon">
                </div>
                <div>
                    <h4 class="logo-text"></h4>
                </div>
                <div class="toggle-icon ms-auto"><i class="bx bx-arrow-to-left"></i>
                </div>
            </div>
            <nav class="navbar navbar-expand-xl w-100">
                <ul class="navbar-nav justify-content-start flex-grow-1 gap-1">
                    <li class="nav-item dropdown">
                        <router-link to="/nhan-vien/trangchu">
                            <a href="/nhan-vien/trangchu" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                                <div class="parent-icon"><i class="bx bx-home-circle"></i></div>
                                <div class="menu-title">Trang Chủ</div>
                            </a>
                        </router-link>
                    </li>
                    <li class="nav-item dropdown">
                        <a href="/nhan-vien/thongtin" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                            <div class="parent-icon"><i class="bx bx-user"></i></div>
                            <div class="menu-title">Thông Tin Cá Nhân</div>
                        </a>
                        <ul class="dropdown-menu">
                            <li> <a class="dropdown-item" href="/nhan-vien/thongtin"><i
                                        class="bx bx-right-arrow-alt"></i>Cập Nhật Thông Tin</a>
                            </li>
                            <!-- <li> <a class="dropdown-item" href="/nguoi-dung/diachi"><i
                                        class="bx bx-right-arrow-alt"></i>Thay đổi địa chỉ</a>
                            </li> -->
                            <li> <a class="dropdown-item" href="/nhan-vien/doimatkhau"><i
                                        class="bx bx-right-arrow-alt"></i>Thay đổi mật khẩu</a>
                            </li>
                        </ul>
                    </li>
                    <li class="nav-item dropdown">
                        <router-link to="/nhan-vien/lich">
                            <a href="/nhan-vien/lich" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                                <div class="parent-icon"><i class="bx bx-calendar"></i></div>
                                <div class="menu-title">Lịch Làm Việc</div>
                            </a>
                        </router-link>
                    </li>
                    <li class="nav-item dropdown">
                        <router-link to="/nhan-vien/thongbao">
                            <a href="/nhan-vien/thongbao" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                                <div class="parent-icon"><i class="bx bx-bell"></i></div>
                                <div class="menu-title">Thông Báo</div>
                            </a>
                        </router-link>
                    </li>
                    <li class="nav-item dropdown">
                        <router-link to="/nhan-vien/lichsudonhang">
                            <a href="/nhan-vien/lichsudonhang" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                                <div class="parent-icon"><i class="bx bx-list-ul"></i></div>
                                <div class="menu-title">Lịch Sử Đơn dịch vụ</div>
                            </a>
                        </router-link>
                    </li>
                    <li class="nav-item dropdown">
                        <router-link to="/nhan-vien/quanligiaodich">
                            <a href="/nhan-vien/quanligiaodich"
                                class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                                <div class="parent-icon"><i class="bx bx-history"></i></div>
                                <div class="menu-title">Lịch Sử Giao Dịch</div>
                            </a>

                        </router-link>
                    </li>
                    <li class="nav-item dropdown">
                        <router-link to="/nhan-vien/vi">
                            <a class="nav-link dropdown-toggle dropdown-toggle-nocaret" href="javascript:;">
                                <div class="parent-icon"><i class="bx bx-wallet"></i></div>
                                <div class="menu-title">Ví Điện Tử</div>
                            </a>
                        </router-link>
                    </li>
                </ul>
            </nav>
        </div>
    </header>
</template>
<script>
import axios from 'axios';
import check_User from '../../router/check_User';
export default {
    data() {
        return {
            dangxuat: {},
            thongtin: {},
        };
    },
    mounted() {
        this.check_nv();
        this.loadNhanvien();

    },
    methods: {
        dangxuat_nv() {
            axios
                .post("http://127.0.0.1:8000/api/nhan-vien/dang-xuat", {}, {
                    headers: {
                        Authorization: 'Bearer ' + localStorage.getItem("token_nhan_vien")
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                        this.$toast.success(thong_bao);
                        // localStorage.removeItem("token_nhan_vien");
                        window.location.reload();
                        router.push('/nhan-vien/dangnhap');
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

        },
        dangxuat_nv_all() {
            axios
                .post("http://127.0.0.1:8000/api/nhan-vien/dang-xuat-tat-ca", {}, {
                    headers: {
                        Authorization: 'Bearer ' + localStorage.getItem("token_nhan_vien")
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                        this.$toast.success(thong_bao);
                        window.location.reload();
                        router.push('/nhan-vien/dangnhap');
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
        },
        loadNhanvien() {
            axios
                .get("http://127.0.0.1:8000/api/nhan-vien/getDataProfile", {
                    headers: {
                        Authorization: 'Bearer ' + localStorage.getItem("token_nhan_vien")
                    }
                })
                .then((res) => {
                    this.thongtin = res.data.data;
                    console.log('123', JSON.stringify(this.users));

                })
                .catch((error) => {
                    console.error("Lỗi khi tải danh sách nhân viên:", error);
                });
        },
        check_nv() {
            axios
                .get('http://127.0.0.1:8000/api/check-nhanvien', {
                    headers: {
                        Authorization: 'Bearer ' + localStorage.getItem("token_nhan_vien")
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        next();
                    } else {
                        next('/nhan-vien/dangnhap');
                        toaster.error(res.data.message);
                    }
                });
        }
    },
}
</script>
<style></style>