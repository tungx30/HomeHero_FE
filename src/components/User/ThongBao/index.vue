<template>
    <div class="container mt-3">
        <div class="container">
            <div v-for="(value, index) in paginatedThongbao" :key="index" class="notification"
                :class="{ 'new-notification': value.is_read === 0 }" >
                <div class="details">
                    <h4>Thông Báo: {{ (currentPage - 1) * rowsPerPage + index + 1 }}</h4>
                    <p>{{ value.loi_nhan }}</p>
                </div>
                <div class="menu">
                    <router-link :to="{
                        path: value.id_dich_vu === 1
                            ? `/nguoi-dung/chitietdontheogio/` + value.id_don_hang
                            : value.id_dich_vu === 2
                                ? `/nguoi-dung/chitietdondinhki/` + value.id_don_hang
                                : `/nguoi-dung/chitiettongvesinh/` + value.id_don_hang
                    }">
                        <button type="button" class="btn btn-danger detail-button" data-bs-dismiss="modal"
                            @click="markAsRead(value.id)">
                            Xem Chi Tiết
                        </button>
                    </router-link>
                </div>
            </div>
            <div class="flex justify-between items-center mt-4 pagination-bar">
                <div class="text-gray-400">
                    Rows per page:
                    <select v-model="rowsPerPage" @change="currentPage = 1" class="pagination-bar-select">
                        <option value="5">5</option>
                        <option value="10">10</option>
                        <option value="50">50</option>
                        <option value="100">100</option>
                    </select>
                </div>
                <div class="text-gray-400">
                    {{ startItem }} - {{ endItem }} of {{ thongbao.length }}
                    <button @click="prevPage" class="ml-2 text-gray-400 hover:text-white" :disabled="currentPage === 1">
                        <i class="fas fa-chevron-left"></i>
                    </button>
                    <button @click="nextPage" class="ml-2 text-gray-400 hover:text-white"
                        :disabled="currentPage === totalPages">
                        <i class="fas fa-chevron-right"></i>
                    </button>
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
            thongbao: [],
            currentPage: 1,
            rowsPerPage: 5,
            currentUserId: null,
        };
    },
    computed: {
        totalPages() {
            return Math.ceil(this.thongbao.length / this.rowsPerPage);
        },
        paginatedThongbao() {
            const start = (this.currentPage - 1) * this.rowsPerPage;
            const end = start + this.rowsPerPage;
            return this.thongbao.slice(start, end);
        },
        startItem() {
            return (this.currentPage - 1) * this.rowsPerPage + 1;
        },
        endItem() {
            return Math.min(this.currentPage * this.rowsPerPage, this.thongbao.length);
        },
    },
    mounted() {
        this.loadtb();
        this.listenToNotifications();
        this.loadCurrentUserId();
    },
    methods: {
        loadCurrentUserId() {
      axios
        .get("http://127.0.0.1:8000/api/nguoi-dung/id", {
          headers: {
            Authorization: 'Bearer ' + localStorage.getItem("token_nguoi_dung"),
          },
        })
        .then((res) => {
          this.currentUserId = res.data.nguoi_dung_id;
          console.log("Current User ID:", this.currentUserId);
          this.listenToNotifications();
        })
        .catch((errors) => {
          console.error("Lỗi khi lấy ID người dùng hiện tại:", errors);
        });
    },
        listenToNotifications() {
            if (!this.currentUserId) {
                console.error("currentUserId chưa được gán, không thể đăng ký channel.");
                return;
            }

            console.log(`Đăng ký channel: notifications.${this.currentUserId}`);
            this.$echo.channel(`notifications.${this.currentUserId}`)
                .listen('.thongbao.created', (data) => {
                    console.log('Thông báo mới nhận:', data);
                    if (data.thongBao.types === 1) {
                        console.log('Thông báo dành cho khách hàng:', data);
                    } else if (data.thongBao.types === 2) {
                        console.log('Thông báo dành cho nhân viên:', data);
                    } else if (data.thongBao.types === 3) {
                        console.log('Thông báo dành cho admin:', data);
                    }
                    this.thongbao.unshift(data.thongBao);
                    this.scrollToTop();
                });
        },
        scrollToTop() {
            const container = document.querySelector('.container');
            if (container) container.scrollTop = 0;
        },
        loadtb() {
            axios
                .get("http://127.0.0.1:8000/api/nguoi-dung/thong-bao/getNhanDonTuNV", {
                    headers: {
                        Authorization: 'Bearer ' + localStorage.getItem("token_nguoi_dung")
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.thongbao = res.data.data;
                        // console.log("Dữ liệu thông báo:", this.thongbao);
                    } else {
                        console.log("Không có thông báo nào:", res.data.message);
                    }
                })
                .catch((error) => {
                    console.error("Lỗi khi tải danh sách thông báo:", error);
                });
        },
        markAsRead(id) {
            axios
                .post(`http://127.0.0.1:8000/api/nguoi-dung/thay-doi-trang-thai-doc/${id}`, {}, {
                    headers: {
                        Authorization: 'Bearer ' + localStorage.getItem("token_nguoi_dung")
                    }
                })
                .then(() => {
                    this.loadtb();
                })
                .catch((error) => {
                    console.error("Lỗi khi cập nhật trạng thái:", error);
                });
        },
        prevPage() {
            if (this.currentPage > 1) this.currentPage--;
        },
        nextPage() {
            if (this.currentPage < this.totalPages) this.currentPage++;
        },
        viewDetails(id_don_hang) {
            console.log("Chi tiết Đơn dịch vụ:", id_don_hang);
        },
    },
};
</script>

<style scoped>
.pagination-bar select {
    padding: 6px 12px;
    font-size: 16px;
    border-radius: 6px;
    border: 1px solid #007bff;
    background-color: #ffffff;
    color: #333;
    cursor: pointer;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    /* Hiệu ứng bóng nhẹ */
    transition: all 0.3s ease;
    /* Hiệu ứng khi hover hoặc focus */
}

.pagination-bar select:hover {
    border-color: #0056b3;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
}

.pagination-bar select:focus {
    outline: none;
    border-color: #007bff;
    box-shadow: 0 0 8px rgba(0, 123, 255, 0.5);
}

/* Thông Báo */
.notification {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px;
    background-color: #ffffff;
    /* Màu trắng đồng nhất */
    border: 1px solid #e0e0e0;
    /* Viền xám nhẹ */
    border-radius: 8px;
    margin-bottom: 12px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: box-shadow 0.2s ease, transform 0.2s ease;
}

.new-notification {
    background-color: #f0f9ff;
    /* Màu nền xanh nhạt */
    font-weight: bold;
    /* Chữ đậm */
    border-left: 4px solid #007bff;
    /* Viền trái màu xanh */
}

.notification:hover {
    transform: translateY(-4px);
    /* Hiệu ứng nhấc khi hover */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
    /* Bóng rõ hơn */
}

.notification .details h4 {
    margin: 0 0 6px;
    font-size: 18px;
    color: #333;
    font-weight: bold;
}

.notification .details p {
    margin: 0;
    font-size: 14px;
    color: #666;
}

.notification .menu {
    margin-left: 20px;
}

/* Nút Chi Tiết */
.detail-button {
    padding: 8px 16px;
    /* Padding đều */
    font-size: 14px;
    /* Giảm kích thước để hài hòa */
    background-color: #46DFB1;
    /* Màu xanh dương */
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.2s ease;
}

.detail-button:hover {
    background-color: #0056b3;
    /* Xanh đậm hơn khi hover */
    transform: scale(1.05);
    /* Phóng to nhẹ */
}

.detail-button:active {
    background-color: #004085;
    /* Màu đậm hơn khi nhấn */
}

.pagination-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    background-color: #f8f9fa;
    border-radius: 8px;
    border: 1px solid #e0e0e0;
    margin-top: 10px;
}

.pagination-bar select {
    padding: 4px 8px;
    font-size: 14px;
    border-radius: 4px;
    border: 1px solid #ddd;
    background-color: #fff;
    color: #333;
}

.pagination-bar button {
    padding: 4px 10px;
    font-size: 14px;
    border: none;
    background: none;
    color: #007bff;
    cursor: pointer;
    transition: color 0.3s ease, transform 0.2s ease;
}

.pagination-bar button:hover {
    color: #0056b3;
    transform: scale(1.2);
    /* Phóng to nhẹ */
}

.pagination-bar button:disabled {
    color: #ccc;
    cursor: not-allowed;
}
</style>