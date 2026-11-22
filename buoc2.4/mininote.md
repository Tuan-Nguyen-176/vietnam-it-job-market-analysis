
# 📝 MININOTE CHI TIẾT: KHAI PHÁ LUẬT KẾT HỢP KỸ NĂNG IT

---

## 1. BÀI TOÁN & Ý TƯỞNG

**Câu hỏi:** Trong tin tuyển dụng IT, những kỹ năng nào thường được yêu cầu **cùng nhau**, và gắn kết mạnh đến mức nào?

**Cách mô hình hóa (giống phân tích giỏ hàng siêu thị):**

 Bài toán này |
|---|
| Một tin tuyển dụng |
| Một kỹ năng (python, sql, docker...) |
| "Yêu cầu Kubernetes thì hay yêu cầu Docker" |

**Dòng chảy dữ liệu:**
`3.135 tin → 2.843 tin (phạm vi IT sơ bộ + có JD sạch) → 2.430 giỏ kỹ năng → ma trận 2.430×121 → 856 tập phổ biến → 4.018 luật thô → 1.228 luật qua lọc → 1.027 luật sau khi loại dư thừa → top luật & biểu đồ`

---

## 2. KHÁI NIỆM NỀN TẢNG

| Khái niệm | Công thức / Định nghĩa | Cách hiểu |
|---|---|---|
| **Itemset** | Một nhóm kỹ năng, vd `{docker, kubernetes}` | "Một món combo" |
| **Support(X)** | (số tin chứa X) ÷ (tổng số tin) | Combo này phổ biến cỡ nào |
| **Tập phổ biến** | Itemset có support ≥ `min_support` | Combo đủ phổ biến để đáng xét |
| **Luật A ⇒ B** | Antecedent (vế trái) ⇒ Consequent (vế phải) | "Có A thì thường có B" |
| **Confidence** | Support(A∪B) ÷ Support(A) = P(B\|A) | Trong các tin có A, bao nhiêu % có B |
| **Lift** | Confidence ÷ Support(B) | So với ngẫu nhiên, A kéo B lên bao nhiêu lần |
| **Conviction** | (1 − Support(B)) ÷ (1 − Confidence) | Luật "sai" ít hơn đoán ngẫu nhiên bao nhiêu lần |

**Cách đọc Lift:**
- **Lift > 1** → A và B kéo nhau lên (tương quan dương)
- **Lift = 1** → độc lập
- **Lift < 1** → A và B ít đi cùng nhau hơn kỳ vọng (tương quan âm)

**Conviction:** càng cao càng đáng tin; **= ∞ khi Confidence = 100%** (luật không có ngoại lệ trong mẫu).

> ⚠️ **Confidence có tính bất đối xứng:** conf(A⇒B) ≠ conf(B⇒A), nhưng **Lift thì đối xứng** (A⇒B và B⇒A có cùng lift).

---

## 3. CHI TIẾT TỪNG BƯỚC

### Bước 2.4.1: Nạp dữ liệu và chốt tập tin
- **Làm gì:** Đọc `clean_data_processed.csv` (kết quả bước 2.1–2.2), có kiểm tra file tồn tại, rồi lọc `scope_status == "included_provisional"` và `has_jd_clean == True`, **cùng bộ lọc với 2.2 và 2.3**.
- **Vì sao:** Đầu vào sạch quyết định chất lượng cả bài; ba bước phải chung một mẫu số. 266 tin `pending_review` (phần lớn PM/PO, Scrum Master, BA) chưa xác nhận là IT. Cột `skills` gộp kỹ năng từ **tiêu đề, thẻ và JD**, nên tin không có JD vẫn có kỹ năng (ví dụ tin ITviec "DevSecOps Expert (Kubernetes, AWS, Terraform)" có `jd_chars = 0` mà vẫn mang 4 kỹ năng).
- **Kết quả:** 3.135 tin, 47 cột → 2.869 tin trong phạm vi → **2.843 tin** vừa trong phạm vi vừa có JD sạch.
- **Nối sang bước sau:** 47 cột quá nhiều, chỉ cột dạng danh sách mới dùng được → chọn thuộc tính.

### Bước 2.4.2: Chọn thuộc tính khai phá
- **Làm gì:** Khảo sát 8 cột (`title`, `tech_list`, `keywords_list`, `skills`, `broad_concepts`, `application_domains`, `languages`, `benefits`), in 10 dòng đầu.
- **Vì sao:** Luật kết hợp cần dữ liệu dạng "tập mục". Trộn cột khác loại (vd `benefits`) sẽ ra luật khó diễn giải.
- **Kết luận:** Dùng cột **`skills`** (sạch, đúng nghĩa "kỹ năng yêu cầu"). Bước này chỉ để khảo sát, không đổi `df`.
- **Nối sang bước sau:** `skills` còn lẫn kỹ năng quá hiếm → làm sạch.

### Bước 2.4.3: Giảm bớt thuộc tính
- **Làm gì:**
  1. `to_list`: đọc chuỗi JSON thành list (lỗi → list rỗng)
  2. Chuẩn hóa `strip().lower()`, dùng `set` để bỏ trùng trong một tin
  3. Giữ kỹ năng xuất hiện **≥ 30 lần** (`MIN_COUNT = 30`)
  4. Bỏ tin còn **< 2 kỹ năng** (không tạo được luật A ⇒ B)
- **Vì sao:** Kỹ năng hiếm → luật thiếu tin cậy; quá nhiều mục → số tổ hợp bùng nổ.
- **Kết quả:** 157 → **121 kỹ năng**; **2.430 transaction** (từ 2.843 tin, bỏ 413 tin có < 2 kỹ năng). Giỏ theo nguồn: TopCV 894, LinkedIn 591, ITviec 496, VietnamWorks 396, TopDev 53.
- **Đánh đổi:** Ngưỡng thấp → giữ được kỹ năng ngách nhưng nhiễu; cao → chắc nhưng mất chuyên biệt.
- **Nối sang bước sau:** Chuyển danh sách thành ma trận để thuật toán đọc được.

### Bước 2.4.4: One-hot encoding
- **Làm gì:** `TransactionEncoder` → ma trận hàng = tin, cột = kỹ năng, ô = True/False.
- **Vì sao:** `mlxtend` chỉ nhận bảng boolean.
- **Kết quả:** Ma trận **2.430 × 121**, rất thưa (đa số False).
- **Top kỹ năng (support):** python 36,8% · sql 32,2% · ci/cd 29,3% · docker 22,1% · aws 21,8% · java ≈ agile ≈ 20,9% · git 20,2% · linux 20,0% · devops 19,3%.
- **Ý nghĩa:** Thị trường cần cả lập trình (Python/Java/SQL), quy trình (Agile/CI-CD) và hạ tầng (Docker/AWS/Linux).
- **Nối sang bước sau:** Tìm tổ hợp kỹ năng phổ biến.

### Bước 2.4.5: Apriori
- **Làm gì:** `apriori(min_support=0.03, max_len=4)`.
- **Vì sao chọn tham số:** 0,03 ≈ 77 tin: đủ tin cậy nhưng vẫn giữ cụm chuyên biệt. `max_len=4` vì luật quá dài khó đọc, ít ứng dụng.
- **Nguyên lý:** Nếu một tập không phổ biến thì mọi tập chứa nó cũng không phổ biến → cắt tỉa sớm.
- **Kết quả:** **856 itemset**: 79 (1 kỹ năng) · 351 (2) · 331 (3) · 95 (4). Thời gian 0,102s.
- **Nhận xét:** Số tập đạt đỉnh ở kích thước 2 rồi giảm: tổ hợp càng dài càng hiếm.
- **Nối sang bước sau:** Kiểm chứng bằng thuật toán khác.

### Bước 2.4.6: FP-Growth
- **Làm gì:** `fpgrowth` cùng tham số, so sánh với Apriori.
- **Khác biệt:** Apriori sinh ứng viên rồi đếm lại nhiều lần; FP-Growth nén dữ liệu vào cây FP-tree, không sinh ứng viên.
- **Kết quả:** Cùng **856 itemset** (`True`). Apriori 0,102s, FP-Growth 0,279s (lần chạy 2026-11-22; phụ thuộc máy).
- **Vì sao Apriori có thể nhanh hơn?** Dữ liệu nhỏ + `max_len=4`; FP-Growth mất chi phí dựng cây. FP-Growth chỉ vượt trội khi dữ liệu lớn/support thấp. Thời gian cỡ phần trăm giây dễ dao động, đừng kết luận quá mạnh.
- **Nối sang bước sau:** Từ tập phổ biến sinh luật có hướng.

### Bước 2.4.7: Sinh luật
- **Làm gì:** `association_rules(metric='confidence', min_threshold=0.0)`, giữ **tất cả** để tự lọc sau. `try/except` xử lý khác biệt phiên bản mlxtend (`num_itemsets`).
- **Kết quả:** **4.018 luật thô**.
- **Ví dụ:** `sql ⇒ python` lift 1,14 (yếu) · `sql ⇒ devops` lift 0,83 (ngược chiều) · `linux ⇒ devops` lift 1,56 (khá).
- **Nối sang bước sau:** Nhiều luật yếu/ngược chiều → lọc.

### Bước 2.4.8: Lọc luật
- **Tiêu chí:** support ≥ 0,03 · confidence ≥ 0,5 · lift > 1,2 · conviction ≥ 1,2.
- **Vì sao nhiều tiêu chí:** Confidence cao chưa đủ (nếu B quá phổ biến thì A ⇒ B luôn đúng).
- **Kết quả:** **1.228 / 4.018 luật (~31%)**.
- **Lưu ý:** `python ⇒ sql` (lift 1,14) bị loại ở đây, nhưng bước 2.4.10 tra trong luật thô nên vẫn thấy.
- **Nối sang bước sau:** Còn hơn 1.200 luật, nhiều luật chỉ là bản dài hơn của luật khác → loại luật dư thừa.

### Bước 2.4.8b: Loại luật dư thừa
- **Vì sao:** Đề (Chương 4) yêu cầu "loại bỏ luật dư thừa".
- **Định nghĩa:** `A ⇒ C` dư thừa nếu có luật ngắn hơn `A' ⇒ C` đã qua lọc với **cùng vế phải**, `A'` là **tập con thật sự** của `A`, và `confidence(A' ⇒ C) ≥ confidence(A ⇒ C)`. Thêm kỹ năng vào vế trái mà confidence không tăng thì luật chỉ hẹp đi.
- **Ví dụ (đếm trên 2.430 giỏ):** `spring ⇒ java` 117/117 = 100% (giữ) → `{spring, spring boot} ⇒ java` 83/83 = 100% là **dư thừa** (cùng 100% nhưng chỉ áp dụng cho 83 tin thay vì 117). `{ci/cd, spring boot} ⇒ java` 83/84 = 98,8% là **dư thừa** vì thấp hơn `spring boot ⇒ java` 168/169 = 99,41%. `{rest api, spring boot} ⇒ java` 109/109 = 100% **được giữ** vì confidence tăng so với 99,41%.
- **Kết quả:** 1.228 luật → **201 dư thừa** (198 vế phải 1 kỹ năng, 3 vế phải 2 kỹ năng) → còn **1.027 luật** (`rules_nr`). Nếu không loại, 8/15 dòng đầu bảng Top Confidence là luật dư thừa. Lần chạy trước (mẫu chưa lọc phạm vi): 168/1.070.
- **Nối sang bước sau:** Xếp hạng trên 1.027 luật còn lại.

### Bước 2.4.9: Xếp hạng top luật
- **Theo Lift** (gắn kết bất ngờ): `android ⇄ ios` ≈ **16,0**; cụm web front-end (`html, css, javascript, git, rest api`) lift 13–15; `{html, react} ⇒ {css}` conf 97,7%.
- **Theo Confidence** (chắc chắn): 5 luật **100%**: `spring ⇒ java` (117/117), `{rest api, spring boot} ⇒ java` (109/109), `{spring boot, sql} ⇒ java` (75/75), `fastapi ⇒ python` (79/79), `{rest api, scrum} ⇒ agile` (91/91). **`spring boot ⇒ java` chỉ 168/169 = 99,41%**, không phải 100%.
- **Nhận xét:** Lift cho thấy *cụm kỹ năng ngành*; Confidence thường cho thấy *quan hệ kỹ thuật hiển nhiên* (framework thuộc ngôn ngữ).
- **Nối sang bước sau:** Tra các cặp cụ thể.

### Bước 2.4.10: Luật theo chủ đề

| Luật | Support | Confidence | Lift | Đọc kết quả |
|---|:-:|:-:|:-:|---|
| docker ⇒ kubernetes | 13,8% | 62,3% | 3,33 | Có Docker thì ~62% cần K8s |
| kubernetes ⇒ docker | 13,8% | **73,6%** | 3,33 | Chiều ngược mạnh hơn |
| docker, kubernetes ⇒ aws | 6,9% | 49,9% | 2,29 | Nhóm container hay kèm AWS |
| python ⇒ sql | 13,5% | 36,8% | **1,14** | Liên kết yếu |
| machine learning ⇒ python | 5,3% | **80,0%** | 2,17 | Python gần như bắt buộc với ML |
| spring ⇒ java | 4,8% | **100%** (117/117) | 4,77 | Tuyệt đối trong mẫu |
| spring boot ⇒ java | 6,9% | **99,41%** (168/169) | 4,75 | Gần tuyệt đối, có 1 ngoại lệ |
| python, sql ⇒ machine learning | – | – | – | **Không có luật** (support < 0,03) |

- **Bất đối xứng:** Docker là kỹ năng nền, K8s là nâng cao → ai biết K8s hầu như biết Docker, ngược lại thì không.
- **Python + SQL yếu:** cùng phổ biến nên hay trùng nhau, không phải gắn kết đặc biệt.
- **Nối sang bước sau:** Kiểm tra luật "trông mạnh nhưng vô nghĩa".

### Bước 2.4.11: Luật gây nhiễu
- **Kiểm tra 1:** conf ≥ 0,6 mà lift < 1,5 → **0 luật**. Giải thích: lift = conf ÷ support(B), nên để lift < 1,5 với conf ≥ 0,6 thì support(B) phải > 0,4; nhưng kỹ năng phổ biến nhất (Python) chỉ 36,8% → loại "bẫy" này không thể xảy ra ở dữ liệu này. (Lưu ý: đoạn code chạy trên `rules` thô, không phải `rules_f`.)
- **Kiểm tra 2:** **8 luật conviction = ∞**: `spring ⇒ java`, `rest api, spring ⇒ java`, `spring, spring boot ⇒ java`, `rest api, spring boot ⇒ java`, `spring boot, sql ⇒ java`, `microservices, rest api, spring boot ⇒ java`, `rest api, scrum ⇒ agile`, `fastapi ⇒ python`. **3 trong 8 là luật dư thừa** (`rest api, spring`; `spring, spring boot`; `microservices, rest api, spring boot`), còn 5. `spring boot ⇒ java` không nằm trong nhóm này vì 168/169 (conviction ≈ 133,6). Phần lớn là quan hệ "thuộc về"; `scrum ⇒ agile` hợp lý vì Scrum là một phương pháp Agile. Conviction ∞ chỉ nói *không có ngoại lệ trong mẫu*, chưa phải quy luật tuyệt đối.
- **Kiểm tra 3:** `ai`, `cloud` **không có** trong ma trận vì bước 2.1 đã tách chúng khỏi `skills` sang `broad_concepts` (Fintech, E-commerce sang `application_domains`) → luôn kiểm tra `onehot.columns` trước khi tra một chủ đề.
- **Nối sang bước sau:** Vẽ biểu đồ để nhìn tổng thể.

### Bước 2.4.12: Trực quan hóa
- **Scatter (Support × Confidence, màu = Lift)**, vẽ trên 1.027 luật đã loại dư thừa: mỗi chấm một luật; luật lý tưởng ở góc trên phải. 90,8% luật có support dưới 6%; 62 luật lift ≥ 8 đều là tổ hợp ngách.
- **Heatmap Lift (15 kỹ năng phổ biến nhất: python, sql, ci/cd, docker, aws, java, agile, git, linux, devops, kubernetes, rest api, azure, microservices, postgresql):** ô đậm nhất là `docker ⇄ kubernetes` (3,33), `aws ⇄ azure` (3,22), `kubernetes ⇄ devops` (2,85), `microservices ⇄ rest api` (2,68); ô trống = không có luật. **Heatmap không có Spring, HTML, CSS** (không nằm trong 15 kỹ năng phổ biến nhất) và không còn Scrum sau khi loại tin chưa xác nhận (70/103 tin `pending_review` có Scrum); hai cụm front-end và Java/Spring chỉ thấy ở bảng top Lift.
- **Lưu ý:** Code **đã lưu** cả hai ảnh (`output/scatter_support_confidence.png`, `output/heatmap_lift.png`) bằng `plt.savefig(...)` đặt **trước** `plt.show()`; ô code cũng lưu `frequent_itemsets.csv`, `association_rules_filtered.csv` (có cột `is_redundant`) và `association_rules_nonredundant.csv`.

---

## 4. BẢNG THAM SỐ & LÝ DO

| Tham số | Giá trị | Tác dụng | Nếu tăng | Nếu giảm |
|---|:-:|---|---|---|
| `MIN_COUNT` | 30 | Lọc kỹ năng hiếm | Mất kỹ năng ngách | Thêm nhiễu |
| `MIN_SUP` | 0,03 | Ngưỡng tập phổ biến | Ít luật, chắc hơn | Bùng nổ số luật |
| `max_len` | 4 | Độ dài tối đa tổ hợp | Luật dài, khó đọc, chậm | Mất tổ hợp phức tạp |
| `MIN_CONF` | 0,5 | Độ chắc chắn tối thiểu | Luật ít mà chắc | Nhiều luật yếu |
| `MIN_LIFT` | 1,2 | Loại luật độc lập/ngược | Chỉ giữ gắn kết mạnh | Lẫn luật ngẫu nhiên |
| `MIN_CONV` | 1,2 | Loại luật kém tin cậy | Khắt khe hơn | Nới lỏng |

---

## 5. PHÁT HIỆN CHÍNH

1. **Cụm kỹ năng rõ rệt:** web front-end · mobile (Android + iOS) · Java/Spring · DevOps-Cloud (Docker, K8s, AWS) · Data/ML.
2. **Luật có hướng:** Kubernetes ⇒ Docker (73,6%) mạnh hơn Docker ⇒ Kubernetes (62,3%).
3. **Phổ biến ≠ gắn kết:** Python/SQL xuất hiện nhiều nhất nhưng lift chỉ 1,14.
4. **Confidence 100% thường hiển nhiên:** framework ⇒ ngôn ngữ, không phải phát hiện thị trường. `spring boot ⇒ java` chỉ 99,41% (168/169): luôn đếm số tin thay vì làm tròn.
5. **Apriori và FP-Growth cho cùng 856 tập phổ biến**; tốc độ phụ thuộc máy, ở dữ liệu nhỏ Apriori có thể nhanh hơn dù lý thuyết ngược lại.
6. **Tổ hợp ngách có lift cao nhưng support thấp:** cần cân bằng giữa "phổ biến" và "bất ngờ".
7. **Phải loại luật dư thừa trước khi xếp hạng:** 201/1.228 luật là biến thể dài hơn của luật khác.

---

## 6. LỖI DỄ MẮC & LƯU Ý

- ❌ Chỉ nhìn confidence → bị đánh lừa bởi kỹ năng phổ biến. ✅ Luôn xem kèm **lift**.
- ❌ Hiểu luật là nhân quả. ✅ Đó chỉ là **đồng xuất hiện**.
- ❌ Diễn giải luật conf 100% như phát hiện mới. ✅ Kiểm tra xem có phải quan hệ bao hàm (framework ⇒ ngôn ngữ).
- ❌ Hỏi về item không có trong dữ liệu (`ai`, `cloud`). ✅ Kiểm tra `onehot.columns` trước.
- ❌ `savefig` đặt sau `plt.show()` → file ảnh trống. ✅ Đặt **trước** `show()`.
- ❌ Xếp hạng top luật khi chưa loại luật dư thừa → bảng đầy biến thể của một luật. ✅ Chạy 2.4.8b rồi xếp hạng trên `rules_nr`.
- ❌ Dùng mẫu khác với 2.2/2.3 (không lọc `scope_status`, `has_jd_clean`). ✅ Dùng chung bộ lọc.
- ❌ Lưu `frozenset` thẳng vào CSV. ✅ Đổi thành chuỗi bằng `', '.join(sorted(x))` trước.

---

## 7. HẠN CHẾ & HƯỚNG PHÁT TRIỂN

**Hạn chế**
- Mẫu gồm 5 nguồn (TopCV, LinkedIn, ITviec, VietnamWorks, TopDev; TopCV chiếm ~37% số giỏ) nhưng thu thập trong một đợt duy nhất → chưa đại diện toàn thị trường
- Ngưỡng do người làm chọn nên kết quả phụ thuộc tham số
- Bỏ 413 tin có < 2 kỹ năng và 292 tin chưa đủ điều kiện phạm vi/JD (266 tin `pending_review` + 26 tin trong phạm vi nhưng JD chưa sạch), có thể lệch mẫu
- Không phân biệt kỹ năng "bắt buộc" và "ưu tiên" trong mô tả

**Hướng phát triển**
- Gợi ý lộ trình học: "đã biết A, nên học B"
- Phân cụm công việc theo bộ kỹ năng (dùng `text_for_tfidf`)
- Kết hợp lương/kinh nghiệm (`Luong_TB`, `experience_group`) xem cụm nào được trả cao
- So sánh luật theo cấp độ (Junior/Senior) hoặc theo thành phố

---

## 8. CÂU HỎI ÔN TẬP NHANH

1. Vì sao phải bỏ transaction có < 2 kỹ năng?
2. Confidence cao nhưng lift ≈ 1 nghĩa là gì?
3. Vì sao `kubernetes ⇒ docker` có confidence cao hơn `docker ⇒ kubernetes` nhưng lift hai chiều bằng nhau?
4. Vì sao với dữ liệu nhỏ Apriori có thể nhanh hơn FP-Growth?
5. Conviction = ∞ có nghĩa là gì và cần thận trọng ra sao?
6. Vì sao loại luật "conf ≥ 0,6 mà lift < 1,5" không xuất hiện trong dữ liệu này?

<details>
<summary>Gợi ý đáp án</summary>

1. Giỏ chỉ có 1 món thì không tạo được luật A ⇒ B.
2. Vế phải vốn đã phổ biến nên luật "đúng" chỉ do trùng hợp, A không thật sự kéo B lên.
3. Confidence chia cho support vế trái (khác nhau theo chiều), còn lift chia cho tích support hai vế nên đối xứng.
4. Dữ liệu nhỏ và `max_len=4`; chi phí dựng FP-tree có thể lớn hơn lợi ích.
5. Confidence = 100%, không có ngoại lệ trong mẫu; thường là quan hệ bao hàm và chưa chắc đúng với toàn thị trường.
6. Cần support(B) > 0,4 mà kỹ năng phổ biến nhất chỉ đạt 36,8%.
</details>
