<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nhà Xe Trung Kén - Hệ Thống Đặt Xe</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&libraries=places"></script>
    <style>
        :root { --primary-color: #e67e22; --dark-blue: #2c3e50; }
        body { background-color: #f4f7f6; font-family: 'Segoe UI', sans-serif; }
        .section { display: none; padding: 20px; }
        .active { display: block; }
        #qrCodeContainer img { border: 5px solid #fff; border-radius: 10px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
        .toast-container { z-index: 1060; }
    </style>
</head>
<body>

<div class="toast-container position-fixed bottom-0 end-0 p-3">
    <div id="regSuccessToast" class="toast align-items-center text-white bg-success border-0" role="alert" aria-live="assertive" aria-atomic="true">
        <div class="d-flex">
            <div class="toast-body">✅ Đăng ký thành công! Chào mừng bạn đến với Trung Kén.</div>
            <button type="button" class="btn-close btn-close-white me-2 m-auto" data-bs-dismiss="toast"></button>
        </div>
    </div>
</div>

<nav class="navbar navbar-dark bg-dark mb-4">
    <div class="container">
        <a class="navbar-brand fw-bold" href="#">NHÀ XE TRUNG KÉN</a>
        <div class="dropdown">
            <button class="btn btn-outline-light dropdown-toggle btn-sm" type="button" data-bs-toggle="dropdown">Vai trò</button>
            <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="#" onclick="showSection('login-section')">Người dùng</a></li>
                <li><a class="dropdown-item" href="#" onclick="showSection('admin-section')">Quản trị viên</a></li>
            </ul>
        </div>
    </div>
</nav>

<div class="container">
    
    <div id="login-section" class="section active">
        <div class="row justify-content-center">
            <div class="col-md-5 card shadow p-4">
                <h4 class="text-center mb-4">Đăng Ký Thành Viên</h4>
                <input type="text" class="form-control mb-3" placeholder="Tên người dùng">
                <input type="text" class="form-control mb-3" placeholder="Số điện thoại">
                <input type="password" class="form-control mb-3" placeholder="Mật khẩu">
                <button class="btn btn-warning w-100 mb-3" onclick="handleRegister()">Đăng Ký Ngay</button>
                <p class="text-center small">Đã có tài khoản? <a href="#" onclick="showSection('user-dashboard')">Đăng nhập tại đây</a></p>
            </div>
        </div>
    </div>

    <div id="user-dashboard" class="section">
        <div class="row">
            <div class="col-md-8">
                <div class="card p-4 shadow-sm">
                    <h5 class="mb-3">Đặt Hành Trình Mới</h5>
                    <div class="row g-3">
                        <div class="col-md-6">
                            <label class="form-label">Điểm đi (Chọn hoặc Nhập)</label>
                            <input id="pickup_location" type="text" class="form-control" placeholder="Nhập địa chỉ đón...">
                        </div>
                        <div class="col-md-6">
                            <label class="form-label">Điểm đến</label>
                            <input id="dropoff_location" type="text" class="form-control" placeholder="Nhập điểm đến...">
                        </div>
                        <div class="col-md-4">
                            <label class="form-label">Số chỗ</label>
                            <select class="form-select"><option>4 chỗ</option><option>7 chỗ</option></select>
                        </div>
                        <div class="col-md-4">
                            <label class="form-label">Giờ đi</label>
                            <input type="time" class="form-control">
                        </div>
                        <div class="col-md-4">
                            <label class="form-label">Ngày đi</label>
                            <input type="date" class="form-control">
                        </div>
                    </div>

                    <h5 class="mt-4">Hình thức thanh toán</h5>
                    <div class="d-flex gap-3 mb-3">
                        <div class="form-check">
                            <input class="form-check-input" type="radio" name="payMethod" id="payCash" checked onclick="toggleQR(false)">
                            <label class="form-check-label" for="payCash">Tiền mặt</label>
                        </div>
                        <div class="form-check">
                            <input class="form-check-input" type="radio" name="payMethod" id="payQR" onclick="toggleQR(true)">
                            <label class="form-check-label" for="payQR">Ví điện tử / QR Bank</label>
                        </div>
                    </div>

                    <div id="qrCodeContainer" class="text-center d-none mb-3 p-3 border rounded">
                        <p class="small text-muted">Quét mã VietQR để thanh toán cho tài xế</p>
                        <img src="https://img.vietqr.io/image/970422-123456789-compact.jpg?amount=500000&addInfo=ThanhToanTrungKen" width="180" alt="QR Code">
                    </div>

                    <button class="btn btn-primary w-100 py-2">Xác Nhận Đặt Xe</button>
                </div>
            </div>
            <div class="col-md-4">
                <div class="card p-3 shadow-sm bg-light">
                    <h6>Thông tin chuyến xe</h6>
                    <p class="mb-1">Tài xế: <b>Lê Hoàng Long</b></p>
                    <p class="mb-1">Mã xe: <b>TK-5562</b></p>
                    <p class="mb-0">Số tiền: <b class="text-danger">250.000đ</b></p>
                </div>
            </div>
        </div>
    </div>

    <div id="admin-section" class="section">
        <div class="d-flex justify-content-between align-items-center mb-3">
            <h3>Danh Sách Khách Hàng</h3>
            <button class="btn btn-sm btn-outline-primary" onclick="sortPassengerByDate()">Sắp xếp theo ngày (Tăng dần) ↑</button>
        </div>
        <div class="table-responsive shadow-sm rounded">
            <table class="table table-hover bg-white" id="passengerTable">
                <thead class="table-dark">
                    <tr>
                        <th>Ngày</th>
                        <th>Khách Hàng</th>
                        <th>Lộ trình</th>
                        <th>Tài xế</th>
                        <th>Trạng thái</th>
                        <th>Hành động</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>2025-12-30</td>
                        <td>Nguyễn Văn A</td>
                        <td>Hà Nội - Nam Định</td>
                        <td>Tài xế Hùng</td>
                        <td><span class="badge bg-warning">Đang chờ</span></td>
                        <td><button class="btn btn-sm btn-danger">Hủy</button></td>
                    </tr>
                    <tr>
                        <td>2025-12-28</td>
                        <td>Trần Thị B</td>
                        <td>Hải Phòng - Hà Nội</td>
                        <td>Tài xế Nam</td>
                        <td><span class="badge bg-success">Đã đi</span></td>
                        <td><button class="btn btn-sm btn-secondary">Lưu trữ</button></td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>

</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
<script>
    // 1. Chuyển đổi giao diện
    function showSection(id) {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
        document.getElementById(id).classList.add('active');
    }

    // 2. Xử lý Đăng ký và hiện Thông báo (Toast)
    function handleRegister() {
        const toastEl = document.getElementById('regSuccessToast');
        const toast = new bootstrap.Toast(toastEl);
        toast.show();
        setTimeout(() => showSection('user-dashboard'), 2000);
    }

    // 3. Google Places Autocomplete (Tự động gợi ý địa chỉ)
    function initAutocomplete() {
        new google.maps.places.Autocomplete(document.getElementById('pickup_location'));
        new google.maps.places.Autocomplete(document.getElementById('dropoff_location'));
    }
    window.onload = initAutocomplete;

    // 4. Logic Thanh toán QR
    function toggleQR(show) {
        const qrBox = document.getElementById('qrCodeContainer');
        if (show) qrBox.classList.remove('d-none');
        else qrBox.classList.add('d-none');
    }

    // 5. Admin: Sắp xếp khách theo ngày tăng dần
    function sortPassengerByDate() {
        const table = document.getElementById("passengerTable");
        const tbody = table.tBodies[0];
        const rows = Array.from(tbody.rows);

        rows.sort((a, b) => {
            const dateA = new Date(a.cells[0].innerText);
            const dateB = new Date(b.cells[0].innerText);
            return dateA - dateB;
        });

        rows.forEach(row => tbody.appendChild(row));
    }
</script>
</body>
</html>
