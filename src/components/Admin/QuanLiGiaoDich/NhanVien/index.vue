<template>
    <div class="page-wrapper">
        <div class="page-content mb">
            <div class="page-breadcrumb d-none d-sm-flex align-items-center mb-4">
                <div class="breadcrumb-title pe-3">Lịch Sử   Giao Dịch Nhân Viên</div>
                <div class="ps-3">
                    <nav aria-label="breadcrumb">
                        <ol class="breadcrumb mb-0 p-0">
                            <li class="breadcrumb-item"><a href="javascript:;"><i class="bx bx-home-alt"></i></a></li>
                            <li class="breadcrumb-item active" aria-current="page">Danh Sách Giao Dịch</li>
                        </ol>
                    </nav>
                </div>
            </div>
            <div class="card move-up">
                <div class="card-body">
                    <div class="d-lg-flex align-items-center mb-3 gap-3">
                        <div class="position-relative">
                            <input type="text" class="form-control ps-5 radius-30"
                                placeholder="Tìm kiếm theo nội dung giao dịch hoặc số tiền giao dịch"
                                v-model="searchTerm" @input="searchGiaoDich">
                            <span class="position-absolute top-50 product-show translate-middle-y">
                                <i class="bx bx-search"></i>
                            </span>
                        </div>
                    </div>
                    <div class="table-responsive">
                        <table class="table mb-0">
                            <thead class="table-light">
                                <tr>
                                    <th class="text-center">#</th>
                                    <th class="text-center">Họ Và Tên Nhân Viên</th>
                                    <th class="text-center">CCCD </th>
                                    <th class="text-center">Ngày Giao Dịch</th>
                                    <th class="text-center">Nội Dung Chuyển</th>
                                    <th class="text-center">Phương Thức Thanh Toán</th>
                                    <th class="text-center">Số Tiền Đã Chuyển</th>
                                    <th class="text-center">Số Tiền Đã Nhận </th>
                                    <th class="text-center">Kiểu Giao Dịch</th>
                                    <th class="text-center">Trạng thái</th>
                                </tr>
                            </thead>
                            <tbody>
                                <template v-for="(value, index) in lSGiaoDich" :key="index">
                                    <tr>
                                        <td class="text-center">{{ index + 1 }}</td>
                                        <td class="text-center">{{ value.nhan_vien.ho_va_ten }}</td>
                                        <td class="text-center">{{ value.nhan_vien.can_cuoc_cong_dan }}</td>
                                        <td class="text-center">{{ formattedTime(value.created_at) }}</td>
                                        <td class="text-center">
                                            <div v-if="!isExpanded" class="truncate-multiline">{{ value.description }}
                                            </div>
                                            <div v-else>{{ value.description }}</div>
                                            <button @click="isExpanded = !isExpanded" class="btn-toggle">
                                                {{ isExpanded ? 'Ẩn bớt' : 'Xem thêm' }}
                                            </button>
                                        </td>
                                        <td class="text-center">Chuyển Khoản</td>
                                        <td class="text-center">{{ formatVND(value.creditAmount) }}</td>
                                        <td class="text-center">{{ formatVND(value.debitAmount) }}</td>
                                        <td class="text-center">
                                            <button v-if="value.type == 1" type="button" class="btn text-nowrap mb-2"
                                                style="background-color: #b2ff33; color: white;">
                                                Nạp tiền Vào ví
                                            </button>
                                            <button v-else-if="value.type == 2" type="button"
                                                class="btn text-nowrap mb-2"
                                                style="background-color: #F8312F; color: white;">
                                                Rút tiền Từ ví
                                            </button>
                                        </td>
                                        <td class="text-center">
                                            <button v-if="value.is_done == 1" type="button" class="btn btn-custom btn-success text-nowrap mb-2">
    Đã Hoàn Thành
</button>
<button v-else type="button" class="btn btn-custom btn-warning text-nowrap mb-2">
    Chưa Hoàn Thành
</button>
                                        </td>
                                    </tr>
                                </template>
                            </tbody>
                        </table>
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
            lSGiaoDich: [],
            searchTerm: '',
            isExpanded: false,
        };
    },
    mounted() {
        this.loadlSGiaoDich();
    },
    methods: {
        formattedTime(rawTime) {
            const date = new Date(rawTime);
            const hours = String(date.getHours()).padStart(2, "0");
            const minutes = String(date.getMinutes()).padStart(2, "0");
            const day = String(date.getDate()).padStart(2, "0");
            const month = String(date.getMonth() + 1).padStart(2, "0");
            const year = date.getFullYear();

            return ` ${day}/${month}/${year}`;
        },
        formatVND(number) {
            return new Intl.NumberFormat('vi-VI', { style: 'currency', currency: 'VND' }).format(number);
        },
        searchGiaoDich() {
            if (this.searchTerm.trim() === '') {
                this.loadlSGiaoDich();
                return;
            }
            axios
                .post("http://127.0.0.1:8000/api/admin/search-giao-dich-nhan-vien",
                    {
                        noi_dung_tim: this.searchTerm
                    },
                    {
                        headers: {
                            Authorization: 'Bearer ' + localStorage.getItem("token_admin")
                        }
                    })
                .then((res) => {
                    if (res.data.data) {
                        this.lSGiaoDich = res.data.data;
                    } else {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '</span>';
                        this.$toast.error(thong_bao);
                    }
                })
                .catch((errors) => {
                    const listErrors = errors.response.data.errors;
                    Object.values(listErrors).forEach((value) => {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + value + '</span>';
                        this.$toast.error(thong_bao);
                    });
                });
        },
        loadlSGiaoDich() {
            axios
                .get("http://127.0.0.1:8000/api/admin/giao-dich-nhan-vien", {
                    headers: {
                        Authorization: 'Bearer ' + localStorage.getItem("token_admin")
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.lSGiaoDich = res.data.data;
                        console.log("lịch sử giao dịch nhân viên", JSON.stringify(this.lSGiaoDich));
                    } else {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + res.data.message + '</span>';
                        this.$toast.error(thong_bao);
                    }
                })
                .catch((errors) => {
                    const listErrors = errors.response.data.errors;
                    Object.values(listErrors).forEach((value) => {
                        var thong_bao = '<b>Thông báo</b><span style="margin-top: 5px">' + value + '</span>';
                        this.$toast.error(thong_bao);
                    });
                });
        },
    },
};
</script>

<style scoped>

.btn-toggle {
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 20px;
    padding: 8px 16px;
    font-size: 14px;
    font-weight: bold;
    cursor: pointer;
    transition: all 0.3s ease-in-out;
}

.btn-toggle:hover {
    background-color: #0056b3;
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.btn-toggle:active {
    transform: translateY(0);
    box-shadow: none;
}

.truncate-multiline {
    display: -webkit-box;
    overflow: hidden;
    text-overflow: ellipsis;
    word-wrap: break-word;
    max-width: 300px;
    text-align: left;
}

.page-wrapper {
    font-family: 'Arial', sans-serif;
    background-color: #f8f9fa;
    max-width: 1300px;
    margin: 20px 50px;
    padding: 0px;
    height: 50vh;
}

.breadcrumb-title {
    font-size: 1.75rem;
    color: #495057;
    font-weight: bold;
    margin-bottom: 5px;
}

.breadcrumb {
    background-color: transparent;
    padding: 0;
    margin-bottom: 4px;
    
}

.card {
    margin-top: 0pX;
    border-radius: 15px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
    border: none;
    overflow: hidden;
    /* Để giữ đường viền mềm mại */
}

.card-body {
    padding: 30px;
    /* Giảm padding để tiết kiệm không gian */
}

.form-control {
    border-radius: 30px;
    box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.1);
    padding: 10px 20px;
}

.btn {
    border-radius: 30px;
    transition: all 0.3s ease-in-out;
    padding: 8px 15px;
    font-size: 0.9rem;
    font-weight: bold;
}

.btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 15px rgba(0, 0, 0, 0.2);
}

.table {
    margin-top: 5px;
    border-collapse: separate;
    border-spacing: 0 10px;
}

.table thead th {
    font-weight: bold;
    color: #495057;
    text-align: center;
    background-color: #f1f3f5;
    border: none;
}

.table td {
    vertical-align: middle;
    border: none;
    padding: 12px 15px;
    text-align: center;
    background-color: #ffffff;
    border-radius: 10px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.table tbody tr {
    transition: transform 0.2s;
}

.table tbody tr:hover {
    transform: translateY(-2px);
    background-color: #f8f9fa;
}

.position-relative input[type="text"] {
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
    padding-left: 2.5rem;
}

.position-absolute.top-50.product-show {
    left: 15px;
    color: #495057;
}

.page-content {
    margin-top: 0;
    padding-top: 5px;
}

.card .table-responsive {
    max-height: 300px; 
    overflow-y: auto; 
    padding-right: 12px; 
}

.card .table-responsive::-webkit-scrollbar {
    width: 8px;
}

.card .table-responsive::-webkit-scrollbar-thumb {
    background-color: #adb5bd;
    border-radius: 10px;
}

.card .table-responsive::-webkit-scrollbar-track {
    background-color: #f1f3f5;
}

.table-light {
    background-color: #e9ecef;
}
</style>
