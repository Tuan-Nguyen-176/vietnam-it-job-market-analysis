# NỘI DUNG SLIDE — CHƯƠNG 5. PHÂN LỚP VÀ DỰ BÁO

**Phụ trách** · Quân  
**Phạm vi** · 9 slide, đánh số 15–23 theo vị trí trong phần trình bày của nhóm. Kết luận chung trong sườn là slide 24 nếu đánh số liên tục.  

Nội dung từ slide 15 đến 23 có thể dán trực tiếp lên slide. Dùng bảng map ở cuối file để đặt hình và đối chiếu nguồn.

## Map tổng thể

| Slide mới | Sườn nhóm | Báo cáo | Notebook | Hình hoặc bảng chính |
|---|---|---|---|---|
| 15 · Bài toán và chọn mẫu | 15 | 5.1 | Bước 1 | Sơ đồ lọc |
| 16 · Phân bố ba nhóm lương | 15 | 5.1 | Bước 1 | Ba khối số |
| 17 · Đặc trưng đầu vào | 15 | 5.2 | Bước 1–3 | Sơ đồ đầu vào và đầu ra |
| 18 · Chia tập theo nhóm | 15 | 5.2 | Bước 2–3 | Sơ đồ train, test và CV |
| 19 · Cắt tỉa Decision Tree | 16 | 5.3 | Bước 5b | Đường cong cắt tỉa |
| 20 · So sánh ba mô hình | 16 | 5.4 | Bước 3–4 | Bảng kết quả |
| 21 · Đọc một nhánh của cây | 17 | 5.3 | Bước 5 | Hình cây quyết định |
| 22 · Ma trận nhầm lẫn | 17 | 5.5 | Bước 4–5 | Ma trận theo số lượng |
| 23 · Một tin dự đoán sai | 17 | 5.6 | Bước 5–6 | Ba khối thông tin về tin đã rà |

---

## Slide 15 — Bài toán và chọn mẫu

Dự đoán **nhóm trung điểm khoảng lương chào tuyển** từ kỹ năng và kinh nghiệm.

**3.135 tin đã xử lý → 761 tin đủ điều kiện lương → 688 tin trong phạm vi IT sơ bộ**

*73 tin chờ rà soát phạm vi được tạm loại.*

---

## Slide 16 — Ba nhóm lương phân bố thế nào?

| Dưới 15 triệu | Từ 15 đến 30 triệu | Trên 30 triệu |
|---|---|---|
| **157 tin** | **323 tin** | **208 tin** |
| 22,82% | 46,95% | 30,23% |

*Đơn vị triệu VNĐ/tháng. Nhóm giữa gồm cả hai mốc.*

---

## Slide 17 — Mô hình học từ thông tin nào?

| Kỹ năng | Kinh nghiệm |
|---|---|
| Mỗi kỹ năng được mã hóa 0/1 | Số năm tối thiểu và tối đa |

**Đầu ra** · Một trong ba nhóm lương.

---

## Slide 18 — Chia tập để hạn chế rò rỉ

**Gom tin liên quan → 550 tin train / 138 tin test → CV 3 fold trên train**

- Tin cùng mã nhóm hoặc cùng mô tả chuẩn hóa được giữ cùng nhóm.
- Pipeline học bộ từ vựng kỹ năng và trung vị kinh nghiệm trên phần huấn luyện của mỗi fold.

---

## Slide 19 — Cắt tỉa thay đổi cây thế nào?

- **248 → 6 lá**
- **0,5368 → 0,5758** Macro F1 test

*Khảo sát riêng 94 giá trị alpha. CV chọn `ccp_alpha ≈ 0,00813`.*

---

## Slide 20 — So sánh ba mô hình

| Mô hình | Macro F1 CV | Macro F1 test | Chênh lệch train–test |
|---|---:|---:|---:|
| Dummy | 0,212872 | 0,2135 | ≈ 0 |
| Decision Tree | 0,626735 | 0,5758 | 0,0680 |
| Random Forest | 0,626751 | 0,6000 | 0,2171 |

**Random Forest hơn Decision Tree khoảng 0,000016 điểm Macro F1 CV và có chênh lệch train–test lớn hơn.**

---

## Slide 21 — Đọc một nhánh của Decision Tree

**177 tin train đi đến lá dự đoán nhóm 15–30 triệu.**

*Cây từ lưới tham số có `ccp_alpha = 0,01`, sâu 3 và có 6 lá.*

---

## Slide 22 — Random Forest nhầm nhiều nhất ở nhóm nào?

**Nhóm 15–30 triệu có Recall thấp nhất · 47,7%**

**31/65 tin** thuộc nhóm này được nhận diện đúng.

*Hàng là nhãn thật. Cột là nhãn dự đoán.*

---

## Slide 23 — Đọc lại một tin dự đoán sai

*Fresher Transmission Engineer*

| Trung điểm ghi nhận | Dự đoán | Bối cảnh trong JD |
|---|---|---|
| **35 triệu** | **Dưới 15 triệu** | Làm việc ở Peru, thu nhập ghi bằng USD |

---

## Map bố cục

| Slide | Vị trí nội dung và nguồn |
|---|---|
| 15 | Đặt mục tiêu bên trên. Ở giữa, trình bày ba số theo hàng ngang. Đặt 73 tin ở nhánh rẽ ngay trước mốc 688. Nguồn số liệu ở mục 5.1 và `filter_summary.csv`. |
| 16 | Dựng ba khối ngang, mỗi khối có tên lớp, số tin lớn và tỷ lệ nhỏ. Dùng màu nhấn cho lớp giữa. Nguồn số liệu ở mục 5.1 và `class_distribution.csv`. |
| 17 | Đặt hai khối đầu vào cạnh nhau và nối xuống một khối đầu ra. Nguồn ở mục 5.2 và notebook bước 1–3. |
| 18 | Vẽ nhánh từ 550 tin sang CV. Đặt 138 tin ở nhánh riêng và ghi “test”. Nguồn ở mục 5.2, `split_manifest.csv` và `experiment_config.json`. |
| 19 | Cho `pruning_curve.png` chiếm khoảng hai phần ba slide, hai số nổi bật đặt bên phải. Đường đứt đoạn trong hình đánh dấu alpha được chọn, hiển thị làm tròn thành 0,0081. Nguồn ở mục 5.3, notebook bước 5b và `overfitting_full_vs_pruned.csv`. |
| 20 | Bảng chiếm phần chính, làm nổi hai điểm CV của Decision Tree và Random Forest. Giữ sáu chữ số thập phân ở cột CV. Nguồn ở mục 5.4 và `model_comparison.csv`. |
| 21 | Dùng `pruned_tree_top_levels.png` gần hết chiều ngang. Đánh dấu đường đi từ nút gốc qua nhánh có cận trên kinh nghiệm đến lá 177 tin. Đặt chú thích alpha và số lá dưới hình. Nguồn ở Hình 5.1, mục 5.3 và notebook bước 5. |
| 22 | Đặt `confusion_counts.png` cỡ lớn, khoanh hàng giữa và đặt hai con số bên cạnh. Nguồn ở mục 5.5, `confusion_matrix.csv` và `per_class_metrics.csv`. |
| 23 | Trình bày ba khối ngang. Dùng màu nhấn cho hai kết quả lệch nhau. Nguồn ở mục 5.6 và `reviewed_errors.csv`, mã tin `topcv-0114f2dcbe`. |

## Map hình

| Tệp hình | Vị trí | Cách dùng |
|---|---|---|
| `pruning_curve.png` | Slide 19 | Chỉ đường validation và vạch alpha được chọn |
| `pruned_tree_top_levels.png` | Slide 21 | Tô một đường đi từ gốc đến lá |
| `confusion_counts.png` | Slide 22 | Khoanh hàng 15–30 triệu |
| `confusion_normalized.png` | Vấn đáp D4 | Đọc tỷ lệ theo từng lớp thật |
| `recall_by_class.png` | Vấn đáp D4 | So sánh Recall 64,5% / 47,7% / 76,2% |

[Mở thư mục hình](<D:/Projects/baithuhanhlon_datamining/bản gốc/vietnam-it-job-market-analysis-git/buoc 2.5_Portable/output_salary_scope/figures>)
