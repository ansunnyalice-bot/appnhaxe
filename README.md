<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nhà Xe Trung Kén - Hệ Thống Đặt Xe</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        :root { --primary-color: #e67e22; --dark-blue: #2c3e50; }
        body { background-color: #f4f7f6; font-family: 'Segoe UI', sans-serif; }
        .navbar { background-color: var(--dark-blue); }
        .btn-primary { background-color: var(--primary-color); border: none; }
        .card-header { background-color: var(--primary-color); color: white; }
        .section { display: none; padding: 20px; }
        .active { display: block; }
        #map { height: 300px; background: #eee; border-radius: 10px; margin-bottom: 15px; }
    </style>
</head>
<body>

<nav class="navbar navbar-dark mb-4">
    <div class="container">
        <a class="navbar-brand fw-bold" href="#">NHÀ XE TRUNG KÉN</a>
        <div class="dropdown">
            <button class="btn btn-outline-light dropdown-toggle btn-sm" type="button" data-bs-toggle="dropdown">Chuyển Vai Trò</button>
            <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="#" onclick="showSection('login-section')">Khách Hàng</a></li>
                <li><a class="dropdown-item" href="#" onclick="showSection('admin-section')">Quản Trị (Admin)</a></li>
                <li><a class="dropdown-item" href="#" onclick="showSection('driver-section')">Tài Xế</a></li>
            </ul>
        </div>
    </div>
</nav>

<div class="container">
    
    <div id="login-section" class="section active">
        <div class="row justify-content-center">
            <div class="col-md-5 card shadow p-4">
                <ul class="nav nav-pills mb-3 justify-content-center" id="pills-tab">
                    <li class="nav-item"><button class="nav-link active" data-bs-toggle="pill" data-bs-target="#login">Đăng Nhập</button></li>
                    <li class="nav-item"><button class="nav-link" data-bs-toggle="pill" data-bs-target="#register">Đăng Ký</button></li>
                </ul>
                <div class="tab-content">
                    <div class="tab-pane fade show active" id="login">
                        <input type="text" class="form-control mb-3" placeholder="Số điện thoại">
                        <input type="password" class="form-control mb-2" placeholder="Mật khẩu">
                        <a href="#" class="d-block mb-3 small text-end" data-bs-toggle="modal" data-bs-target="#otpModal">Quên mật khẩu?</a>
                        <button class="btn btn-primary w-100" onclick="showSection('user-dashboard')">Đăng Nhập</button>
                    </div>
                    <div class="tab-pane fade" id="register">
                        <input type="text" class="form-control mb-3" placeholder="Họ và Tên">
                        <input type="text" class="form-control mb-3" placeholder="Số điện thoại">
                        <input type="password" class="form-control mb-3" placeholder="Mật khẩu">
                        <button class="btn btn-primary w-100">Đăng Ký Ngay</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div id="user-dashboard" class="section">
        <h4 class="mb-3">Chào mừng bạn, Trung!</h4>
        <div class="alert alert-warning">🔔 Nhắc nhở: Chuyến đi <b>Hà Nội - Vinh</b> sẽ khởi hành vào 08:00 sáng mai!</div>
        
        <div class="row">
            <div class="col-md-7">
                <div class="card p-3 mb-3 shadow-sm">
                    <h5>Lên lịch chuyến đi</h5>
                    <div id="map" class="d-flex align-items-center justify-content-center text-muted border">Google Map API Placeholder</div>
                    <div class="row g-2">
                        <div class="col-6"><input type="text" class="form-control" placeholder="Điểm đi"></div>
                        <div class="col-6"><input type="text" class="form-control" placeholder="Điểm đến"></div>
                        <div class="col-6">
                            <select class="form-select">
                                <option>Xe 4 chỗ</option>
                                <option>Xe 7 chỗ</option>
                                <option>Xe Limousine</option>
                            </select>
                        </div>
                        <div class="col-6"><input type="time" class="form-control"></div>
                    </div>
                    <button class="btn btn-primary mt-3" data-bs-toggle="modal" data-bs-target="#paymentModal">Đặt Xe Ngay</button>
                </div>
            </div>
            <div class="col-md-5">
                <div class="card p-3 shadow-sm">
                    <h5>Chuyến xe hiện tại</h5>
                    <p class="mb-1"><b>Tài xế:</b> Nguyễn Văn An</p>
                    <p class="mb-1"><b>Mã chuyến:</b> TK-9921</p>
                    <p class="mb-1"><b>Số tiền:</b> 250.000đ</p>
                    <span class="badge bg-success">Đang đến điểm đón</span>
                    <hr>
                    <h6>Đánh giá chuyến đi</h6>
                    <textarea class="form-control mb-2" rows="2" placeholder="Cảm nhận của bạn..."></textarea>
                    <button class="btn btn-sm btn-outline-warning">Gửi đánh giá</button>
                </div>
            </div>
        </div>
    </div>

    <div id="admin-section" class="section">
        <div class="d-flex justify-content-between align-items-center mb-3">
            <h3>Quản Trị Hệ Thống</h3>
            <div class="text-end">
                <span class="badge bg-info p-2">Tổng khách: 45</span>
                <span class="badge bg-success p-2">Doanh thu: 12.500.000đ</span>
            </div>
        </div>
        <div class="table-responsive bg-white p-3 rounded shadow-sm">
            <table class="table table-hover">
                <thead class="table-dark">
                    <tr>
                        <th>Mã</th>
                        <th>Khách Hàng</th>
                        <th>Lộ trình (Tăng dần)</th>
                        <th>Tài Xế</th>
                        <th>Trạng thái</th>
                        <th>Thao tác</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>#TK01</td>
                        <td>Lê Văn B</td>
                        <td>Hà Nội -> Hải Phòng (07:00)</td>
                        <td>Tài xế Cường</td>
                        <td><span class="badge bg-warning text-dark">Chờ đi</span></td>
                        <td>
                            <button class="btn btn-sm btn-info">Sửa</button>
                            <button class="btn btn-sm btn-danger">Hủy</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>

    <div id="driver-section" class="section">
        <div class="card shadow-sm p-4">
            <h3 class="text-primary border-bottom pb-2">Khu Vực Tài Xế</h3>
            <div class="row mt-3">
                <div class="col-md-6">
                    <p><b>Tài xế:</b> Trần Văn Hùng | <b>SĐT:</b> 0988.xxx.xxx</p>
                    <p><b>Mã xe:</b> 29A-123.45</p>
                    <p><b>Giờ đi:</b> 14:30 | <b>Lộ trình:</b> Hà Nội -> Nam Định</p>
                    <div class="alert alert-info">Trạng thái: <b>Đang thực hiện</b></div>
                </div>
                <div class="col-md-6 text-center">
                    <div id="map" class="bg-secondary text-white d-flex align-items-center justify-content-center">Bản đồ điều hướng khách</div>
                    <button class="btn btn-success w-100 mt-2">Đã trả khách (Lưu trữ)</button>
                </div>
            </div>
        </div>
    </div>

</div>

<div class="modal fade" id="paymentModal" tabindex="-1">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header"><h5>Chọn phương thức thanh toán</h5></div>
            <div class="modal-body text-center">
                <button class="btn btn-outline-dark w-100 mb-2">Tiền mặt</button>
                <button class="btn btn-outline-primary w-100" type="button" data-bs-toggle="collapse" data-bs-target="#qrArea">Chuyển khoản QR Code</button>
                <div class="collapse mt-3" id="qrArea">
                    <img src="https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=NhaXeTrungKen" alt="QR Payment">
                    <p class="mt-2 small">Quét mã để thanh toán qua Ngân hàng</p>
                </div>
            </div>
        </div>
    </div>
</div>

<div class="modal fade" id="otpModal" tabindex="-1">
    <div class="modal-dialog">
        <div class="modal-content p-4 text-center">
            <h5>Lấy lại mật khẩu</h5>
            <p>Mã OTP sẽ được gửi về SĐT của bạn</p>
            <input type="text" class="form-control mb-3 text-center" placeholder="Nhập mã OTP">
            <button class="btn btn-primary">Xác nhận</button>
        </div>
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
<script>
    function showSection(id) {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
        document.getElementById(id).classList.add('active');
    }
</script>
</body>
</html>
