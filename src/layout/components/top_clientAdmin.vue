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
                            <p class="user-name mb-0"> {{ thongtin.ho_va_ten }}</p>
                            <p class="designattion mb-0">Admin</p>
                        </div>
                    </a>
                    <ul class="dropdown-menu dropdown-menu-end">
                        <li><a class="dropdown-item" href="#" @click="dangxuat_admin"><i class="bx bx-user"></i>Đăng Xuất</a></li>
                        <li>
                            <div class="dropdown-divider mb-0"></div>
                        </li>
                        <li><a class="dropdown-item" href="#" @click="dangxuat_admin_all"><i class="bx bx-log-out-circle"></i>Đăng Xuất Tất Cả</a></li>
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
                        <router-link to="/admin/trangchu">
                            <a href="/admin/trangchu" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                                <div class="parent-icon"><i class="bx bx-home-circle"></i></div>
                                <div class="menu-title">Trang Chủ</div>
                            </a>
                        </router-link>
                    </li>
                    <li class="nav-item dropdown">
                        <a href="" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                            <div class="parent-icon"><i class="bx bx-user"></i></div>
                            <div class="menu-title">Thông Tin Cá Nhân</div>
                        </a>
                        <ul class="dropdown-menu">
                            <li> <a class="dropdown-item" href="/admin/thongtin"><i
                                        class="bx bx-right-arrow-alt"></i>Cập Nhật Thông Tin</a>
                            </li>
                            <!-- <li> <a class="dropdown-item" href="/nguoi-dung/diachi"><i
                                        class="bx bx-right-arrow-alt"></i>Thay đổi địa chỉ</a>
                            </li> -->
                            <li> <a class="dropdown-item" href="/admin/doimatkhau"><i
                                        class="bx bx-right-arrow-alt"></i>Thay đổi mật khẩu</a>
                            </li>
                        </ul>
                    </li>
                    <li class="nav-item dropdown">
                        <a href="" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                            <div class="parent-icon"><i class="bx bx-user-check"></i></div>
                            <div class="menu-title">Thêm Mới</div>
                        </a>
                        <ul class="dropdown-menu">
                            <li> <a class="dropdown-item" href="/admin/danhmuc"><i
                                        class="bx bx-right-arrow-alt"></i>Thêm Danh Mục</a>
                            </li>
                            <li> <a class="dropdown-item" href="/admin/dichvu"><i class="bx bx-right-arrow-alt"></i>Thêm
                                    Dịch Vụ</a>
                            </li>
                            <li> <a class="dropdown-item" href="/admin/magiamgia"><i
                                        class="bx bx-right-arrow-alt"></i>Mã Giảm Giá</a>
                            </li>
                        </ul>
                    </li>
                    <li class="nav-item dropdown">
                        <a href="" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                            <div class="parent-icon"><i class="bx bx-user-check"></i></div>
                            <div class="menu-title">Quản Lý Tài Khoản</div>
                        </a>
                        <ul class="dropdown-menu">
                            <li> <a class="dropdown-item" href="/admin/quanlinguoidung"><i
                                        class="bx bx-right-arrow-alt"></i>Quản Lý Người
                                    Dùng</a>
                            </li>
                            <li> <a class="dropdown-item" href="/admin/quanlinhanvien"><i
                                        class="bx bx-right-arrow-alt"></i>Quản Lý Nhân Viên</a>
                            </li>
                        </ul>
                    </li>
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle dropdown-toggle-nocaret" href="javascript:;">
                            <div class="parent-icon"><i class="bx bx-wallet"></i></div>
                            <div class="menu-title">Quản Lý Ví Điện Tử</div>
                        </a>
                        <ul class="dropdown-menu">
                            <li> <a class="dropdown-item" href="/admin/vinhanvien"><i
                                        class="bx bx-right-arrow-alt"></i>Nhân Viên</a>
                            </li>
                            <li> <a class="dropdown-item" href="/admin/vinguoidung"><i
                                        class="bx bx-right-arrow-alt"></i>Người Dùng</a>
                            </li>
                        </ul>
                    </li>
                    <li class="nav-item dropdown">
                            <a href="" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                                <div class="parent-icon"><i class="bx bx-history"></i></div>
                                <div class="menu-title">Lịch Sử Giao Dịch</div>
                            </a>
                            <ul class="dropdown-menu">
                            <li> <a class="dropdown-item" href="/admin/quanligiaodichnhanvien"><i
                                        class="bx bx-right-arrow-alt"></i>Nhân Viên</a>
                            </li>
                            <li> <a class="dropdown-item" href="/admin/quanligiaodichnguoidung"><i
                                        class="bx bx-right-arrow-alt"></i>Người Dùng</a>
                            </li>
                        </ul>
                    </li>
                    <li class="nav-item dropdown">
                        <router-link to="/admin/thongbao">
                            <a href="/admin/thongbao" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                                <div class="parent-icon"><i class="bx bx-bell"></i></div>
                                <div class="menu-title">Thông Báo</div>
                            </a>
                        </router-link>
                    </li>

                    <router-link to="/admin/hotro">
                        <a href="/admin/hotro" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                            <div class="parent-icon"><i class="bx bx-group"></i></div>
                            <div class="menu-title">Hỗ Trợ </div>
                        </a>
                    </router-link>
                    <li class="nav-item dropdown">
                        <router-link to="/admin/quanlidonhang">
                            <a href="/admin/quanlidonhang" class="nav-link dropdown-toggle dropdown-toggle-nocaret">
                                <div class="parent-icon"><i class="bx bx-bell"></i></div>
                                <div class="menu-title">Quản Lí Đơn dịch vụ</div>
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
import router from '../../router';
export default {
    data() {
        return {
            dangxuat: {},
            thongtin: {},
        };
    },
    mounted() {
        this.check_admin();
        this.loadAdmin();
    },
    methods: {
        dangxuat_admin() {
            axios
                .post("http://127.0.0.1:8000/api/admin/dang-xuat", {}, {
                    headers: {
                        Authorization: 'Bearer ' + localStorage.getItem("token_admin")
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                        this.$toast.success(thong_bao);
                        // localStorage.removeItem("token_nhan_vien");
                        window.location.reload();
                        router.push('/admin/dangnhap');
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
        dangxuat_admin_all() {
            axios
                .post("http://127.0.0.1:8000/api/admin/dang-xuat-tat-ca", {}, {
                    headers: {
                        Authorization: 'Bearer ' + localStorage.getItem("token_admin")
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '<span>';
                        // this.$toast.success(thong_bao);
                        window.location.reload();
                        router.push('/admin/dangnhap');
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
        loadAdmin() {
            axios
                .get("http://127.0.0.1:8000/api/admin/getDataProfile", {
                    headers: {
                        Authorization: 'Bearer ' + localStorage.getItem("token_admin")
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
        check_admin() {
            axios
                .get('http://127.0.0.1:8000/api/check-admin', {
                    headers: {
                        Authorization: 'Bearer ' + localStorage.getItem("token_admin")
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        next();
                    } else {
                        next('/admin/dangnhap');
                        toaster.error(res.data.message);
                    }
                });
        }
    },
}
</script>
<style></style>