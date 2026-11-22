# MiniNote 2.5 — Phân lớp nhóm nghề CNTT

## 1. Tổng quan và mục tiêu
Bước 2.5 xây dựng mô hình phân lớp `title_role` (15 nhóm nghề CNTT), so sánh DummyClassifier, Decision Tree và Random Forest. Tài liệu này tổng hợp kết quả quan sát từ Notebook và ảnh output do thành viên nhóm cung cấp; những kiểm tra chưa có bằng chứng được ghi rõ.

## 2. Nguồn dữ liệu và kế thừa 2.1–2.4
- Nguồn chạy: `buoc2.1_2.2/data/processed/clean_data_processed.csv` trong repository chính.
- Dữ liệu đọc thành công: **3.135 dòng × 47 cột**.
- Sau loại 30 bản ghi mô tả rỗng: **3.105 dòng, 15 lớp**.
- Khác với `clean_data.csv` (3.204 × 41), file processed là nguồn dùng cho phân lớp; tránh nhầm phiên bản.
- Các bước EDA (2.2), clustering (2.3), Apriori (2.4) cung cấp bối cảnh để lựa chọn đặc trưng. Chưa có đối chiếu định lượng đầy đủ giữa đầu ra 2.3–2.4 và đặc trưng mô hình trong các ảnh được cung cấp.

## 3. Nhật ký 12 bước thực hiện

### 2.5.1 — Nạp và kiểm tra dữ liệu đầu vào
**Mục đích:** xác nhận file, số dòng/cột, schema và mẫu dữ liệu. **Lý do:** dùng nhầm CSV sẽ làm sai chuỗi xử lý. **Lưu ý:** nên dùng đường dẫn tương đối theo repository, không hard-code ổ D của một máy.

### 2.5.2 — Khảo sát nhãn phân lớp
**Mục đích:** kiểm tra nhãn hiện có, số lớp, thiếu nhãn và phân bố. Có hai cột ứng viên `salary_status` (8 lớp) và `title_role` (15 lớp), đều không thiếu ở 3.135 dòng. Chọn `title_role` cho bài toán phân loại nhóm nghề. **Lưu ý:** `salary_status` phản ánh trạng thái lương, không phải cấp bậc; không đánh đồng với `seniority_clean`.

### 2.5.3 — Kiểm tra chất lượng trường và khả năng loại trừ
**Mục đích:** xem phân bố nhãn, dữ liệu thiếu, độ phân biệt của cột và trùng dòng. Phát hiện 29 lớp/cột được báo là nhóm nhỏ trong bước khảo sát cột (không phải 29 lớp của nhãn), và số dòng trùng toàn bộ bằng 0 trong kiểm tra được chụp. **Lưu ý:** không đưa ID, URL hoặc cột nhãn vào tập đặc trưng chỉ vì chúng có nhiều giá trị phân biệt.

### 2.5.4 — Phân bố 15 lớp và kiểm tra trường ứng viên
**Mục đích:** đánh giá mất cân bằng, nhóm nghề hiếm và mức đầy đủ của dữ liệu. `title` thiếu 0, có 2.801 giá trị duy nhất; `description_clean` thiếu 30, có 3.097 giá trị duy nhất; `title_role` không thiếu và có 15 lớp. Một số trường như `tech`, `keywords`, `seniority` không tồn tại dưới đúng tên đó trong file được kiểm tra. **Lưu ý:** cần kiểm tra tên cột thực tế trước khi viết code chọn đặc trưng.

### 2.5.5 — Chuẩn bị dữ liệu phân lớp
**Mục đích:** loại 30 mô tả rỗng, giữ 3.105 mẫu, 15 lớp. Kiểm tra văn bản rỗng = 0, nhãn thiếu = 0, độ dài mô tả trung vị = 3.106 ký tự. **Lưu ý:** ghi rõ quy tắc loại mẫu để tái lập.

### 2.5.6 — Kiểm tra nhóm và nguy cơ rò rỉ
**Mục đích:** xác minh `job_group_id`, trùng nội dung, phân bố theo nguồn và các cột có thể gây leakage. Quan sát: 3.105 group khác nhau, không group thiếu, không group nhiều mẫu; có 8 mô tả trùng trong một phép kiểm tra. **Lưu ý:** ID khác nhau không chứng minh nội dung độc lập; cần rà soát các tin đăng giống nhau.

### 2.5.7 — Thiết kế X, y
**Mục đích:** xây dựng đặc trưng và nhãn, xem trùng mô tả và xung đột nhãn. Có 15 bản ghi nằm trong nhóm mô tả trùng và 3 nhóm mô tả có nhiều nhãn; X và y đều có 3.105 mẫu. **Lưu ý:** `title_role` có thể được suy ra từ tiêu đề; nếu dùng `title` để dự đoán `title_role`, cần nêu rõ nguy cơ học lại quy tắc gán nhãn, và cân nhắc thử nghiệm loại tiêu đề.

### 2.5.8 — Chia Train/Test và baseline
**Mục đích:** tạo thước đo tối thiểu trước khi huấn luyện. Train = 2.484, Test = 621 (80/20); kiểm tra mô tả trùng giữa hai tập = 0. DummyClassifier: Accuracy = 0,1787, Macro F1 = 0,0202. **Lưu ý:** Macro F1 thấp cho thấy chỉ dự đoán lớp phổ biến không giải quyết được bài toán nhiều lớp.

### 2.5.9 — Decision Tree và kiểm tra bổ sung
**Mục đích:** tối ưu cây với cross-validation, xem khả năng tổng quát hóa và độ phức tạp. Best CV Macro F1 = 0,3541; tham số tốt nhất: `ccp_alpha=0.001`, `max_depth=None`, `min_samples_leaf=5`. Train Macro F1 = 0,6838; Test Accuracy = 0,3688; Test Macro F1 = 0,3567. Gap = 0,3271; độ sâu 40, 182 lá, 363 nút. **Lưu ý:** chênh lệch Train/Test lớn cho thấy nguy cơ overfitting; cần giải thích tác dụng cắt tỉa `ccp_alpha`.

### 2.5.10 — Random Forest và kiểm tra bổ sung
**Mục đích:** so sánh mô hình tổ hợp với cây đơn và baseline. Tối ưu 18 cấu hình × 3 folds = 54 fits. Tham số tốt nhất: `max_depth=40`, `min_samples_leaf=5`, `n_estimators=200`. Best CV Macro F1 = 0,4495; Train Macro F1 = 0,8995; Test Accuracy = 0,5797; Test Macro F1 = 0,5474; gap = 0,3521. CV theo fold: 0,4213 / 0,5092 / 0,4180; trung bình 0,4495, độ lệch chuẩn 0,0422. Trung bình độ sâu cây 39,47; trung bình số lá 234,66. **Lưu ý:** dù vượt Decision Tree trên test, gap lớn vẫn cảnh báo overfitting.

### 2.5.11 — Đánh giá, trực quan hóa và phân tích lỗi
**Mục đích:** so sánh thống nhất ba mô hình bằng Accuracy, Macro Precision, Macro Recall, Macro F1, Weighted F1; lập confusion matrix, recall theo lớp và cặp nhầm lẫn. **Lưu ý:** DummyClassifier dùng bộ đặc trưng baseline riêng; Decision Tree/Random Forest phải dự đoán bằng pipeline đã fit, không gọi riêng `RandomForestClassifier` trên chuỗi văn bản thô.

| Model | Accuracy | Macro Precision | Macro Recall | Macro F1 | Weighted F1 |
|---|---:|---:|---:|---:|---:|
| Random Forest | 0,5797 | 0,5634 | 0,6148 | 0,5474 | 0,5541 |
| Decision Tree | 0,3688 | 0,3359 | 0,4123 | 0,3567 | 0,3598 |
| DummyClassifier | 0,1787 | 0,0119 | 0,0667 | 0,0202 | 0,0542 |

**Điểm làm tốt:** Random Forest nhận diện tốt `embedded` (10/10, recall 1,00), `devops_cloud` (35/40, 0,875), `data_ai` (59/68, 0,8676), `mobile` (13/15, 0,8667), `fullstack` (26/32, 0,8125), `qa_test` (25/31, 0,8065), `pm_po` (40/50, 0,80).

**Nhóm dễ nhầm:** `software` đúng 32/111; nhiều mẫu bị dự đoán thành `generic` (15), `it_general` (15), `qa_test` (11), `fullstack` (10), `embedded` (9). `it_general` đúng 28/89, nhầm sang `data_ai` (14), `system_network` (11), `generic` (8). `generic` đúng 37/82, nhầm sang `system_network` (10), `it_general` (8), `pm_po` (8). `frontend` đúng 1/8, chủ yếu nhầm `fullstack` (5). `unknown` đúng 0/5. Đây là dấu hiệu ranh giới một số nhóm nghề chồng lấn và các lớp ít mẫu chưa ổn định.

**Lưu ý diễn giải:** recall 1,00 của `embedded` chỉ trên 10 mẫu test, chưa đủ để khẳng định mô hình ổn định trong thực tế. Confusion Matrix nên trình bày cả bản số lượng và chuẩn hóa theo hàng để không che khuất lớp nhỏ.

### 2.5.12 — Xuất kết quả và cấu hình
**Mục đích:** tạo đầu ra để báo cáo và chạy lại. Output được ảnh Explorer xác nhận có 8 file trong `output/`:

- `data/confusion_matrix.csv`
- `data/model_comparison.csv`
- `data/recall_by_class.csv`
- `data/top_confusion_pairs.csv`
- `figures/confusion_matrix.png`
- `figures/normalized_confusion_matrix.png`
- `figures/recall_by_class.png`
- `experiment_config.json`

**Lưu ý:** đã xác nhận sự hiện diện qua ảnh; chưa đọc trực tiếp nội dung từng file để kiểm tra schema, độ nhất quán và khả năng tái lập.

## 4. Kết luận
Random Forest dẫn đầu trong ba mô hình với Macro F1 test 0,5474, cao hơn Decision Tree 0,3567 và DummyClassifier 0,0202. Mô hình có tín hiệu phân biệt một số nhóm chuyên môn rõ ràng, nhưng vẫn nhầm các nhóm nghề khái quát/chồng lấn; lớp ít mẫu như `frontend` và `unknown` cần thận trọng. Cả Decision Tree và Random Forest có khoảng cách Train/Test lớn. Kết quả là bằng chứng thực nghiệm trên tập dữ liệu hiện tại, chưa phải bảo đảm hiệu quả ngoài thực tế.

## 5. Hạn chế và hướng phát triển
1. Kiểm tra nguy cơ **label leakage** do `title_role` có thể được xây dựng từ tiêu đề; thực hiện ablation loại `title` và so sánh Macro F1.
2. Kiểm tra mô tả trùng hoặc gần trùng và các nhóm có nhãn mâu thuẫn trước khi chia tập.
3. Phân tích thêm lớp ít mẫu, thử cân bằng lớp hoặc cải thiện quy tắc gán nhãn; không gộp lớp nếu chưa có cơ sở nghiệp vụ.
4. So sánh CV/test một cách nhất quán, ghi rõ seed, quy trình chia và tham số; tránh điều chỉnh nhiều lần theo test.
5. Đối chiếu cụ thể đặc trưng được sử dụng với phát hiện EDA, Clustering và Apriori để làm rõ tính kế thừa.

## 6. Hướng dẫn chạy lại và bàn giao GitHub
1. Mở repository chính trong VS Code, chọn môi trường Python có `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `jupyter`.
2. Mở `buoc2.5/Classification.ipynb` (hoặc vị trí thực tế của Notebook), sửa đường dẫn dữ liệu thành đường dẫn tương đối theo cấu trúc repository.
3. Đảm bảo `buoc2.1_2.2/data/processed/clean_data_processed.csv` tồn tại; chạy notebook theo thứ tự từ đầu bằng **Restart & Run All**.
4. Kiểm tra 3.105 mẫu sau làm sạch, Train 2.484/Test 621, và so sánh các chỉ số với bảng ở trên.
5. Kiểm tra 8 file đầu ra trong `output/`, commit Notebook, MiniNote, README và các kết quả được phép chia sẻ. Không commit môi trường `.venv`, cache, khóa bí mật hay dữ liệu bị hạn chế chia sẻ.

**Trạng thái bàn giao:** Notebook chạy đến 2.5.12 và đã xuất 8 file theo ảnh; cần kiểm tra tái lập, nội dung file và thống nhất đường dẫn trước khi merge lên GitHub.
