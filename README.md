<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nhà Xe Trung Kén - Hệ Thống Quản Lý</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&libraries=places"></script>
    <style>
        :root { --primary: #e67e22; --dark: #2c3e50; --success: #27ae60; }
        body { background-color: #f8f9fa; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        .section { display: none; padding: 30px 0; animation: fadeIn 0.5s; }
        .active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .card { border: none; border-radius: 15px; box-shadow: 0 5px 15px rgba(0,0,0,0.05); }
        .btn-primary { background-color: var(--primary); border: none; }
        .btn-primary:hover { background-color: #d35400; }
        .admin-stat-card { background: linear-gradient(45deg, #2c3e50, #34495e); color: white; }
    </style>
</head>
<body>

<div class="toast-container position-fixed bottom-0 end-0 p-3">
    <div id="mainToast" class="toast align-items-center text-white border-0" role="alert">
        <div class="d-flex">
            <div id="toastBody" class="toast-body"></div>
            <button type="button" class="btn-close btn-close-white me-2 m-auto" data-bs-dismiss="toast"></button>
        </div>
    </div>
</div>

<nav class="navbar navbar-expand-lg navbar-dark bg-dark shadow-sm">
    <div class="container">
        <a class="navbar-brand fw-bold" href="#" onclick="location.reload()">NHÀ XE TRUNG KÉN</a>
        <div class="dropdown">
            <button class="btn btn-outline-light btn-sm dropdown-toggle" data-bs-toggle="dropdown">Chế độ Xem</button>
            <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="#" onclick="showSection('register-section')">Khách hàng</a></li>
                <li><a class="dropdown-item" href="#" onclick="showSection('admin-section')">Quản trị viên</a></li>
                <li><a class="dropdown-item" href="#" onclick="showSection('driver-section')">Tài xế</a></li>
            </ul>
        </div>
    </div>
</nav>

<div class="container">

    <div id="register-section" class="section active">
        <div class="row justify-content-center">
            <div class="col-md-5">
                <div class="card p-4">
                    <h3 class="text-center mb-4">Đăng Ký Tài Khoản</h3>
                    <div class="mb-3"><label>Tên người dùng</label><input type="text" class="form-control" id="reg_name"></div>
                    <div class="mb-3"><label>Số điện thoại</label><input type="text" class="form-control" id="reg_phone"></div>
                    <div class="mb-3"><label>Mật khẩu</label><input type="password" class="form-control" id="reg_pass"></div>
                    <button class="btn btn-primary w-100 p-2" onclick="processRegister()">Tiếp Tục Đăng Ký</button>
                    <p class="text-center mt-3 small">Đã có tài khoản? <a href="#" onclick="showSection('login-section')">Đăng nhập ngay</a></p>
                </div>
            </div>
        </div>
    </div>

    <div id="login-section" class="section">
        <div class="row justify-content-center">
            <div class="col-md-5">
                <div class="card p-4 border-top border-warning border-4">
                    <h3 class="text-center mb-4 text-warning">Đăng Nhập</h3>
                    <div class="mb-3"><input type="text" class="form-control p-2" placeholder="Số điện thoại"></div>
                    <div class="mb-3"><input type="password" class="form-control p-2" placeholder="Mật khẩu"></div>
                    <button class="btn btn-dark w-100 mb-3" onclick="showSection('user-dashboard')">Vào Hệ Thống</button>
                    <div class="text-center"><a href="#" class="small text-muted">Quên mật khẩu? (Gửi OTP)</a></div>
                </div>
            </div>
        </div>
    </div>

    <div id="user-dashboard" class="section">
        <div class="row g-4">
            <div class="col-md-8">
                <div class="card p-4 h-100">
                    <h4 class="mb-4">Hành Trình Chuyến Đi</h4>
                    <div class="row g-3">
                        <div class="col-md-6">
                            <label class="form-label fw-bold">Điểm đi (Google Maps)</label>
                            <input id="pickup" type="text" class="form-control" placeholder="Tìm địa chỉ đón...">
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-bold">Điểm đến</label>
                            <input id="dropoff" type="text" class="form-control" placeholder="Tìm địa chỉ đến...">
                        </div>
                        <div class="col-md-4">
                            <label class="form-label">Chọn loại xe</label>
                            <select class="form-select"><option>Xe 4 chỗ</option><option>Xe 7 chỗ</option><option>Giường nằm</option></select>
                        </div>
                        <div class="col-md-4">
                            <label class="form-label">Giờ khởi hành</label>
                            <input type="time" class="form-control">
                        </div>
                        <div class="col-md-4">
                            <label class="form-label">Ngày đi</label>
                            <input type="date" class="form-control">
                        </div>
                    </div>
                    
                    <h5 class="mt-4 border-top pt-3">Hình thức thanh toán</h5>
                    <div class="btn-group w-100 mt-2" role="group">
                        <input type="radio" class="btn-check" name="btnpay" id="pay1" checked onclick="toggleQR(false)">
                        <label class="btn btn-outline-secondary" for="pay1">Tiền mặt</label>
                        <input type="radio" class="btn-check" name="btnpay" id="pay2" onclick="toggleQR(true)">
                        <label class="btn btn-outline-primary" for="pay2">Chuyển khoản / Ví điện tử</label>
                    </div>

                    <div id="qrArea" class="text-center d-none mt-3 p-3 bg-light rounded">
                        <p class="mb-2 small fw-bold">QUÉT MÃ ĐỂ THANH TOÁN</p>
                        <img src="https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=TrungKenPay" alt="QR" class="img-fluid">
                    </div>

                    <button class="btn btn-primary w-100 mt-4 py-3 fw-bold" onclick="alert('Đặt xe thành công! Tài xế sẽ gọi cho bạn.')">XÁC NHẬN ĐẶT CHUYẾN</button>
                </div>
            </div>
            <div class="col-md-4">
                <div class="card p-3 mb-3 bg-warning-subtle border-warning">
                    <h6 class="text-warning-emphasis">🔔 Chuyến xe sắp tới</h6>
                    <p class="small mb-0">Hà Nội - Nghệ An (Ngày mai 05:00)</p>
                </div>
                <div class="card p-4 shadow-sm">
                    <h5>Chi tiết tài xế</h5>
                    <p class="mb-1">Tên: <b>Nguyễn Văn Trung</b></p>
                    <p class="mb-1">Mã chuyến: <b>TK-889</b></p>
                    <p class="mb-3 text-danger">Số tiền: <b>350.000đ</b></p>
                    <hr>
                    <h6>Đánh giá</h6>
                    <div class="text-warning mb-2">⭐⭐⭐⭐⭐</div>
                    <textarea class="form-control form-control-sm" placeholder="Nhận xét..."></textarea>
                </div>
            </div>
        </div>
    </div>

    <div id="admin-section" class="section">
        <div class="row g-3 mb-4 text-center">
            <div class="col-md-4"><div class="card p-3 admin-stat-card"><h5>Tổng khách hôm nay</h5><h2>42</h2></div></div>
            <div class="col-md-4"><div class="card p-3 bg-success text-white"><h5>Doanh thu ngày</h5><h2>15.800.000đ</h2></div></div>
            <div class="col-md-4"><div class="card p-3 bg-info text-white"><h5>Xe đang chạy</h5><h2>08</h2></div></div>
        </div>
        
        <div class="card p-4">
            <div class="d-flex justify-content-between mb-3 align-items-center">
                <h4>Quản Lý Lịch Trình</h4>
                <button class="btn btn-sm btn-dark" onclick="sortPassengerByDate()">Sắp xếp theo ngày ↑</button>
            </div>
            <div class="table-responsive">
                <table class="table table-hover align-middle" id="adminTable">
                    <thead class="table-light">
                        <tr>
                            <th>Ngày</th><th>Mã</th><th>Khách hàng</th><th>Lộ trình</th><th>Tài xế</th><th>Trạng thái</th><th>Thao tác</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>2025-12-29</td><td>#TK01</td><td>Anh Tuấn</td><td>Hà Nội - Vinh</td><td>Hùng Dũng</td><td><span class="badge bg-warning">Chờ đi</span></td>
                            <td>
                                <button class="btn btn-sm btn-outline-primary">Sửa</button>
                                <button class="btn btn-sm btn-outline-danger">Hủy</button>
                            </td>
                        </tr>
                        <tr>
                            <td>2025-12-28</td><td>#TK02</td><td>Chị Lan</td><td>Hà Nội - Nam Định</td><td>Minh Hải</td><td><span class="badge bg-secondary">Lưu trữ (Đã đi)</span></td>
                            <td><button class="btn btn-sm btn-light text-danger">Xóa</button></td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <div id="driver-section" class="section">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card p-4">
                    <div class="d-flex justify-content-between border-bottom pb-2 mb-3">
                        <h4>Khu Vực Tài Xế</h4>
                        <span class="badge bg-primary d-flex align-items-center">ID: TX-449</span>
                    </div>
                    <div class="row">
                        <div class="col-md-6">
                            <p><b>Tài xế:</b> Trần Văn Kén</p>
                            <p><b>Số điện thoại:</b> 0912.xxx.xxx</p>
                            <p><b>Mã xe:</b> 29A-999.88</p>
                            <p><b>Giờ đi:</b> 13:45 | <b>Ngày:</b> 28/12</p>
                        </div>
                        <div class="col-md-6 text-end">
                            <div class="bg-light p-3 border rounded mb-2 text-center">
                                <small class="text-muted">Bản đồ điều hướng (Google Maps)</small>
                                <div id="driverMap" style="height: 100px; background: #ddd; border-radius: 5px;"></div>
                            </div>
                            <button class="btn btn-success w-100" onclick="alert('Đã cập nhật trạng thái: Trả khách thành công')">HOÀN THÀNH - TRẢ KHÁCH</button>
                        </div>
                    </div>
                    <div class="mt-4 p-3 bg-info-subtle border border-info rounded">
                        <h6>Trạng thái hiện tại: <span class="text-primary">Đang đón khách tại Điểm A</span></h6>
                    </div>
                </div>
            </div>
        </div>
    </div>

</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
<script>
    // Hàm hiển thị thông báo Toast
    function showToast(msg, type = 'success') {
        const toastEl = document.getElementById('mainToast');
        document.getElementById('toastBody').innerText = msg;
        toastEl.classList.remove('bg-success', 'bg-danger');
        toastEl.classList.add(type === 'success' ? 'bg-success' : 'bg-danger');
        new bootstrap.Toast(toastEl).show();
    }

    // Chuyển đổi giữa các màn hình
    function showSection(id) {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        window.scrollTo(0,0);
    }

    // Xử lý Đăng ký xong tới Đăng nhập
    function processRegister() {
        const name = document.getElementById('reg_name').value;
        if(!name) { showToast("Vui lòng điền đủ thông tin!", "danger"); return; }
        
        showToast("Đăng ký thành công! Đang chuyển sang Đăng nhập...");
        setTimeout(() => showSection('login-section'), 2000);
    }

    // Hiện/Ẩn QR Thanh toán
    function toggleQR(show) {
        document.getElementById('qrArea').classList.toggle('d-none', !show);
    }

    // Google Autocomplete cho địa chỉ
    function initMaps() {
        try {
            new google.maps.places.Autocomplete(document.getElementById('pickup'));
            new google.maps.places.Autocomplete(document.getElementById('dropoff'));
        } catch(e) { console.log("Google Maps Key không hợp lệ."); }
    }
    window.onload = initMaps;

    // Admin sắp xếp
    function sortPassengerByDate() {
        const table = document.getElementById("adminTable");
        const rows = Array.from(table.tBodies[0].rows);
        rows.sort((a, b) => new Date(a.cells[0].innerText) - new Date(b.cells[0].innerText));
        rows.forEach(row => table.tBodies[0].appendChild(row));
        showToast("Đã sắp xếp danh sách theo ngày tăng dần");
    }
</script>
</body>
</html>
