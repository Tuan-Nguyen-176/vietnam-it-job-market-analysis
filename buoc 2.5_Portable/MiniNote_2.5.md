# MiniNote 2.5 — Phân lớp khoảng lương chào tuyển trong phạm vi IT

## 1. Mục tiêu và phạm vi

Nhóm sử dụng kỹ năng và số năm kinh nghiệm để dự đoán **nhóm của trung điểm khoảng lương chào tuyển**, so sánh DummyClassifier, Decision Tree có cắt tỉa và Random Forest. `Luong_TB` là giá trị đã xử lý từ khoảng lương đăng tuyển, không phải lương thực nhận của ứng viên.

Notebook dùng cho phiên bản này là `buoc 2.5_Portable/Classification_HoanChinh.ipynb`. Đây là tên file thống nhất để chạy và đối chiếu trong cả ba bản lưu. Nhóm giữ pipeline và cách so sánh mô hình của bản trước, bổ sung lọc `scope_status`, sửa scorer và cập nhật toàn bộ số liệu sau lọc.

Sau khi lọc phạm vi ngành sơ bộ và điều kiện lương, tập mô hình còn **688 tin**, gồm **550 tin train và 138 tin test**. Random Forest được chọn bằng CV Macro F1 và đạt **Accuracy 60,14%, Macro F1 0,6000** trên test. Tuy nhiên, điểm CV của RF và Decision Tree gần bằng nhau; RF còn gap Train/Test **0,2171**, nên chưa thể kết luận mô hình vượt trội ổn định hoặc đã hết overfitting.

## 2. Dữ liệu, phạm vi và nhãn

### 2.1. Nguồn dữ liệu và điều kiện lọc

Nguồn dữ liệu là `buoc2.1_2.2/data/processed/clean_data_processed.csv`, gồm **3.135 dòng × 47 cột**. Nhóm kế thừa trực tiếp dữ liệu processed ở bước 2.1–2.2; chưa đưa kết quả Clustering hoặc Apriori của 2.3–2.4 vào đặc trưng.

| Bước kiểm tra | Số tin |
|---|---:|
| Dữ liệu gốc | 3.135 |
| Đủ điều kiện lương trước lọc phạm vi ngành | 761 |
| Trong số 761 tin: `included_provisional` | 688 |
| Trong số 761 tin: `pending_review`, tạm loại | 73 |
| Tập cuối dùng cho phân lớp | **688** |

Điều kiện cuối gồm `scope_status == "included_provisional"`, `salary_eda_eligible=True`, `Luong_TB` chuyển được sang số, hữu hạn và lớn hơn 0. `scope_status` được kiểm tra trong danh sách cột bắt buộc và chỉ dùng để chọn mẫu, không đưa vào X.

`included_provisional` nghĩa là được chấp nhận **sơ bộ** trong phạm vi ngành. Nhóm tạm loại tin `pending_review` vì chưa hoàn tất rà soát, không khẳng định 73 tin đó đều ngoài IT. Số 761 chỉ còn dùng mô tả bước lọc, không phải số mẫu huấn luyện của phiên bản này.

### 2.2. Nhãn mục tiêu

| Nhãn | Trung điểm khoảng lương đã xử lý | Số tin | Tỷ lệ |
|---|---|---:|---:|
| `duoi_15` | Dưới 15 triệu | 157 | 22,82% |
| `15_den_30` | Từ 15 đến 30 triệu, gồm hai đầu | 323 | 46,95% |
| `tren_30` | Trên 30 triệu | 208 | 30,23% |
| **Tổng** | | **688** | **100,00%** |

Đơn vị mục tiêu là VND/tháng theo dữ liệu đã chuẩn hóa. Quy đổi ngoại tệ, kỳ lương và phân biệt lương cơ bản với tổng thu nhập cần được đối chiếu ở bước đầu nguồn. Các ngưỡng 15 và 30 triệu là quy ước của thí nghiệm. Nhóm không điền lương thiếu để tạo nhãn và không loại mẫu chỉ vì mô tả rỗng.

### 2.3. Đặc trưng

`X = [skills, exp_min_years, exp_max_years]`; `y = salary_class`.

- Đọc `skills` như JSON list, chuẩn hóa chữ thường, loại trùng; mã hóa multi-hot, giữ nguyên kỹ năng nhiều từ và ký hiệu như C++.
- Chuyển kinh nghiệm sang số; giá trị âm hoặc vô hạn coi là thiếu. Kiểm tra kinh nghiệm tối thiểu không lớn hơn tối đa.
- Giữ NaN đến pipeline, dùng median và chỉ báo thiếu trong từng fold; không mặc định thiếu kinh nghiệm là 0 năm.
- Không dùng các cột lương, `salary_status`, `scope_status`, `title_role`, tiêu đề hoặc mô tả làm đặc trưng. Mô tả/ID chỉ giúp nhóm chống trùng và phân tích lỗi sau dự đoán.

## 3. Quy trình thực hiện

Notebook có **sáu bước chính**, đánh số 1–6. Phần này gom các thao tác theo đúng sáu mục trong notebook; những mục 4–9 phía dưới là phần trình bày và bàn luận kết quả, không phải bước chạy bổ sung.

### Bước 1. Kiểm tra dữ liệu, lọc phạm vi và tạo nhãn

Notebook tìm CSV processed theo cấu trúc repo, kiểm tra các cột bắt buộc như `scope_status` và `job_uid`, rồi giữ tin có `scope_status == "included_provisional"` và đủ điều kiện lương. Tập đủ điều kiện lương trước lọc phạm vi là 761; tạm loại 73 tin `pending_review`, còn 688 tin để phân lớp. Việc tạm loại không khẳng định các tin đó đều ngoài IT.

Tạo ba nhãn từ `Luong_TB`: dưới 15 triệu, từ 15 đến 30 triệu (gồm hai ngưỡng), và trên 30 triệu. Phân bố hiện tại là 157/323/208. `Luong_TB` là trung điểm khoảng lương chào tuyển đã xử lý, không phải lương thực nhận.

Đầu vào X chỉ gồm `skills`, `exp_min_years`, `exp_max_years`; y là `salary_class`. Kỹ năng được đọc từ JSON list và mã hóa multi-hot. Kinh nghiệm âm hoặc vô hạn được xem là thiếu; khoảng tối thiểu lớn hơn tối đa bị kiểm tra. Không đưa lương, `scope_status`, tiêu đề hay mô tả vào X. Median và cờ thiếu được học bên trong pipeline theo từng fold.

### Bước 2. Gom nhóm chống rò rỉ và chia tập

Gom các tin nối với nhau qua cùng `job_group_id` hoặc mô tả chuẩn hóa giống nhau bằng thành phần liên thông; không gom mọi mô tả rỗng vào một nhóm. Cách này chưa phát hiện hết mô tả gần trùng hoặc tin đăng lại đã thay nội dung.

Dùng fold đầu của `StratifiedGroupKFold(5, shuffle=True, random_state=42)` làm test: 550 train và 138 test. Trên train, tạo ba fold `StratifiedGroupKFold(3, shuffle=True, random_state=42)` dùng chung cho ba mô hình. Assert kiểm tra đủ lớp và không có nhóm giao nhau giữa train/test hoặc train/validation. Các bảng `split_manifest.csv` và `fold_distribution.csv` lưu thông tin để đối chiếu.

### Bước 3. Pipeline, baseline và tối ưu mô hình

Pipeline gồm mã hóa kỹ năng, điền median kinh nghiệm kèm cờ thiếu và bộ phân lớp. GridSearchCV fit toàn bộ pipeline trong từng fold để vocabulary và median không học từ validation/test. Scorer dùng Macro F1 với danh sách nhãn cố định, `pos_label=None` và `zero_division=0`. DummyClassifier là baseline, đạt Accuracy 0,4710 và Macro F1 0,2135.

Decision Tree khảo sát 18 cấu hình × 3 fold với `max_depth=[3,6,None]`, `min_samples_leaf=[3,10]`, `ccp_alpha=[0,0.001,0.01]`; cấu hình tốt nhất có depth 3, 6 lá, alpha 0,01, CV Macro F1 0,626735.

Random Forest khảo sát 6 cấu hình × 3 fold với 200 cây, `max_depth=[6,None]`, `min_samples_leaf=[2,5,10]`. Cấu hình tốt nhất có `max_depth=None`, `min_samples_leaf=2`, CV Macro F1 0,626751. Theo quy tắc chọn CV cao nhất đã đặt trước, RF được chọn; chênh lệch với DT chỉ khoảng 0,000016 nên chưa thể hiện ưu thế ổn định. Không chọn mô hình theo điểm test.

### Bước 4. Đánh giá chung và kiểm tra overfitting

Sau khi chọn mô hình bằng CV, dùng test để báo cáo Accuracy, Macro Precision, Macro Recall, Macro F1, Weighted F1, confusion matrix và recall từng lớp. Báo cáo thêm điểm train, khoảng cách train–test và độ lệch chuẩn CV. CV Std mô tả biến động qua ba fold, không phải khoảng tin cậy.

RF đạt Test Macro F1 0,6000 nhưng gap train–test 0,2171; DT đạt 0,5758 với gap 0,0680. Vì vậy cần nêu RF còn dấu hiệu overfitting và không khẳng định RF vượt trội rõ ràng chỉ từ chênh lệch CV rất nhỏ.

### Bước 5. Khảo sát cắt tỉa và phân tích tin dự đoán sai

Ngoài lưới Decision Tree ở bước 3, notebook khảo sát riêng cây không giới hạn độ sâu bằng `cost_complexity_pruning_path` (94 giá trị alpha), chọn alpha bằng CV trên cùng ba fold; test chỉ dùng để báo cáo. Kết quả hiện tại: cây đầy đủ depth 33/248 lá, F1 train 0,958, CV 0,594, test 0,537; cây cắt tỉa alpha khoảng 0,0081, depth 3/6 lá, F1 train 0,644, CV 0,629, test 0,576. Đây là thí nghiệm khảo sát riêng, không thay mô hình đã chọn ở bước 3.

Notebook lấy tối đa năm lỗi từ kết quả hiện tại, ưu tiên sai lệch hai bậc, rồi đối chiếu JD để ghi nhận xét. `review_fingerprint` gắn nhận xét với dữ liệu, cấu hình mô hình, phiên bản sklearn, tập test và danh sách ứng viên; nếu dấu nhận dạng thay đổi thì nhận xét cũ không được tự áp dụng. Các nguyên nhân từ JD là giả thuyết, không phải kết luận nhân quả. Không chỉnh mô hình theo test này rồi dùng lại cùng test để tuyên bố đánh giá độc lập.

### Bước 6. Tổng hợp và xuất kết quả

Notebook lưu bảng, biểu đồ, cấu hình thí nghiệm, manifest chia tập và MiniNote dạng text dưới `output_salary_scope/` cạnh notebook. `experiment_config.json` ghi hash CSV, seed, phiên bản thư viện, điều kiện lọc và tham số; `requirements.txt` ghi các thư viện trực tiếp. File Markdown này là phần giải thích cho người đọc.

Khi chạy lại, chọn **Restart Kernel → Run All**. Cùng CSV và môi trường thì đối chiếu 688 mẫu, 550/138 train/test và các bảng. Nếu đổi dữ liệu hoặc phiên bản thư viện, kiểm tra lại hash, số mẫu, điểm số và các nhận xét lỗi trước khi cập nhật tài liệu.

## 4. Kết quả trên 138 tin test

### 4.1. Chỉ số so sánh

Các giá trị ở thang 0–1.

| Mô hình | Accuracy | Macro Precision | Macro Recall | Macro F1 | Weighted F1 |
|---|---:|---:|---:|---:|---:|
| Dummy | 0,4710 | 0,1570 | 0,3333 | 0,2135 | 0,3016 |
| DecisionTree | 0,5725 | 0,5713 | 0,6019 | 0,5758 | 0,5701 |
| RandomForest | 0,6014 | 0,5974 | 0,6280 | 0,6000 | 0,5975 |

| Mô hình | CV Macro F1 | CV Std | Train Macro F1 | Test Macro F1 | Train trừ Test |
|---|---:|---:|---:|---:|---:|
| Dummy | 0,2129 | 0,0004 | 0,2129 | 0,2135 | -0,0006 |
| DecisionTree | 0,6267 | 0,0230 | 0,6438 | 0,5758 | 0,0680 |
| RandomForest | 0,6268 | 0,0178 | 0,8171 | 0,6000 | 0,2171 |

RF đạt Test Macro F1 0,6000 nhưng Train Macro F1 0,8171, gap 0,2171 cho thấy còn nguy cơ overfitting. DT có gap 0,0680 nhỏ hơn. CV Std mô tả biến động qua ba fold, không phải khoảng tin cậy. Gap âm rất nhỏ của Dummy phản ánh chênh lệch phân bố lớp giữa train/test, không phải lỗi tính toán.

Không so điểm 0,6000 với điểm 0,6664 của bản trước để kết luận mô hình kém đi, vì phạm vi dữ liệu, các mẫu test và phép chia đã thay đổi. Lọc phạm vi nhằm đúng thiết kế bài toán, không nhằm bảo đảm tăng điểm.

### 4.2. Confusion matrix của Random Forest

Hàng là nhãn thật; cột là dự đoán.

| Nhãn thật / Dự đoán | Dưới 15 triệu | 15–30 triệu | Trên 30 triệu | Support |
|---|---:|---:|---:|---:|
| Dưới 15 triệu | 20 | 7 | 4 | 31 |
| 15–30 triệu | 18 | 31 | 16 | 65 |
| Trên 30 triệu | 3 | 7 | 32 | 42 |
| **Tổng** | **41** | **45** | **52** | **138** |

Mô hình đúng **83/138 tin**, sai **55 tin**. Có **7 lỗi hai bậc**: 4 tin dưới 15 triệu bị dự đoán trên 30 triệu và 3 tin theo chiều ngược lại. 48 lỗi còn lại giữa hai lớp liền kề. Khoảng cách này là khoảng cách nhãn, không phải sai số lương liên tục.

| Nhãn | Precision | Recall | F1 | Support |
|---|---:|---:|---:|---:|
| Dưới 15 triệu | 0,4878 | 0,6452 | 0,5556 | 31 |
| 15–30 triệu | 0,6889 | 0,4769 | 0,5636 | 65 |
| Trên 30 triệu | 0,6154 | 0,7619 | 0,6809 | 42 |

Nhóm 15–30 triệu có recall thấp nhất, đúng 31/65 tin; 18 tin bị dự đoán thấp hơn và 16 tin cao hơn. Nhóm dưới 15 triệu có precision thấp nhất: chỉ 20/41 dự đoán thuộc nhóm này đúng. Đây là các điểm cần lưu ý khi diễn giải thay vì chỉ nhìn Accuracy chung.

## 5. Phân tích năm tin sai thực tế

Các ví dụ sau được chọn sau khi chạy lại trên tập 688 tin. Nhận xét là giả thuyết từ JD và đặc trưng, không phải kết luận nhân quả. Tiêu đề/JD được đọc sau dự đoán, không đưa vào X. Điểm xác suất dùng để sắp xếp chưa được hiệu chỉnh thành độ tin cậy.

### 5.1. CG Phát Triển Giải Pháp CNTT - ID9280

- ID: `vietnamworks-7fe0717a04`.
- Trung điểm khoảng lương: **12.5 triệu VND/tháng**.
- Nhãn thật: `duoi_15`; dự đoán: `tren_30`.
- Kỹ năng: `[".net", "agile", "aws", "c#", "ci/cd", "devops", "docker", "gitlab", "java", "jira", "kafka", "microservices", "rest api", "scrum", "spring boot"]`.
- Kinh nghiệm tối thiểu/tối đa: 10.0 / thiếu năm.

Quan sát: trung điểm lương trong CSV là 12,5 triệu, nhưng JD yêu cầu tối thiểu 10 năm CNTT, ít nhất 3 năm technical lead/kiến trúc và nhiều kỹ năng backend/cloud. Dự đoán nhóm cao có thể liên quan đến các đặc trưng này. Cần kiểm tra lại khoảng lương và nguồn quy đổi vì chúng có vẻ chưa phù hợp với mô tả vai trò; chưa đủ bằng chứng để kết luận nhãn sai và không tự sửa nhãn theo mô hình.

### 5.2. FRESHER Transmission Engineer Bitel (Viettel PERU)

- ID: `topcv-0114f2dcbe`.
- Trung điểm khoảng lương: **35 triệu VND/tháng**.
- Nhãn thật: `tren_30`; dự đoán: `duoi_15`.
- Kỹ năng: `["autocad", "cisco", "excel", "firewall", "visio"]`.
- Kinh nghiệm tối thiểu/tối đa: 0.0 / thiếu năm.

Quan sát: tin ghi Fresher, không cần kinh nghiệm, exp_min=0; JD nêu làm tại Peru và tổng thu nhập khoảng 1.200–1.500 USD/tháng. Giả thuyết: mô hình chỉ có skills/experience nên chưa thể hiện bối cảnh làm việc nước ngoài và thành phần thu nhập. Cần rà phạm vi thị trường Việt Nam và quy đổi lương ở bước đầu nguồn. Chưa tự loại tin chỉ vì dự đoán sai; mọi quy tắc địa lý mới phải áp dụng chung rồi đánh giá lại.

### 5.3. Kỹ Sư An Toàn Thông Tin

- ID: `topcv-033a519fc3`.
- Trung điểm khoảng lương: **32.5 triệu VND/tháng**.
- Nhãn thật: `tren_30`; dự đoán: `duoi_15`.
- Kỹ năng: `["cisco", "cybersecurity", "linux", "windows server"]`.
- Kinh nghiệm tối thiểu/tối đa: 1.0 / 1.0 năm.

Quan sát: trung điểm lương 32,5 triệu, kinh nghiệm trong dữ liệu là 1/1 năm; skills hiện có Cisco, Cybersecurity, Linux, Windows Server. JD có thêm CCNA, CompTIA Security+, pentest, vulnerability assessment, mã hóa và quản lý truy cập. Giả thuyết: bộ trích kỹ năng chưa phản ánh đầy đủ yêu cầu an toàn thông tin. Cần rà extractor trên dữ liệu phát triển; không khẳng định đây là nguyên nhân duy nhất.

### 5.4. Research & Teaching Assistants, Computer Science Study Program (2 Openings)

- ID: `vietnamworks-03cddaef4c`.
- Trung điểm khoảng lương: **40.5 triệu VND/tháng**.
- Nhãn thật: `tren_30`; dự đoán: `duoi_15`.
- Kỹ năng: `["agile", "c#", "c++", "computer vision", "deep learning", "machine learning", "python", "pytorch"]`.
- Kinh nghiệm tối thiểu/tối đa: thiếu / thiếu năm.

Quan sát: trung điểm lương 40,5 triệu, thiếu cả hai trường kinh nghiệm; JD yêu cầu bằng thạc sĩ, tiếng Anh IELTS, nghiên cứu/giảng dạy về deep learning và computer vision. X chỉ chứa kỹ năng công nghệ và kinh nghiệm nên chưa phản ánh hết trình độ học thuật và nhiệm vụ nghiên cứu. Đây là giới hạn biểu diễn đầu vào; nếu mở rộng đặc trưng cần phù hợp yêu cầu đề và một đánh giá độc lập.

### 5.5. NHÂN VIÊN TRIỂN KHAI BẢO TRÌ PHẦN MỀM

- ID: `topdev-13fa7c7f57`.
- Trung điểm khoảng lương: **6.5 triệu VND/tháng**.
- Nhãn thật: `duoi_15`; dự đoán: `tren_30`.
- Kỹ năng: `[]`.
- Kinh nghiệm tối thiểu/tối đa: thiếu / thiếu năm.

Quan sát: skills rỗng và cả hai trường kinh nghiệm đều thiếu; JD ghi lương 6–7 triệu cộng thưởng, phù hợp trung điểm 6,5 triệu trong CSV. Tin mô tả triển khai phần mềm, hỗ trợ người dùng, nghiệp vụ và giao tiếp. Giả thuyết: đầu vào quá ít thông tin sau trích xuất và điền median nên mô hình phân biệt kém. Cần rà kỹ năng hỗ trợ/triển khai và bổ sung cờ thiếu kỹ năng nếu được đánh giá bằng CV; không đọc mức lương trong JD để làm X.


Không điều chỉnh nhãn, extractor hoặc tham số theo riêng các lỗi test rồi báo cùng test như một đánh giá độc lập mới. Mọi thay đổi tiếp theo cần quy tắc chung và một tập đánh giá độc lập mới hoặc nested CV phù hợp.

## 6. Đối chiếu các nhận xét mới

| Nhận xét cần xử lý | Nội dung đã bổ sung |
|---|---|
| Chưa lọc phạm vi IT | Thêm scope vào required; giữ included_provisional; xuất bảng lọc 761 → 688 và tạm loại 73 tin |
| Scorer tương thích | Thêm pos_label=None; giữ nhãn, Macro F1 và zero_division như cũ |
| Ví dụ lỗi gắn ID cũ | Chọn năm tin sai thực tế mới, đọc JD, lưu nhận xét và kiểm tra fingerprint |
| Số liệu còn theo 761 tin | Chạy lại tám cell, cập nhật train/test, CV, bảng, ma trận, hình và cấu hình |
| Tên mục tiêu chưa rõ | Phân nhóm trung điểm khoảng lương chào tuyển, không gọi là lương thực nhận |
| Môi trường tái lập | Có requirements.txt, cấu hình và manifest chia tập |

Năm tiêu chí ban đầu vẫn được thể hiện: đúng đầu vào kỹ năng/kinh nghiệm; DT có cắt tỉa và đối sánh; đủ chỉ số và ma trận; kiểm tra nhóm/pipeline và overfitting; phân tích lỗi cụ thể. Việc lọc ngành sơ bộ không đồng nghĩa đã bảo đảm phạm vi địa lý hoặc loại mọi rò rỉ.

## 7. Hạn chế và hướng phát triển

1. **Phạm vi ngành và địa lý:** included_provisional vẫn là chấp nhận sơ bộ. Ví dụ Fresher tại Peru cho thấy phạm vi ngành khác phạm vi địa lý; cần chốt cách hiểu thị trường Việt Nam trước thí nghiệm tiếp theo và áp dụng quy tắc địa lý nhất quán.
2. **Thiên lệch công khai lương:** chỉ 688/3.135 tin vào mô hình; chưa suy rộng cho tin thương lượng, thiếu lương hoặc chờ rà soát.
3. **Định nghĩa lương:** trung điểm khoảng chào tuyển khác lương thực nhận; ngoại tệ, kỳ lương, phụ cấp và tổng thu nhập có thể ảnh hưởng nhãn.
4. **Chất lượng trích xuất:** kỹ năng rỗng và kinh nghiệm thiếu có thể làm mất tín hiệu. Không mặc định thiếu là 0 năm hoặc tự sửa nhãn theo mô hình.
5. **Tổng quát hóa:** RF còn gap 0,2171; CV gần bằng DT; một test 138 tin và ba fold chưa đủ để khẳng định ưu thế ổn định. Chưa kiểm soát hết tin gần trùng hoặc thay đổi phân phối theo nguồn/thời gian.
6. **Đánh giá tiếp:** không tối ưu lặp theo test. Nếu sửa dữ liệu/phạm vi, cần ghi phiên bản và dùng thiết kế đánh giá mới. Không thêm cột lương hoặc văn bản có lương vào X để tăng điểm.

## 8. File đầu ra, môi trường và chạy lại

### 8.1. Thành phần bàn giao

- `Classification_HoanChinh.ipynb`: notebook gồm tám cell code, gồm thí nghiệm cắt tỉa bổ sung ở mục 5b; kết quả được kiểm tra lại bằng Run All.
- `MiniNote_2.5.md`: báo cáo theo tập 688 tin.
- `requirements.txt`: phiên bản thư viện của lần chạy đối chiếu.
- `output_salary_scope/`: bảng, hình, MiniNote tự xuất và cấu hình.
- `../buoc2.1_2.2/data/processed/clean_data_processed.csv`: CSV đầu vào chuẩn nằm ở gốc repo; không sử dụng bản CSV lồng trong Portable.

Các bảng quan trọng: `scope_summary.csv`, `filter_summary.csv`, `class_distribution.csv`, `split_manifest.csv`, `fold_distribution.csv`, `cv_*.csv`, `model_comparison.csv`, `classification_report_*.csv`, `confusion_matrix.csv`, `recall_by_class.csv`, `top_confusion_pairs.csv`, `pruned_tree_structure.csv`, `test_predictions.csv`, `misclassified_jobs.csv`, `review_candidates.csv`, `reviewed_errors.csv`.

Các hình: `confusion_counts.png`, `confusion_normalized.png`, `recall_by_class.png`, `pruned_tree_top_levels.png`. Cấu hình là `experiment_config.json`.

### 8.2. Môi trường đối chiếu

Python **3.14.0**, NumPy **2.5.5**, pandas **3.1.1**, matplotlib **3.11.2**, scikit-learn **1.9.1**, IPython **9.18.1**, seed **42**. `requirements.txt` chốt các thư viện trực tiếp; thành phần chạy notebook như Jupyter/ipykernel cần được cài trong môi trường sử dụng. File này không phải bản khóa toàn bộ hệ điều hành và phụ thuộc gián tiếp.

SHA-256 của CSV: `8b931fe7c90b7ee89b0bdf342d7d1a8bfb5b40a95aa0e617d6ddd5f820d2790f`.

### 8.3. Quy ước đường dẫn và cách chạy chung

Nhóm chốt duy nhất bản `buoc 2.5_Portable/Classification_HoanChinh.ipynb` trong repository `vietnam-it-job-market-analysis`. Không cố định đường dẫn ổ đĩa cá nhân trong code; notebook tìm gốc repo dựa trên thư mục Portable và CSV đầu vào. Các đường dẫn tuyệt đối đang hiện trong output lưu kèm notebook có thể là snapshot từ lần chạy trước, không phải đường dẫn hardcode. Sau khi chuyển máy hoặc folder, chạy **Restart Kernel → Run All** để làm mới đường dẫn và kết quả hiển thị.

```text
thu_muc_repo/
├── buoc2.1_2.2/data/processed/clean_data_processed.csv
└── buoc 2.5_Portable/
    ├── Classification_HoanChinh.ipynb
    ├── MiniNote_2.5.md
    ├── requirements.txt
    └── output_salary_scope/
```

1. Mở gốc repo hoặc thư mục Portable trong VS Code/Jupyter, cài `python -m pip install -r "buoc 2.5_Portable/requirements.txt" ipykernel` khi đứng ở gốc repo. Chọn đúng kernel.
2. Giữ `PROJECT_ROOT_OVERRIDE=None`. Code tìm gốc có cả thư mục Portable và CSV chuẩn; không chọn bản CSV khác chỉ vì nằm gần kernel hơn.
3. Giữ tên `Classification_HoanChinh.ipynb` khớp `NOTEBOOK_NAME`. Khi đổi tên file phải cập nhật biến tương ứng. Không chạy các notebook lưu trữ khác.
4. Cell đầu phải in đúng CSV từ gốc repo và nơi xuất `buoc 2.5_Portable/output_salary_scope`. Chạy từ gốc repo hay Portable đều cho cùng hai đường dẫn này.
5. Chọn Restart Kernel → Run All. Với cùng CSV và môi trường, đối chiếu 688 tin, 550/138 train/test và các bảng. Nếu khác, kiểm tra hash/thư viện trước khi kết luận code sai hoặc thay số trong báo cáo.
6. Mọi CSV/PNG/JSON mới chỉ nằm trong output_salary_scope. Notebook không tạo thư mục đầu ra thứ hai theo cwd. Lần chạy lại cập nhật các file cùng tên tại đó.
7. Khi review_fingerprint đổi, đọc lại review_candidates.csv rồi cập nhật nhận xét; không ép dùng năm ID cũ.

Các nhận xét để vấn đáp: RF còn overfitting; CV của RF và DT gần như bằng nhau; chọn RF là theo quy tắc CV cao nhất, không chứng minh vượt trội. Tên mục tiêu là phân nhóm trung điểm khoảng lương chào tuyển. Phạm vi ngành sơ bộ chưa bảo đảm phạm vi địa lý, như ví dụ làm tại Peru.

## 9. Kết luận

Sau khi bổ sung scope_status, thí nghiệm sử dụng 688 tin thuộc phạm vi IT sơ bộ và đủ điều kiện lương. RF được chọn theo CV và đạt Test Macro F1 **0,6000**, Accuracy **60,14%**. DT có cắt tỉa đạt Test Macro F1 **0,5758** với gap nhỏ hơn RF; điểm CV của hai mô hình gần như tương đương.

Kết quả cho thấy kỹ năng và kinh nghiệm có tín hiệu phân biệt nhóm trung điểm khoảng lương chào tuyển trong tập hiện tại. Nhóm cần giữ rõ các giới hạn về phạm vi sơ bộ, địa lý, chất lượng lương và đặc trưng thiếu khi trình bày kết quả.
