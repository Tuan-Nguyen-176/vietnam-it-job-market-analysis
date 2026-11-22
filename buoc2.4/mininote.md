
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
`3.135 tin → 2.552 giỏ kỹ năng → ma trận 2.552×121 → 769 tập phổ biến → 3.472 luật thô → 1.070 luật tốt → top luật & biểu đồ`

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

### Bước 2.4.1: Nạp dữ liệu
- **Làm gì:** Đọc `clean_data_processed.csv` (kết quả bước 2.1–2.2), có kiểm tra file tồn tại.
- **Vì sao:** Đầu vào sạch quyết định chất lượng cả bài.
- **Kết quả:** 3.135 tin, 47 cột (định danh, thời gian, lương, kinh nghiệm, nhóm kỹ năng...).
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
- **Kết quả:** 157 → **121 kỹ năng**; **2.552 transaction** (bỏ ~583 tin).
- **Đánh đổi:** Ngưỡng thấp → giữ được kỹ năng ngách nhưng nhiễu; cao → chắc nhưng mất chuyên biệt.
- **Nối sang bước sau:** Chuyển danh sách thành ma trận để thuật toán đọc được.

### Bước 2.4.4: One-hot encoding
- **Làm gì:** `TransactionEncoder` → ma trận hàng = tin, cột = kỹ năng, ô = True/False.
- **Vì sao:** `mlxtend` chỉ nhận bảng boolean.
- **Kết quả:** Ma trận **2.552 × 121**, rất thưa (đa số False).
- **Top kỹ năng (support):** python 35,1% · sql 30,9% · ci/cd 28,0% · agile 23,0% · docker 21,1% · aws 21,0% · java 20,1% · git 19,3% · linux 19,0% · devops 18,7%.
- **Ý nghĩa:** Thị trường cần cả lập trình (Python/Java/SQL), quy trình (Agile/CI-CD) và hạ tầng (Docker/AWS/Linux).
- **Nối sang bước sau:** Tìm tổ hợp kỹ năng phổ biến.

### Bước 2.4.5: Apriori
- **Làm gì:** `apriori(min_support=0.03, max_len=4)`.
- **Vì sao chọn tham số:** 0,03 ≈ 77 tin: đủ tin cậy nhưng vẫn giữ cụm chuyên biệt. `max_len=4` vì luật quá dài khó đọc, ít ứng dụng.
- **Nguyên lý:** Nếu một tập không phổ biến thì mọi tập chứa nó cũng không phổ biến → cắt tỉa sớm.
- **Kết quả:** **769 itemset**: 77 (1 kỹ năng) · 328 (2) · 285 (3) · 79 (4). Thời gian 0,033s.
- **Nhận xét:** Số tập đạt đỉnh ở kích thước 2 rồi giảm: tổ hợp càng dài càng hiếm.
- **Nối sang bước sau:** Kiểm chứng bằng thuật toán khác.

### Bước 2.4.6: FP-Growth
- **Làm gì:** `fpgrowth` cùng tham số, so sánh với Apriori.
- **Khác biệt:** Apriori sinh ứng viên rồi đếm lại nhiều lần; FP-Growth nén dữ liệu vào cây FP-tree, không sinh ứng viên.
- **Kết quả:** Cùng **769 itemset** (`True`). Apriori 0,033s, FP-Growth 0,087s.
- **Vì sao Apriori nhanh hơn?** Dữ liệu nhỏ + `max_len=4`; FP-Growth mất chi phí dựng cây. FP-Growth chỉ vượt trội khi dữ liệu lớn/support thấp. Thời gian cỡ phần trăm giây dễ dao động, đừng kết luận quá mạnh.
- **Nối sang bước sau:** Từ tập phổ biến sinh luật có hướng.

### Bước 2.4.7: Sinh luật
- **Làm gì:** `association_rules(metric='confidence', min_threshold=0.0)`, giữ **tất cả** để tự lọc sau. `try/except` xử lý khác biệt phiên bản mlxtend (`num_itemsets`).
- **Kết quả:** **3.472 luật thô**.
- **Ví dụ:** `sql ⇒ python` lift 1,19 (yếu) · `sql ⇒ devops` lift 0,86 (ngược chiều) · `linux ⇒ devops` lift 1,61 (khá).
- **Nối sang bước sau:** Nhiều luật yếu/ngược chiều → lọc.

### Bước 2.4.8: Lọc luật
- **Tiêu chí:** support ≥ 0,03 · confidence ≥ 0,5 · lift > 1,2 · conviction ≥ 1,2.
- **Vì sao nhiều tiêu chí:** Confidence cao chưa đủ (nếu B quá phổ biến thì A ⇒ B luôn đúng).
- **Kết quả:** **1.070 / 3.472 luật (~31%)**.
- **Lưu ý:** `python ⇒ sql` (lift 1,19) bị loại ở đây, nhưng bước 2.4.10 tra trong luật thô nên vẫn thấy.
- **Nối sang bước sau:** Còn hơn 1.000 luật → xếp hạng.

### Bước 2.4.9: Xếp hạng top luật
- **Theo Lift** (gắn kết bất ngờ): `android ⇄ ios` ≈ **16,8**; cụm web front-end (`html, css, javascript, git, react, rest api`) lift 13–15; `{html, react} ⇒ {css}` conf 97,7%.
- **Theo Confidence** (chắc chắn): nhiều luật **100%**: `fastapi ⇒ python`, `spring ⇒ java`, `spring boot ⇒ java`.
- **Nhận xét:** Lift cho thấy *cụm kỹ năng ngành*; Confidence thường cho thấy *quan hệ kỹ thuật hiển nhiên* (framework thuộc ngôn ngữ).
- **Nối sang bước sau:** Tra các cặp cụ thể.

### Bước 2.4.10: Luật theo chủ đề

| Luật | Support | Confidence | Lift | Đọc kết quả |
|---|:-:|:-:|:-:|---|
| docker ⇒ kubernetes | 13,2% | 62,3% | 3,48 | Có Docker thì ~62% cần K8s |
| kubernetes ⇒ docker | 13,2% | **73,5%** | 3,48 | Chiều ngược mạnh hơn |
| docker, kubernetes ⇒ aws | 6,5% | 49,7% | 2,37 | Nhóm container hay kèm AWS |
| python ⇒ sql | 12,9% | 36,8% | **1,19** | Liên kết yếu |
| machine learning ⇒ python | 5,0% | **79,0%** | 2,25 | Python gần như bắt buộc với ML |
| python, sql ⇒ machine learning | – | – | – | **Không có luật** (support < 0,03) |

- **Bất đối xứng:** Docker là kỹ năng nền, K8s là nâng cao → ai biết K8s hầu như biết Docker, ngược lại thì không.
- **Python + SQL yếu:** cùng phổ biến nên hay trùng nhau, không phải gắn kết đặc biệt.
- **Nối sang bước sau:** Kiểm tra luật "trông mạnh nhưng vô nghĩa".

### Bước 2.5.11: Luật gây nhiễu
- **Kiểm tra 1:** conf ≥ 0,6 mà lift < 1,5 → **0 luật**. Giải thích: lift = conf ÷ support(B), nên để lift < 1,5 với conf ≥ 0,6 thì support(B) phải > 0,4; nhưng kỹ năng phổ biến nhất (Python) chỉ 35,1% → loại "bẫy" này không thể xảy ra ở dữ liệu này. (Lưu ý: đoạn code chạy trên `rules` thô, không phải `rules_f`.)
- **Kiểm tra 2:** **6 luật conviction = ∞**: `spring ⇒ java`, `spring, spring boot ⇒ java`, `rest api, spring boot ⇒ java`, `microservices, rest api, spring boot ⇒ java`, `rest api, scrum ⇒ agile`, `fastapi ⇒ python`. Phần lớn là quan hệ "thuộc về"; `scrum ⇒ agile` hợp lý vì Scrum là một phương pháp Agile. Conviction ∞ chỉ nói *không có ngoại lệ trong mẫu*, chưa phải quy luật tuyệt đối.
- **Kiểm tra 3:** `ai`, `cloud` **không có** trong dữ liệu → luôn kiểm tra `onehot.columns` trước khi tra một chủ đề.
- **Nối sang bước sau:** Vẽ biểu đồ để nhìn tổng thể.

### Bước 2.5.12: Trực quan hóa
- **Scatter (Support × Confidence, màu = Lift):** mỗi chấm một luật; luật lý tưởng ở góc trên phải. Phần lớn luật có support thấp (~3–6%); luật lift cao thường là tổ hợp ngách.
- **Heatmap Lift (15 kỹ năng phổ biến nhất):** khối ô đậm = cụm gắn kết (front-end, Java/Spring, Docker/K8s/AWS); ô trống = không có luật.
- **Lưu ý:** Code chỉ `plt.show()`, không lưu file. Muốn có file phải thêm `plt.savefig(...)` **trước** `plt.show()`.

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
2. **Luật có hướng:** Kubernetes ⇒ Docker (73,5%) mạnh hơn Docker ⇒ Kubernetes (62,3%).
3. **Phổ biến ≠ gắn kết:** Python/SQL xuất hiện nhiều nhất nhưng lift chỉ 1,19.
4. **Confidence 100% thường hiển nhiên:** framework ⇒ ngôn ngữ, không phải phát hiện thị trường.
5. **Apriori nhanh hơn FP-Growth** ở bộ dữ liệu nhỏ này, dù lý thuyết ngược lại.
6. **Tổ hợp ngách có lift cao nhưng support thấp:** cần cân bằng giữa "phổ biến" và "bất ngờ".

---

## 6. LỖI DỄ MẮC & LƯU Ý

- ❌ Chỉ nhìn confidence → bị đánh lừa bởi kỹ năng phổ biến. ✅ Luôn xem kèm **lift**.
- ❌ Hiểu luật là nhân quả. ✅ Đó chỉ là **đồng xuất hiện**.
- ❌ Diễn giải luật conf 100% như phát hiện mới. ✅ Kiểm tra xem có phải quan hệ bao hàm (framework ⇒ ngôn ngữ).
- ❌ Hỏi về item không có trong dữ liệu (`ai`, `cloud`). ✅ Kiểm tra `onehot.columns` trước.
- ❌ `savefig` đặt sau `plt.show()` → file ảnh trống. ✅ Đặt **trước** `show()`.
- ❌ Lưu `frozenset` thẳng vào CSV. ✅ Đổi thành chuỗi bằng `', '.join(sorted(x))` trước.

---

## 7. HẠN CHẾ & HƯỚNG PHÁT TRIỂN

**Hạn chế**
- Chỉ một nguồn (ITviec), một thời điểm → chưa đại diện toàn thị trường
- Ngưỡng do người làm chọn nên kết quả phụ thuộc tham số
- Bỏ ~583 tin có < 2 kỹ năng, có thể lệch mẫu
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
4. Vì sao Apriori nhanh hơn FP-Growth trong bài này?
5. Conviction = ∞ có nghĩa là gì và cần thận trọng ra sao?
6. Vì sao loại luật "conf ≥ 0,6 mà lift < 1,5" không xuất hiện trong dữ liệu này?

<details>
<summary>Gợi ý đáp án</summary>

1. Giỏ chỉ có 1 món thì không tạo được luật A ⇒ B.
2. Vế phải vốn đã phổ biến nên luật "đúng" chỉ do trùng hợp, A không thật sự kéo B lên.
3. Confidence chia cho support vế trái (khác nhau theo chiều), còn lift chia cho tích support hai vế nên đối xứng.
4. Dữ liệu nhỏ và `max_len=4`; chi phí dựng FP-tree lớn hơn lợi ích.
5. Confidence = 100%, không có ngoại lệ trong mẫu; thường là quan hệ bao hàm và chưa chắc đúng với toàn thị trường.
6. Cần support(B) > 0,4 mà kỹ năng phổ biến nhất chỉ đạt 35,1%.
</details>
