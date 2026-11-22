## Tổng quan bước 2.2 — Phân tích khám phá dữ liệu (EDA)

- Khảo sát nhu cầu tuyển dụng IT thông qua kỹ năng, mức lương, cấp bậc và địa điểm.

- Phần mở rộng bổ sung góc nhìn về ngoại ngữ, quyền lợi và mức độ phụ thuộc của kết quả vào nguồn dữ liệu.

**Đầu vào:** `clean_data_processed.csv`  
**Đầu ra:** Các bảng thống kê, biểu đồ và nhận xét làm cơ sở cho gom cụm, luật kết hợp và phân lớp.

### 2.2.1 — Đọc dữ liệu, xem cơ cấu và độ bao phủ

Chọn tập IT dùng sơ bộ và thống kê số tin theo nguồn. Kiểm tra tỷ lệ có JD, đủ thông tin lương và xác định được hình thức làm việc để hiểu giới hạn của mẫu trước khi phân tích.

### 2.2.2 — Top 15 kỹ năng được ghi nhận nhiều nhất

Đếm số tin đề cập từng kỹ năng và trực quan hóa Top 15. Mỗi tin chỉ đóng góp một lần cho mỗi kỹ năng; tỷ lệ được tính trên toàn bộ tin có JD, kể cả giỏ kỹ năng rỗng.

### 2.2.3 — Phân bố lương theo cấp bậc

So sánh trung điểm khoảng lương chào tuyển giữa các cấp bậc bằng median, tứ phân vị và boxplot. Hiển thị số mẫu và đối chiếu thêm tập không giả định chu kỳ lương để kiểm tra ảnh hưởng của lựa chọn dữ liệu.

### 2.2.4 — So sánh Hà Nội, TP.HCM và Đà Nẵng

So sánh số tin và mặt bằng lương được ghi nhận giữa ba khu vực. Giữ đầy đủ tin tuyển nhiều địa điểm, phân biệt cách đếm tại từng nơi với cách phân bổ trọng số để tránh nhầm tổng số tin.

### 2.2.5 — Mở rộng: kỹ năng theo cấp bậc trong cùng nhóm nghề

So sánh IT tổng quát, Phần mềm/Web, Dữ liệu/AI và Cloud/DevOps: bốn nhóm đã xác định nghề có nhiều tin nhất trong mẫu. Mỗi nhóm trình bày 12 kỹ năng phổ biến nhất của chính nhóm đó, dùng chung các cột cho các cấp bậc.

### 2.2.6 — Mở rộng: ngoại ngữ và quyền lợi theo nhóm nghề

Thống kê tỷ lệ đề cập ngoại ngữ và các quyền lợi như đào tạo, thưởng, bảo hiểm trong từng nhóm nghề. Bổ sung góc nhìn ngoài lương, đồng thời phân biệt dữ liệu thiếu với danh sách không ghi nhận được nhãn.

### 2.2.7 — Mở rộng: kết quả có phụ thuộc cách đếm và nguồn không?

Đối chiếu thứ hạng kỹ năng theo số tin và số tên công ty đề cập; so sánh tỷ lệ giữa các nền tảng. Kiểm tra xem kết luận có chịu ảnh hưởng bởi một số đơn vị đăng nhiều tin hoặc cơ cấu mẫu của từng nguồn hay không.

> **Lưu ý:** Kết quả mô tả mẫu dữ liệu thu thập, chưa đại diện toàn bộ thị trường IT Việt Nam. Kỹ năng, ngoại ngữ và quyền lợi được thống kê theo nhãn trích xuất; chênh lệch quan sát được chưa chứng minh quan hệ nhân quả.
