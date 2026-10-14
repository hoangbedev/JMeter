# JMeter Basic Performance Testing

## 1. Giới thiệu dự án

Dự án **JMeter Basic Performance Testing** được thực hiện nhằm tìm hiểu và thực hành kiểm thử hiệu năng cơ bản bằng Apache JMeter.

Dự án sử dụng HTTP Request để gửi yêu cầu đến API JSONPlaceholder, mô phỏng nhiều người dùng truy cập và thu thập các chỉ số về thời gian phản hồi, thông lượng và tỷ lệ lỗi.

### Mục tiêu

- Làm quen với giao diện và cách sử dụng Apache JMeter.
- Xây dựng Test Plan để kiểm thử một API thông qua HTTP GET.
- Mô phỏng nhiều người dùng ảo bằng Thread Group.
- Sử dụng Response Assertion để kiểm tra mã phản hồi HTTP.
- Đọc và phân tích kết quả qua View Results Tree và Summary Report.
- Lưu Test Plan, kết quả và ảnh minh họa để phục vụ báo cáo.

## 2. Công nghệ và công cụ sử dụng

| Công cụ/thành phần | Mục đích |
|---|---|
| Apache JMeter 5.6.3 | Xây dựng và thực thi kịch bản kiểm thử |
| JSONPlaceholder | API công khai phục vụ thực hành |
| HTTP Request | Gửi yêu cầu GET đến API |
| Thread Group | Cấu hình số người dùng ảo và số vòng lặp |
| Response Assertion | Kiểm tra mã phản hồi HTTP 200 |
| View Results Tree | Xem kết quả của từng request |
| Summary Report | Tổng hợp các chỉ số kiểm thử |

## 3. Đối tượng kiểm thử

API được sử dụng:

- **Website:** https://jsonplaceholder.typicode.com/
- **Endpoint:** https://jsonplaceholder.typicode.com/posts/1
- **HTTP Method:** `GET`
- **Response Code mong đợi:** `200 OK`

Endpoint `/posts/1` trả về thông tin của một bài viết ở định dạng JSON.

## 4. Cấu hình Test Plan

Test Plan được lưu tại `test-plans/basic-load-test.jmx`.

### 4.1. Thread Group

| Tham số | Giá trị |
|---|---|
| Name | Website Load Test |
| Number of Threads (users) | 5 |
| Ramp-up Period | 5 giây |
| Loop Count | 3 |
| Tổng số request dự kiến | 15 |

Với một HTTP Request trong mỗi vòng lặp, tổng số request dự kiến được tính như sau:

**Tổng request = Số người dùng × Số vòng lặp**

`5 × 3 = 15 request`

Ramp-up 5 giây quy định khoảng thời gian khởi tạo 5 người dùng ảo.

### 4.2. HTTP Request

| Tham số | Giá trị |
|---|---|
| Name | Test Homepage |
| Protocol | HTTPS |
| Server Name or IP | jsonplaceholder.typicode.com |
| Method | GET |
| Path | /posts/1 |

HTTP Request gửi yêu cầu GET đến API để lấy dữ liệu bài viết.

### 4.3. Response Assertion

Response Assertion được thêm vào HTTP Request với cấu hình:

| Tham số | Giá trị |
|---|---|
| Field to Test | Response Code |
| Pattern Matching Rules | Equals |
| Patterns to Test | 200 |

Assertion giúp đánh dấu request thất bại nếu mã phản hồi không bằng 200. Việc này bổ sung bước kiểm tra kết quả bên cạnh việc đo thời gian phản hồi.

### 4.4. Listener

Test Plan sử dụng hai Listener:

- **View Results Tree:** Hiển thị từng request, trạng thái thực thi và thông tin phản hồi.
- **Summary Report:** Tổng hợp số lượng mẫu, thời gian phản hồi, độ lệch chuẩn, throughput và tỷ lệ lỗi.

## 5. Quy trình thực hiện

### Bước 1. Tạo Test Plan

Tạo Test Plan, thêm Thread Group và cấu hình 5 người dùng ảo, ramp-up 5 giây và loop count bằng 3.

### Bước 2. Cấu hình HTTP Request

Thêm HTTP Request với phương thức GET và endpoint `/posts/1` của JSONPlaceholder.

### Bước 3. Thêm Response Assertion

Cấu hình assertion để kiểm tra mã HTTP trả về bằng 200.

### Bước 4. Thêm Listener

Thêm View Results Tree và Summary Report để theo dõi kết quả chi tiết và tổng hợp.

### Bước 5. Thực thi kiểm thử

Lưu Test Plan dưới dạng file `.jmx`, sau đó chạy bằng nút Start trên giao diện Apache JMeter.

### Bước 6. Thu thập kết quả

Sau khi kiểm thử hoàn tất, xem kết quả trong Summary Report, lưu bảng thống kê CSV và chụp ảnh minh họa.

## 6. Hình ảnh minh họa

### 6.1. Cấu hình HTTP Request

![Cấu hình HTTP Request](screenshots/01-http-request.png)

### 6.2. Kết quả thực thi trong View Results Tree

![View Results Tree](screenshots/02-view-results-tree.png)

Các request được hiển thị trong View Results Tree để kiểm tra trạng thái thực thi.

### 6.3. Summary Report

![Summary Report](screenshots/03-summary-report.png)

Bảng Summary Report được sử dụng để phân tích kết quả kiểm thử.

### 6.4. Cấu hình Response Assertion

![Response Assertion](screenshots/04-response-assertion.png)

Response Assertion kiểm tra mã phản hồi HTTP có bằng 200 hay không.

## 7. Kết quả kiểm thử

Kết quả dưới đây được ghi nhận từ lần chạy kiểm thử có 15 samples.

| Chỉ số | Kết quả |
|---|---:|
| Tổng số samples | 15 |
| Thời gian phản hồi trung bình (Average) | 212 ms |
| Thời gian phản hồi nhỏ nhất (Min) | 56 ms |
| Thời gian phản hồi lớn nhất (Max) | 1.183 ms |
| Độ lệch chuẩn (Std. Dev.) | 280,85 ms |
| Tỷ lệ lỗi (Error %) | 0,00% |
| Throughput | Khoảng 3,49 requests/giây |
| Received KB/sec | 4,51 |
| Sent KB/sec | 0,47 |

**File thống kê:** `results/summary.csv`

### 7.1. Phân tích kết quả

#### Số lượng request

JMeter ghi nhận 15 samples, đúng với số request dự kiến từ cấu hình 5 người dùng ảo và 3 vòng lặp.

#### Thời gian phản hồi

Thời gian phản hồi trung bình là 212 ms. Request nhanh nhất mất 56 ms, trong khi request chậm nhất mất 1.183 ms.

Sự chênh lệch giữa thời gian phản hồi nhỏ nhất và lớn nhất cho thấy thời gian xử lý các request không hoàn toàn đồng đều.

#### Tỷ lệ lỗi

Tỷ lệ lỗi ghi nhận là 0,00%. Trong lần chạy này, không có sample nào được JMeter đánh dấu thất bại. Response Assertion được cấu hình để kiểm tra mã phản hồi HTTP 200.

#### Throughput

Throughput đạt khoảng 3,49 requests/giây trong lần chạy thử nghiệm. Chỉ số này phản ánh thông lượng ghi nhận trong điều kiện kiểm thử hiện tại, không phải giới hạn tối đa của API.

### 7.2. Nhận xét chung

Với cấu hình tải nhỏ, tất cả 15 samples đều được ghi nhận thành công và thời gian phản hồi trung bình là 212 ms.

Tuy nhiên, số lượng request còn ít và chỉ có một cấu hình tải được thử nghiệm. Vì vậy, kết quả chưa đủ để kết luận về khả năng chịu tải tối đa hoặc độ ổn định của hệ thống khi có nhiều người dùng đồng thời.

## 8. Cấu trúc dự án

```text
JMeter-Basic-Performance-Testing/
├── README.md
├── test-plans/
│   └── basic-load-test.jmx
├── results/
│   └── summary.csv
└── screenshots/
    ├── 01-http-request.png
    ├── 02-view-results-tree.png
    ├── 03-summary-report.png
    ├── 04-response-assertion.png
    ├── Screenshot 2026-10-14 104538.png
    └── Screenshot 2026-10-14 104621.png
```

## 9. Hướng dẫn chạy lại dự án

### Yêu cầu

- Máy tính đã cài Java tương thích với phiên bản JMeter sử dụng.
- Apache JMeter 5.6.3 hoặc phiên bản tương thích.
- Kết nối Internet để truy cập JSONPlaceholder.

### Các bước chạy

1. Tải hoặc clone repository về máy.
2. Mở Apache JMeter.
3. Chọn **File → Open**.
4. Mở file `test-plans/basic-load-test.jmx`.
5. Kiểm tra cấu hình Thread Group và HTTP Request.
6. Nhấn nút **Start** để chạy kiểm thử.
7. Xem kết quả trong View Results Tree và Summary Report.

Kết quả có thể thay đổi giữa các lần chạy tùy theo mạng, thời điểm thực hiện và tình trạng dịch vụ API.

## 10. Kết luận

Qua dự án này, người thực hiện đã thực hành quy trình kiểm thử hiệu năng cơ bản bằng Apache JMeter, từ xây dựng Test Plan, cấu hình HTTP Request và Response Assertion đến thu thập, đọc và phân tích kết quả.

Dự án là nền tảng để tiếp tục tìm hiểu các nội dung nâng cao hơn như tăng số lượng người dùng ảo, so sánh các mức tải, kiểm thử nhiều endpoint và tạo báo cáo HTML.

## 11. Tài liệu tham khảo

- Apache JMeter: https://jmeter.apache.org/
- Apache JMeter User Manual: https://jmeter.apache.org/usermanual/
- JSONPlaceholder: https://jsonplaceholder.typicode.com/
