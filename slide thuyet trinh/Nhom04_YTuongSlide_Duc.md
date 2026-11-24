# Ý tưởng slide – phần của Đức (Crawler + Pipeline, Bước 2.3 Gom cụm)

> Nhóm 4 · ITEC3417 · 10 slide (5 + 5), dự kiến khoảng 7–8 phút.
> Mọi con số lấy từ `file báo cáo bài nhóm/Nhom04_BaoCao_Gop_Chuong2-4.md` (mục 2.1–2.7 và 4.1), là số đã đối chiếu với dữ liệu ngày 2026-11-23.
> **Nguyên tắc hình:** chỉ dùng hình sinh ra từ chính bài làm, không vẽ minh họa mới.
> - Bước 2.3: `buoc 2.3/gom_cum.ipynb` có đúng 3 biểu đồ, đã xuất sẵn vào `slide thuyet trinh/buoc 2.3/`: `slide thuyet trinh/buoc 2.3/2.3_elbow_silhouette.png` (ô 15), `slide thuyet trinh/buoc 2.3/2.3_kich_thuoc_cum.png` (ô 28), `slide thuyet trinh/buoc 2.3/2.3_pca_2d.png` (ô 47). Mỗi slide 2.3 dựng quanh một biểu đồ.
> - Crawler/pipeline: không có biểu đồ nào trong bài làm (notebook 2.1 không có ô vẽ hình). Thay bằng **ảnh chụp kết quả thật**: terminal chạy crawler, dòng JSONL thô, dòng `raw_data.csv` so với `clean_data.csv`, bảng in ra trong notebook.

Nguyên tắc chung: mỗi slide có một thông điệp chính (dòng **Thông điệp**), tối đa 4–5 gạch đầu dòng, số lớn để đọc từ xa. Phần chi tiết để trong lời nói và để trả lời vấn đáp.

---

## PHẦN A. CRAWLER VÀ PIPELINE (5 slide, khoảng 3,5–4 phút)

### Slide A1. Kiến trúc thu thập: 5 nguồn, 2 giai đoạn

**Thông điệp:** crawler tự viết, chỉ mở trang chi tiết cho tin đã lọc IT, nên mở 3.216 trang thay vì 7.388.

**Nội dung:**
- Sơ đồ khối ngang: `5 crawler danh sách` → `data/raw/*.jsonl` → `khử trùng` → `lọc IT` → `fetch_detail (JD đầy đủ)` → `trích kỹ năng + lương` → `raw_data.csv / clean_data.csv`.
- Giai đoạn 1: trang danh sách (title, công ty, địa điểm, chuỗi lương gốc, tag).
- Giai đoạn 2: trang chi tiết (JD đầy đủ, lương ẩn từ JSON-LD).
- Thời điểm cào: 17/11/2026, 17:37:59 → 22:07:29.

**Hình:** ảnh chụp cây thư mục `crawler/` + `pipline/` trong VS Code (bài làm thật), chú thích mỗi file thuộc bước nào. Chuỗi bước viết bằng một dòng chữ, không vẽ sơ đồ.

**Lời nói (gợi ý):** "Chúng em không cào chi tiết ngay, vì hơn một nửa tin ở TopDev là tin ngân hàng. Lọc trước rồi mới mở trang chi tiết giúp giảm hơn một nửa số request."

---

### Slide A2. Mỗi nguồn một cách cào

**Thông điệp:** chọn kỹ thuật theo đặc điểm từng trang, không dùng dịch vụ cào trả phí.

**Nội dung (bảng 5 dòng):**

| Nguồn | Cách cào | Phân trang | Nghỉ ngẫu nhiên | Tin thô |
|---|---|---|---|---:|
| ITviec | Playwright + chờ `div.job-card` | tự động `?page=N` | 2–5 s | 604 |
| TopDev | Neo vào link `/detail-jobs/` (class Tailwind vô nghĩa) | tự động | 3–6 s | 2.910 |
| VietnamWorks | Gọi thẳng API JSON tìm được qua tab Network | tự động, 50 tin/trang | 1–3 s | 701 |
| TopCV | Cloudflare chặn → bán tự động, đọc DOM mỗi 2–3 s | người bấm | — | 2.426 |
| LinkedIn | Cần đăng nhập → hồ sơ Chrome lưu cookie | người cuộn | — | 747 |

**Hình:** ảnh chụp 1 dòng JSONL thật trong `data/raw/itviec.jsonl` (thấy `job_url`, `Timestamp`, chuỗi lương gốc).

**Lời nói:** nhấn mạnh 3 yêu cầu của đề: phân trang, tải động (Playwright chờ render), delay ngẫu nhiên `random.uniform`. Nói thẳng TopCV/LinkedIn là bán tự động và vì sao.

**Câu hỏi vấn đáp dự kiến:** "Vì sao VietnamWorks không cần trình duyệt?" (có API JSON trả sẵn dữ liệu). "TopCV có phải crawler tự động không?" (trả lời trung thực: bán tự động do Cloudflare).

---

### Slide A3. Thách thức kỹ thuật đã vượt qua

**Thông điệp:** mỗi khó khăn có một cách xử lý cụ thể trong code.

**Nội dung (4 ô, mỗi ô: vấn đề → cách giải):**
- **Tải động (JS):** Playwright + Chrome thật, `wait_for_selector` thay vì chờ cứng.
- **Chống chặn:** `random.uniform`, ẩn `navigator.webdriver`, UA máy thật; gặp Cloudflare thì mở lại trình duyệt, thử tối đa 5 lần.
- **Lương bị ẩn:** đọc `baseSalary` trong JSON-LD: 649 tin có trường lương, chỉ 151 tin ra số (còn lại "You'll love it", "Negotiable").
- **Dừng giữa chừng không mất dữ liệu:** `fetch_detail` lưu mỗi 20 tin qua file tạm, chạy tiếp được.
- **Audit:** 100% dòng có `URL_Goc` + `Timestamp`.

**Hình:** ảnh chụp terminal khi chạy lệnh demo dưới đây (thanh `tqdm`, số tin mỗi trang). Nên chụp trước để dự phòng mạng lỗi.

**Gợi ý demo live (nếu có thời gian, làm ở slide này):**
```
python -m crawler.itviec --pages 1 --limit 10 --headless --out data/raw/demo_itviec.jsonl
```
Xóa file demo cũ trước khi chạy (script từ chối ghi đè). **Không demo VietnamWorks với `--out` mặc định** (ghi đè `data/raw/vietnamworks.jsonl`, lỗi K1 chưa sửa) và không demo LinkedIn (cần đăng nhập).

---

### Slide A4. Phễu dữ liệu: từ 7.388 tin thô đến 3.135 việc làm

**Thông điệp:** mỗi nấc bỏ tin đều có lý do và con số.

**Hình:** không có biểu đồ phễu trong bài làm → dùng chính bảng dưới (số lớn, căn phải), không vẽ thêm.

| Nấc | Số tin | Ghi chú |
|---|---:|---|
| Tin thô | 7.388 | 5 nguồn |
| Khử trùng trong nguồn | 7.291 | trùng URL hoặc cùng nguồn + công ty + chức danh |
| Lọc phạm vi IT | 3.216 | loại 4.075 (TopDev 2.830, phần lớn tin MBBank) |
| Chốt IT → `raw_data.csv`, `clean_data.csv` | 3.204 | bỏ 12 tin đọc tay ngoài IT |
| Một dòng mỗi việc làm (`is_primary`) | 3.135 | 67 nhóm đăng trên nhiều nguồn |

**Ý phụ (1 dòng dưới phễu):** lọc IT không dùng AI: luật trên tiêu đề + tag; "IT Sales" là bán hàng; `BA`, `SAP`, `IT` so khớp phân biệt hoa thường để không nhầm "Ba Vì", "sắp xếp".

**Lời nói:** 3.204 ≥ 800 tin, đạt yêu cầu đề. Ghép chéo nguồn bằng `rapidfuzz` (công ty và chức danh ≥ 90 điểm), không xóa dòng mà gắn `job_group_id`.

**Câu hỏi dự kiến:** "7.291 tạo bằng khóa nào?" → khóa cũ không có thành phố; code hiện tại thêm thành phố sẽ ra 7.308 (chưa chạy lại phễu). Nên chuẩn bị sẵn câu trả lời này.

---

### Slide A5. Làm sạch: lương, địa điểm, cấp bậc, kỹ năng

**Thông điệp:** mọi giá trị chuẩn hóa đều kèm căn cứ (`*_source`), "không biết" thì để trống, không điền 0.

**Nội dung (2 cột):**
- **Lương (regex, `clean_salary.py`):** quy về VND/tháng, 1 USD = 25.000 VND, lương năm ÷ 12, ngoài 1–300 triệu là `invalid`. Bảng ví dụ 3 dòng:
  - `Thoả thuận` → trống, `negotiable`
  - `1,000 - 2,000 USD` → 25M – 50M, `closed_range`
  - `300 - 400 triệu/năm` → 25M – 33,3M
- **Kết quả lương (3.204 tin):** negotiable 1.557 · closed_range 767 · missing 674 · max_only 153 · khác 53.
- **Giả định duy nhất:** TopCV không ghi kỳ trả → coi là lương tháng, gắn cờ `salary_period_assumed` (529 tin).
- **Địa điểm:** TP.HCM 1.569 · Hà Nội 1.391 · Khác 189 · Đà Nẵng 40.
- **Cấp bậc:** chức danh → nhãn nguồn → số năm; ghi `seniority_source` để bước phân lớp tránh rò rỉ nhãn.
- **Hình:** ảnh chụp cùng một tin ở `raw_data.csv` (`salary_raw`, `location_raw`) và `clean_data.csv` (`Luong_Min`, `Luong_Max`, `city`, `tech`): thấy ngay trước/sau làm sạch.
- **Kỹ năng:** từ điển tự viết 161 mục (135 `tech` + 26 `keywords`); bẫy đã xử lý: "test" ≠ "Contest", "React Native" ≠ "React", `C`/`R`/`Go` chỉ nhận trong danh sách. 2.774 tin có ≥ 1 công nghệ.

**Lời nói:** cầu nối sang phần sau: "Đầu ra là `clean_data.csv` 41 cột, Tuấn làm tiếp phần notebook 2.1 và EDA." (nếu thứ tự thuyết trình là Đức → Tuấn → Đức 2.3 thì đổi câu nối cho phù hợp).

---

## PHẦN B. BƯỚC 2.3 GOM CỤM (5 slide, khoảng 3,5–4 phút)

### Slide B1. Bài toán và dữ liệu đầu vào

**Thông điệp:** gom 2.218 tin IT thành các nhóm kỹ năng bằng K-Means trên ma trận 0/1.

**Nội dung:**
- **Câu hỏi:** tin tuyển dụng IT chia thành những nhóm kỹ năng nào, mỗi nhóm có chân dung ra sao?
- **Mẫu số (phễu nhỏ):** 3.135 → 2.869 tin IT dùng sơ bộ → 2.843 có JD → bỏ 175 không có kỹ năng, bỏ 450 có < 3 kỹ năng → **2.218 tin × 143 kỹ năng**.
- **Hai ngưỡng (`build_matrix`, lặp đến khi ổn định):** kỹ năng ≥ 10 tin, tin ≥ 3 kỹ năng. Lý do: ma trận rất thưa (trung vị 6 kỹ năng/tin).
- **Chuẩn hóa L2:** để K-Means không chia theo "ghi nhiều hay ít kỹ năng". Bằng chứng: η² của số kỹ năng/tin ở K = 8 giảm 0,51 → 0,31.

**Hình:** không có biểu đồ ở bước này → ảnh chụp output ô `build_matrix` trong `gom_cum.ipynb` (in số tin/kỹ năng sau lọc) hoặc `X.head()` của ma trận 0/1.

**Câu hỏi dự kiến:** "Vì sao không dùng TF-IDF trên JD?" → kỹ năng đã chuẩn hóa dễ đặt tên cụm và giải thích hơn. "L2 khác gì không chuẩn hóa?" → tương đương khoảng cách cosine.

---

### Slide B2. Chọn K: Elbow + Silhouette + độ ổn định

**Thông điệp:** Elbow không có khuỷu rõ, nên dùng quy tắc đặt trước; K = 8 là K nhỏ nhất giữ đủ 8 hồ sơ kỹ năng.

**Hình chính:** `slide thuyet trinh/buoc 2.3/2.3_elbow_silhouette.png` (ô 15 của notebook), chiếm 2/3 slide; chữ chỉ là 3 dòng ghi chú bên cạnh. Chỉ vào hình khi nói: đường inertia trái không gãy, vòng tròn bên phải là các K đạt quy tắc, K = 8 cao nhất trong số đó.

**Nội dung:**
- K = 2…12, `KMeans(n_init=10, random_state=42)`.
- Elbow: inertia giảm đều 3,9% → 1,4% mỗi bước, không có khuỷu.
- Silhouette < 0,1 ở mọi K: kỹ năng chung (Git, Docker, SQL) có ở mọi nhóm nghề.
- **Quy tắc đặt trước:** cụm nhỏ nhất ≥ 5%, ARI trung bình ≥ 0,80, rồi Silhouette cao nhất → K ∈ {2, 3, 6, 7, 8} → **K = 8** (Silhouette 0,074, ARI 0,860).
- **Vì sao không K = 7:** chạy 20 hạt giống, K = 7 mất cụm Backend Java 19/20 lần; K = 8 không mất hồ sơ nào (0/20).

**Lời nói:** đây là slide hay bị hỏi nhất ("Elbow và Silhouette gợi ý khác nhau thì sao?"). Trả lời: Silhouette K = 6–8 chỉ chênh 0,002–0,004, nên dùng tiêu chí ổn định để quyết định.

---

### Slide B3. Chân dung 8 cụm

**Thông điệp:** đặt tên cụm bằng lift, không bằng tần suất thô, vì Python/SQL/Git có ở mọi cụm.

**Hình chính:** `slide thuyet trinh/buoc 2.3/2.3_kich_thuoc_cum.png` (ô 28) chiếm nửa trái; bảng rút gọn bên phải chỉ giữ 2–3 kỹ năng lift cao nhất mỗi cụm.

| Cụm | Số tin | Kỹ năng đặc trưng (lift) |
|---|---:|---|
| Frontend và Web | 363 | CSS 5,5× · HTML 5,3× · JavaScript 3,7× |
| Dữ liệu doanh nghiệp | 339 | Power BI 5,0× · ERP 3,0× · SQL 2,7× |
| Backend Java | 331 | Spring Boot 5,1× · Redis 4,7× · Kafka 4,4× |
| DevOps / Cloud | 323 | Ansible 5,4× · Terraform 5,2× · Jenkins 3,6× |
| Data Science / ML / LLM | 296 | Machine Learning 4,4× · LLM 3,9× |
| Hệ thống, mạng, bảo mật | 211 | Cisco 9,0× · Windows Server 7,9× · Firewall 7,7× |
| Agile / Scrum | 206 | Scrum 6,0× · Jira 4,5× · Agile 4,3× |
| C / C++ và Linux | 149 | C 12,6× · C++ 10,5× · Linux 2,7× |

**Lời nói:**
- Công thức lift = P(kỹ năng | cụm) ÷ P(kỹ năng toàn mẫu); chỉ xét kỹ năng có ở ≥ 25% tin trong cụm.
- Kiểm tra hợp lý: nhóm nghề không tham gia gom cụm nhưng khớp (DevOps 53% tin `devops_cloud`, C/C++ 21% `embedded`).
- Cấp bậc: Backend Java và DevOps nghiêng về Senior/Lead; mạng và C/C++ nhiều Junior nhất (36%, 35%).
- **Không xếp hạng lương giữa cụm** (mỗi cụm chỉ 25–89 tin có lương, phần lớn TopCV có giả định kỳ trả).

---

### Slide B4. Cụm có đáng tin không?

**Thông điệp:** tên cụm bền, ranh giới cụm thì mềm.

**Nội dung (3 ý, mỗi ý một con số lớn):**
- **20/20** lần chạy lại vẫn đủ 8 hồ sơ; trung vị **96,3%** tin giữ nguyên tên cụm (thấp nhất 85,3%). Data Science (239–388 tin) và Dữ liệu doanh nghiệp (248–354) dao động nhiều nhất vì cùng có Python/SQL → kích thước cụm chỉ báo theo khoảng.
- **Silhouette 0,074**, 305/2.218 tin (13,8%) có Silhouette âm. Tách rõ nhất: C/C++ 0,196, Agile/Scrum 0,142. Kém nhất: Frontend 0,016 (38,6% tin âm, nhiều tin gần Backend Java).
- **Lệch nguồn:** cụm mạng/bảo mật 57% TopCV, cụm C/C++ 48% LinkedIn (toàn mẫu: TopCV 35%, LinkedIn 26%).

**Hình chính:** `slide thuyet trinh/buoc 2.3/2.3_pca_2d.png` (ô 47) chiếm 2/3 slide, chú thích "PCA chỉ để minh họa, 2 trục giữ 12,8% biến thiên". Chỉ vào hình: DevOps/Cloud bên trái, Agile/Scrum dưới phải, các vùng chồng lấn khớp với Silhouette thấp. Ba ý dưới đây rút thành 3 con số đặt cạnh hình.

**Lời nói:** ví dụ ranh giới: "Senior .NET Developer" có React cùng ASP.NET, PostgreSQL → Silhouette −0,001, nằm giữa Frontend và Backend.

---

### Slide B5. Kết luận và giới hạn

**Thông điệp:** 8 phân khúc kỹ năng đặt tên được, là xu hướng chồng lấn chứ không phải nhóm nghề tách biệt.

**Nội dung:**
- **Kết quả:** 8 phân khúc: Frontend/Web, Dữ liệu doanh nghiệp, Backend Java, DevOps/Cloud, Data Science/ML/LLM, Mạng–bảo mật, Agile/Scrum, C/C++.
- **Giới hạn:**
  - Silhouette thấp (< 0,1): kỹ năng chung làm ranh giới mờ.
  - Ngưỡng ≥ 3 kỹ năng/tin đã loại 625 tin (22,0% tin có JD); cụm phụ thuộc chất lượng từ điển kỹ năng.
  - Kích thước cụm đổi nhẹ theo phiên bản scikit-learn; `cluster_assignments.csv` đã commit là bản chốt.
- **Đầu ra cho bước sau:** `buoc 2.3/data/cluster_assignments.csv` (2.843 dòng: `job_uid`, `cluster_id`, `cluster_name`, `cluster_status`).

**Hình:** không thêm hình mới; có thể đặt lại thu nhỏ `2.3_kich_thuoc_cum.png` cạnh danh sách 8 tên cụm.

**Câu nối sang Thành (2.4):** "Cụm cho biết kỹ năng nào hay đi cùng nhau theo nhóm; luật kết hợp ở phần sau đo cụ thể cặp kỹ năng nào kéo theo nhau, ví dụ Spring Boot ⇒ Java."

---

## Việc cần làm trước khi dựng slide

- [ ] Chụp ảnh kết quả thật cho phần A: cây thư mục (A1), dòng JSONL (A2), terminal demo (A3), cặp dòng raw/clean (A5).
- [ ] Không chụp ô notebook có đường dẫn máy cá nhân (`/Users/tuan/...` ở cuối notebook 2.1).
- [ ] Chuẩn bị file `data/raw/demo_itviec.jsonl` trống (xóa bản cũ) nếu demo live ở slide A3.
- [ ] Thống nhất với nhóm: sửa slide 3 của `Nhom04_SuonSlide.pdf` ("3.216 tin, chỉ 8 lỗi" → "3.184 có JD, 3.178 đầy đủ").
