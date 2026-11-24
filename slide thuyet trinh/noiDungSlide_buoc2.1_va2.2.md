# NỘI DUNG SLIDE — GIỚI THIỆU DỮ LIỆU, BƯỚC 2.1 VÀ BƯỚC 2.2

## Slide 1 — Giới thiệu file clean_data_processed.csv

- Là bản **bổ sung đặc trưng phục vụ phân tích** từ `clean_data.csv`.
- Quy mô: **3.135 dòng, 47 cột**, sau khi tách **69 bản ghi phụ** khỏi 3.204 dòng ban đầu.
- Có **2.869 tin IT dùng sơ bộ** cho các phân tích tiếp theo.

| Dữ liệu có thể tận dụng trong tập IT sơ bộ | Số tin |
| :--- | ---: |
| Có mô tả công việc (JD) | 2.843 |
| Được gắn nhãn Intern / Fresher | 92 / 130 |
| Có ít nhất một ngoại ngữ được ghi nhận | 1.552 |
| Có ít nhất một quyền lợi được ghi nhận | 2.197 |
| Đủ điều kiện phân tích lương | 688 |
| Xác định được hình thức làm việc | 745 |

---

## Slide 2 — Những đặc trưng được bổ sung

| Nhóm thông tin | Bổ sung trong clean_data_processed.csv |
| :--- | :--- |
| Phạm vi IT | `scope_status`: phân biệt tin dùng sơ bộ và tin chờ rà soát. |
| Lương | **Giữ nguyên giá trị VNĐ**; thêm cờ kiểm tra và điều kiện chọn mẫu phân tích. |
| Địa điểm, hình thức làm việc | `cities_clean`, `n_cities`, `work_mode_clean`; giữ thông tin tuyển nhiều nơi, ghi thiếu thành `Unknown`. |
| Kinh nghiệm, cấp bậc | `experience_group`, `seniority_clean`; bổ sung cờ kiểm tra kinh nghiệm, tách Intern và nhận diện một số tin tuyển nhiều cấp bậc. |
| Mô tả công việc | `description_clean`, `text_for_tfidf`: văn bản đã làm sạch để phân tích. |
| Kỹ năng | `skills`: chuẩn hóa tên, gộp và loại trùng; tách khái niệm chung và lĩnh vực ứng dụng. |

**Nguyên tắc:** Giữ các cột gốc để đối chiếu; bổ sung đặc trưng và cờ lựa chọn dữ liệu phù hợp với từng phân tích.

---

## Slide 3 — Ví dụ chuẩn hóa kỹ năng và giá trị sử dụng

**Thông tin ban đầu:**

```text
tech:     ["Python", "SQL"]
keywords: ["SQL", "AWS", "Cloud", "Fintech"]
```

**Sau xử lý, giữ hai cột gốc và bổ sung:**

```text
skills:              ["AWS", "Python", "SQL"]
broad_concepts:      ["Cloud"]
application_domains: ["Fintech"]
n_skills:            3
```

- **SQL được loại trùng** khi gộp hai danh sách.
- **Cloud** được tách thành khái niệm chung; **Fintech** là lĩnh vực ứng dụng.
- Danh sách `skills` có thể dùng để tạo **ma trận kỹ năng 0/1**.

**Giá trị chính:** Có sẵn đặc trưng và cờ lựa chọn dữ liệu, giúp EDA, gom cụm và luật kết hợp không phải xử lý lại từ đầu.

---

## Slide 4 — Bước 2.1: Kiểm tra, chuẩn hóa và tạo đặc trưng

Kiểm tra, chuẩn hóa dữ liệu và tạo các đặc trưng để sử dụng thống nhất cho **EDA, gom cụm, luật kết hợp và phân lớp**.

| Nhóm cột | Đã làm gì? | Phục vụ bước tiếp theo |
| :--- | :--- | :--- |
| Mã tin | Giữ một tin đại diện cho mỗi nhóm, tách 69 bản ghi phụ để truy vết. | Hạn chế đếm trùng khi thống kê và phân tích. |
| Phạm vi IT | Phân biệt tin IT dùng sơ bộ và tin chưa rõ cần rà soát. | Chọn đúng tập tin phục vụ phân tích ngành IT. |
| Lương | Kiểm tra tính hợp lệ, sự nhất quán và đánh dấu tin đủ điều kiện phân tích; giữ nguyên giá trị lương gốc. | Thống kê, so sánh mức lương trên tập dữ liệu phù hợp. |
| Địa điểm, hình thức làm việc | Chuẩn hóa danh sách địa điểm; ghi rõ hình thức làm việc chưa xác định. | Phân tích tuyển dụng theo địa phương và Onsite/Hybrid/Remote. |
| Kinh nghiệm, cấp bậc | Kiểm tra khoảng kinh nghiệm, chia nhóm số năm; nhận diện Intern và tin tuyển nhiều cấp bậc. | So sánh nhu cầu theo kinh nghiệm, cấp bậc và chuẩn bị nhãn cho phân lớp. |
| Mô tả công việc | Làm sạch HTML, ký tự thừa và khoảng trắng; tạo văn bản phục vụ TF-IDF. | Chuẩn bị dữ liệu cho phân tích văn bản khi cần. |
| Kỹ năng | Thống nhất tên, gộp và loại trùng; tách khái niệm chung, lĩnh vực ứng dụng; tạo ma trận kỹ năng 0/1. | Thống kê kỹ năng phổ biến, gom cụm và tìm các kỹ năng thường xuất hiện cùng nhau. |

**Đầu vào:** `clean_data.csv` → **Đầu ra:** `clean_data_processed.csv` với **3.135 tin đại diện**.

---

## Slide 5 — Bước 2.2.1: Cơ cấu và độ bao phủ dữ liệu

- **2.869 tin IT** được lựa chọn từ 3.135 tin đại diện sau tiền xử lý.
- **2.843 tin có mô tả công việc (JD) — 99,09%**.
- **688 tin đủ điều kiện phân tích lương — 23,98%**.
- TopCV chiếm **39,84% tổng mẫu**, nhưng đóng góp **68,02% tập lương hợp lệ**.

**Thông điệp chính:** Độ bao phủ JD cao, nhưng thông tin lương còn hạn chế và không đồng đều giữa các nguồn.

---

## Slide 6 — Bước 2.2.2: Top 15 kỹ năng được ghi nhận nhiều nhất

| Kỹ năng dẫn đầu | Số tin đề cập | Tỷ lệ |
| :--- | ---: | ---: |
| Python | 912 | 32,08% |
| SQL | 802 | 28,21% |
| CI/CD | 711 | 25,01% |

- Docker, AWS, Agile và Java cũng xuất hiện phổ biến, mỗi kỹ năng đạt khoảng **18–19%**.
- Mẫu số: **2.843 tin có JD**, bao gồm cả tin không trích xuất được kỹ năng.
- Mỗi kỹ năng được tính tối đa **một lần trong mỗi tin**.

**Thông điệp chính:** Python, SQL và CI/CD nổi bật trong mẫu; tần suất đề cập không đồng nghĩa với số người cần tuyển.

---

## Slide 7 — Bước 2.2.3: Phân bố lương theo cấp bậc

- Phân tích **688 tin có lương hợp lệ**, sử dụng trung vị và biểu đồ hộp.

| Cấp bậc | Intern | Fresher | Junior | Middle | Senior | Lead |
| :--- | ---: | ---: | ---: | ---: | ---: | ---: |
| Trung vị lương (triệu VNĐ) | 4,00 | 13,50 | 18,00 | 30,00 | 37,50 | 30,50 |

- Trung vị tăng từ Intern đến Senior; mức lương phân hóa mạnh hơn ở các bậc cao.
- Lead có trung vị thấp hơn Senior trong mẫu, chưa đủ để kết luận về mặt bằng lương của hai cấp bậc.
- Khi chỉ xét **321 tin không cần giả định chu kỳ lương**, trung vị chung tăng từ **22 lên 28,12 triệu VNĐ**.

**Lưu ý:** Số liệu là **trung điểm khoảng lương chào tuyển**, không phải lương thực nhận; kết quả còn phụ thuộc cách chọn tập dữ liệu.

---

## Slide 8 — Bước 2.2.4: So sánh Hà Nội, TP.HCM và Đà Nẵng

| Khu vực | Số tin có tuyển tại địa điểm | Số tin có lương hợp lệ | Trung vị lương (triệu VNĐ) |
| :--- | ---: | ---: | ---: |
| Hà Nội | 1.239 | 407 | 22,50 |
| TP.HCM | 1.474 | 231 | 21,00 |
| Đà Nẵng | 49 | 10 | 41,25 |

- Mẫu tuyển dụng tập trung chủ yếu tại **TP.HCM và Hà Nội**.
- Một tin tuyển nhiều địa điểm được ghi nhận tại từng nơi; phân bổ trọng số **1/k** giúp bảo toàn tổng số tin khi tổng hợp.

**Thông điệp chính:** Đà Nẵng chỉ có 10 tin có lương hợp lệ, chưa đủ cơ sở để kết luận khu vực này có mặt bằng lương cao hơn.

---

## Slide 9 — Bước 2.2.5: Kỹ năng theo cấp bậc trong cùng nhóm nghề

- Phân tích **bốn nhóm nghề đã xác định có nhiều tin nhất**.
- Mỗi nhóm chọn **12 kỹ năng phổ biến nhất**; chỉ so sánh các bậc có **ít nhất 20 tin có JD**.

| Nhóm nghề | Phát hiện tiêu biểu |
| :--- | :--- |
| IT tổng quát | AWS: Junior **2,52%**, Senior **22,00%**. |
| Phần mềm/Web | CI/CD: Intern **12,12%**, Senior **50,46%**. |
| Dữ liệu/AI | Python **65,31–77,57%**; SQL **60,00–70,41%** ở bốn bậc đủ mẫu. |
| Cloud/DevOps | Terraform: Junior **42,86%**, Senior **60,34%**. |

**Thông điệp chính:** Hồ sơ kỹ năng khác nhau theo nhóm nghề và cấp bậc; tỷ lệ đề cập kỹ năng không tăng đồng loạt theo cấp bậc.

---

## Slide 10 — Bước 2.2.6: Ngoại ngữ và quyền lợi theo nhóm nghề

Trong **bốn nhóm nghề chính: IT tổng quát, Phần mềm/Web, Dữ liệu/AI và Cloud/DevOps**:

| Thông tin được đề cập | Khoảng tỷ lệ giữa bốn nhóm |
| :--- | ---: |
| Tiếng Anh | **47,1–57,5%** |
| Thưởng KPI/dự án/lễ tết | **76,2–78,2%** |
| Đào tạo/chứng chỉ | **51,9–57,7%** |

- **Tiếng Anh** là ngoại ngữ được ghi nhận phổ biến nhất.
- Mẫu số tính riêng theo từng trường: **giữ danh sách rỗng, loại dữ liệu thiếu**.

**Thông điệp chính:** Ngoại ngữ và quyền lợi bổ sung góc nhìn ngoài lương; không đề cập không đồng nghĩa với không yêu cầu hoặc không cung cấp.

---

## Slide 11 — Bước 2.2.7: Kết quả có phụ thuộc cách đếm và nguồn không?

- Đối chiếu kỹ năng theo **số tin** và **số tên công ty đề cập**.
- Mỗi tên công ty đóng góp tối đa **một lần cho mỗi kỹ năng**.

| Kỹ năng | Số tin đề cập | Số tên công ty đề cập | Hạng theo tin / tên công ty |
| :--- | ---: | ---: | :---: |
| Python | 912 | 552 | 1 / 1 |
| SQL | 802 | 538 | 2 / 2 |
| CI/CD | 711 | 441 | 3 / 3 |

- Khi đếm theo tên công ty, Git tăng từ hạng **9 lên đồng hạng 4**; AWS giảm từ hạng **5 xuống hạng 8**.
- Tỷ lệ đề cập Python trên tập tin có JD: **LinkedIn 46,9%**, **TopCV 22,5%**.

**Thông điệp chính:** Ba kỹ năng dẫn đầu tương đối ổn định, nhưng kết luận vẫn phụ thuộc cách đếm và cơ cấu nguồn; chưa thể khái quát trực tiếp cho toàn thị trường IT Việt Nam.
