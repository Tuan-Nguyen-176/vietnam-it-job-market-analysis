# BÁO CÁO BƯỚC 2.4: KHAI PHÁ LUẬT KẾT HỢP KỸ NĂNG TRONG TIN TUYỂN DỤNG IT

## 1. Mục tiêu và bài toán

**Câu hỏi nghiên cứu:** trong các tin tuyển dụng IT, những kỹ năng nào thường được yêu cầu **cùng nhau**, và mức độ gắn kết giữa chúng mạnh đến đâu?

**Cách mô hình hóa:** bài toán được xem như phân tích giỏ hàng. Mỗi tin tuyển dụng là một "giỏ hàng", mỗi kỹ năng (python, sql, docker...) là một "món hàng". Luật `A ⇒ B` diễn đạt: *tin nào yêu cầu A thì thường cũng yêu cầu B*.

**Đầu ra mong muốn:** một tập luật kết hợp ngắn gọn, đã loại luật yếu và luật dư thừa, kèm diễn giải có thể dùng cho định hướng học tập và tuyển dụng.

## 2. Dữ liệu và tiền xử lý

### 2.1. Nguồn dữ liệu
Dữ liệu là file `clean_data_processed.csv` (kết quả của bước 2.1–2.2) gồm **3.135 tin, 47 cột**, thu thập từ 5 nguồn (TopCV, LinkedIn, ITviec, VietnamWorks, TopDev) trong một đợt duy nhất.

### 2.2. Lọc phạm vi (Bước 2.4.1)
Dùng cùng bộ lọc với bước 2.2 và 2.3 để ba bước chung một mẫu số:

1. `scope_status == "included_provisional"`: tin được chấp nhận sơ bộ là việc làm IT. 266 tin `pending_review` (phần lớn là PM/PO, Scrum Master, Business Analyst) chưa được xác nhận nên chưa đưa vào.
2. `has_jd_clean == True`: tin có mô tả công việc (JD) đủ sạch. Điều kiện này cần thiết vì cột `skills` gộp kỹ năng từ tiêu đề, thẻ và JD, nên tin không có JD vẫn có thể mang kỹ năng mà ta chưa đọc được trong mô tả.

| Bước lọc | Số tin |
|---|---:|
| Toàn bộ file | 3.135 |
| Thuộc phạm vi IT sơ bộ | 2.869 |
| Thuộc phạm vi IT sơ bộ **và** có JD sạch | **2.843** |

Số tin theo nguồn sau lọc: TopCV 1.138, LinkedIn 637, ITviec 530, VietnamWorks 477, TopDev 61.

### 2.3. Chọn thuộc tính (Bước 2.4.2)
Khảo sát 8 cột (`title`, `tech_list`, `keywords_list`, `skills`, `broad_concepts`, `application_domains`, `languages`, `benefits`). Cột **`skills`** được chọn vì sạch và đúng nghĩa "kỹ năng yêu cầu"; trộn các cột khác loại (ví dụ `benefits`) sẽ cho luật khó diễn giải.

### 2.4. Giảm thuộc tính (Bước 2.4.3)
Các thao tác theo thứ tự:

1. Đọc chuỗi JSON của cột `skills` thành danh sách (lỗi thì trả danh sách rỗng).
2. Chuẩn hóa bằng `strip()` và `lower()`; dùng `set` để loại kỹ năng trùng trong cùng một tin.
3. Giữ kỹ năng xuất hiện ở **≥ 30 tin** (`MIN_COUNT = 30`).
4. Bỏ tin còn **dưới 2 kỹ năng**, vì giỏ chỉ có một món thì không tạo được luật `A ⇒ B`.

| Chỉ số | Giá trị |
|---|---|
| Số kỹ năng ban đầu | 157 |
| Số kỹ năng giữ lại | **121** (loại 36 kỹ năng hiếm) |
| Số giỏ (transaction) | **2.430** (bỏ 413 tin có dưới 2 kỹ năng) |

Số giỏ theo nguồn: TopCV 894, LinkedIn 591, ITviec 496, VietnamWorks 396, TopDev 53.

> **Lưu ý về mẫu số.** Từ bước này, mọi support, confidence và lift được tính trên **2.430 giỏ** (tin nêu ít nhất 2 kỹ năng), không phải trên 2.843 tin.

### 2.5. Mã hóa One-Hot (Bước 2.4.4)
Dùng `TransactionEncoder` để chuyển danh sách giỏ thành ma trận True/False kích thước **2.430 × 121** (hàng là tin, cột là kỹ năng), vì thư viện `mlxtend` chỉ nhận bảng boolean. Ma trận rất thưa, đúng với đặc điểm dữ liệu giỏ hàng.

Các kỹ năng phổ biến nhất (tỷ lệ trên 2.430 giỏ): **python 36,8%, sql 32,2%, ci/cd 29,3%, docker 22,1%, aws 21,8%**, tiếp theo là java, agile, git, linux, devops. Danh sách này gồm cả ngôn ngữ lập trình, quy trình (Agile, CI/CD) và hạ tầng (Docker, AWS, Linux), cho thấy tin tuyển dụng IT đòi hỏi cả kỹ năng lập trình lẫn vận hành.

## 3. Phương pháp và tham số

### 3.1. Các thước đo
| Thước đo | Công thức | Cách đọc |
|---|---|---|
| Support(X) | số giỏ chứa X ÷ tổng số giỏ | Tổ hợp phổ biến đến mức nào |
| Confidence(A⇒B) | Support(A∪B) ÷ Support(A) = P(B\|A) | Trong các tin có A, bao nhiêu % có B |
| Lift | Confidence ÷ Support(B) | >1: kéo nhau lên; =1: độc lập; <1: ít đi cùng nhau hơn kỳ vọng |
| Conviction | (1 − Support(B)) ÷ (1 − Confidence) | Luật "sai" ít hơn đoán ngẫu nhiên bao nhiêu lần; ∞ khi confidence = 100% |

Confidence có tính **bất đối xứng** (`conf(A⇒B) ≠ conf(B⇒A)`), còn Lift thì **đối xứng**.

### 3.2. Bảng tham số
| Tham số | Giá trị | Tác dụng |
|---|:-:|---|
| `MIN_COUNT` | 30 | Giữ kỹ năng có ở ≥ 30 tin |
| `MIN_SUP` | 0,03 | Tập phổ biến phải có ở ≥ 3% giỏ (≥ 73 giỏ) |
| `max_len` | 4 | Tối đa 4 kỹ năng một tổ hợp (luật dài khó đọc) |
| `MIN_CONF` | 0,5 | Ít nhất một nửa tin có A thì có B |
| `MIN_LIFT` | 1,2 | Lift phải lớn hơn 1,2 để loại luật độc lập hoặc ngược chiều |
| `MIN_CONV` | 1,2 | Luật phải đáng tin hơn đoán ngẫu nhiên |

### 3.3. Quy trình khai phá
1. **Tập phổ biến:** chạy Apriori (Bước 2.4.5) và kiểm chứng chéo bằng FP-Growth (Bước 2.4.6).
2. **Sinh luật:** `association_rules` với `min_threshold = 0` để giữ toàn bộ luật thô (Bước 2.4.7).
3. **Lọc luật:** áp dụng đồng thời bốn ngưỡng ở trên (Bước 2.4.8).
4. **Loại luật dư thừa:** luật `A ⇒ C` bị coi là dư thừa nếu có luật ngắn hơn `A' ⇒ C` đã qua lọc với cùng vế phải, `A'` là tập con thật sự của `A`, và confidence của luật ngắn không thấp hơn (Bước 2.4.8b).
5. **Xếp hạng và diễn giải** (Bước 2.4.9–2.4.11), **kiểm tra độ nhạy tham số** (Bước 2.4.11b) và **trực quan hóa** (Bước 2.4.12).

## 4. Kết quả

### 4.1. Tập phổ biến
Apriori tìm được **856 tập phổ biến**: 79 tập một kỹ năng, 351 tập hai kỹ năng, 331 tập ba kỹ năng và 95 tập bốn kỹ năng. Số tập đạt đỉnh ở kích thước 2 rồi giảm dần vì tổ hợp càng dài càng hiếm đạt ngưỡng 3%.

FP-Growth cho **cùng 856 tập**, xác nhận tính đúng đắn của kết quả. Về thời gian, Apriori mất 0,102s và FP-Growth 0,279s. Kết quả này ngược với lý thuyết vì dữ liệu nhỏ (2.430 × 121) và đã giới hạn `max_len = 4`, nên chi phí dựng cây FP-tree không đáng; thời gian cỡ phần trăm giây cũng dao động theo máy nên không nên kết luận mạnh.

### 4.2. Số luật qua từng bước
| Giai đoạn | Số luật |
|---|---:|
| Luật thô | 4.018 |
| Sau lọc theo ngưỡng (support, confidence, lift, conviction) | 1.228 (~31%) |
| Dư thừa bị loại | 201 (198 luật vế phải 1 kỹ năng, 3 luật vế phải 2 kỹ năng) |
| **Luật chất lượng cuối cùng** | **1.027** |

Nếu không loại luật dư thừa, 8 trong 15 dòng đầu bảng Top Confidence là biến thể dài hơn của `spring ⇒ java` hoặc `spring boot ⇒ java`. Ví dụ: `{spring, spring boot} ⇒ java` (83/83) là dư thừa vì `spring ⇒ java` cũng đạt 100% nhưng áp dụng cho 117 tin. Ngược lại `{rest api, spring boot} ⇒ java` (109/109) được giữ vì confidence tăng so với `spring boot ⇒ java` (168/169 = 99,41%).

### 4.3. Top luật theo Lift (gắn kết bất ngờ)
- `android ⇄ ios` có lift cao nhất, khoảng **16,0** (confidence 71,2% và 82,4% theo hai chiều), cho thấy nhiều tin mobile nêu cả hai nền tảng.
- Cụm **web front-end** (`html, css, javascript, git, rest api`) đạt lift **13–15**; ví dụ `{html, react} ⇒ {css}` có confidence 97,7%.

### 4.4. Top luật theo Confidence
Năm luật đạt **100%**: `spring ⇒ java` (117/117), `{rest api, spring boot} ⇒ java` (109/109), `{spring boot, sql} ⇒ java` (75/75), `fastapi ⇒ python` (79/79) và `{rest api, scrum} ⇒ agile` (91/91). Đây chủ yếu là quan hệ "framework thuộc ngôn ngữ", đúng trong mẫu nhưng hiển nhiên về mặt kỹ thuật, không phải phát hiện mới về thị trường. Riêng `spring boot ⇒ java` chỉ đạt 168/169 = 99,41% vì có đúng một ngoại lệ.

### 4.5. Các luật theo chủ đề (tính trên 2.430 giỏ)
| Luật | Support | Confidence | Lift | Nhận xét |
|---|:-:|:-:|:-:|---|
| docker ⇒ kubernetes | 13,8% | 62,3% | 3,33 | Có Docker thì khoảng 62% có Kubernetes |
| kubernetes ⇒ docker | 13,8% | **73,6%** | 3,33 | Chiều ngược lại mạnh hơn |
| docker, kubernetes ⇒ aws | 6,9% | 49,9% | 2,29 | Nhóm container thường kèm AWS |
| python ⇒ sql | 13,5% | 36,8% | **1,14** | Liên kết yếu |
| machine learning ⇒ python | 5,3% | **80,0%** | 2,17 | Python gần như đi kèm ML |
| spring ⇒ java | 4,8% | **100%** | 4,77 | Tuyệt đối trong mẫu |
| sql ⇒ devops | 5,1% | 16,0% | **0,83** | Ít đi cùng nhau hơn kỳ vọng |
| linux ⇒ devops | 6,0% | 30,1% | 1,56 | Liên kết khá rõ |

Luật `{python, sql} ⇒ machine learning` **không xuất hiện** ở ngưỡng support 3%: tổ hợp ba kỹ năng có 56 giỏ (2,30%), thiếu 17 giỏ so với ngưỡng 73 giỏ. Confidence của nó cũng chỉ 17,0% (< 50%). Kết luận đúng là "không tìm thấy luật ở ngưỡng đã chọn", không phải "ba kỹ năng không đi cùng nhau".

### 4.6. Trực quan hóa
- **Biểu đồ phân tán** (support × confidence, màu theo lift, vẽ trên 1.027 luật): 90,8% luật có support dưới 6%; 62 luật có lift từ 8 trở lên đều là tổ hợp ngách. Chỉ 8 luật có support ≥ 10% và confidence ≥ 70%. Luật mạnh thường là luật chuyên biệt chứ không phải luật phổ biến.
- **Heatmap lift** của 15 kỹ năng phổ biến nhất: ô đậm nhất là `docker ⇄ kubernetes` (3,33), `aws ⇄ azure` (3,22), `kubernetes ⇄ devops` (2,85) và `microservices ⇄ rest api` (2,68). Ô trống là cặp không có luật. Heatmap không thể hiện cụm front-end hay Java/Spring vì `html`, `css`, `spring` không nằm trong 15 kỹ năng phổ biến nhất.

Hai biểu đồ và ba file CSV (`frequent_itemsets.csv`, `association_rules_filtered.csv`, `association_rules_nonredundant.csv`) được lưu trong thư mục `output/`.

## 5. Kiểm tra độ tin cậy

### 5.1. Ảnh hưởng của mẫu số (Bước 2.4.10b)
Tính lại mười luật chủ đề trên toàn bộ 2.843 tin (thay vì 2.430 giỏ):
- **Support giảm** khoảng 14,5% vì mẫu số tăng (ví dụ Docker–Kubernetes từ 13,79% xuống 11,78%).
- **Confidence gần như không đổi** (6/10 cặp giống hệt, 4/10 giảm không quá 1,5 điểm phần trăm).
- **Lift tăng ở mọi luật** (từ ×1,03 đến ×1,17). Đáng chú ý, `python ⇒ sql` tăng từ 1,14 lên 1,28, nên nhận định "Python và SQL liên kết yếu" phụ thuộc vào mẫu số.
- **Bộ luật khá ổn định:** 1.225/1.228 luật vẫn đạt confidence, lift và conviction khi tính trên 2.843 tin. Ba luật rớt (`c# ⇒ .net`, `linux ⇒ python`, `html ⇒ {css, rest api}`) đều do confidence tụt sát dưới 50%.
- Nếu đòi thêm support ≥ 3% của 2.843 tin (≥ 86 tin) thì chỉ còn **705** luật.

### 5.2. Luật gây nhiễu (Bước 2.4.11)
- Không có luật nào có confidence ≥ 0,6 mà lift < 1,5. Điều này có thể giải thích bằng toán: muốn vậy support của vế phải phải lớn hơn 0,4, trong khi kỹ năng phổ biến nhất (Python) chỉ đạt 36,8%.
- Có 8 luật conviction vô cực (confidence = 100%), trong đó 3 luật là dư thừa. Conviction vô cực chỉ nói không có ngoại lệ trong mẫu, chưa phải quy luật tuyệt đối.
- `ai` và `cloud` không có trong ma trận vì bước 2.1 đã tách chúng sang cột `broad_concepts`.

### 5.3. Độ nhạy của `MIN_COUNT` (Bước 2.4.11b)
| `MIN_COUNT` | Kỹ năng giữ | Giỏ | Tập phổ biến | Luật thô | Luật qua lọc |
|:-:|:-:|:-:|:-:|:-:|:-:|
| 10 | 145 | 2.437 | 832 | 3.870 | 1.182 |
| **30** | 121 | 2.430 | 856 | 4.018 | 1.228 |
| 50 | 99 | 2.405 | 856 | 4.018 | 1.228 |
| 73 | 79 | 2.371 | 879 | 4.156 | 1.280 |
| 100 | 64 | 2.333 | 913 | 4.498 | 1.374 |

Từ 30 đến 50, bộ luật không đổi, vì các kỹ năng bị loại thêm đều dưới 73 giỏ nên dù giữ lại cũng không đạt `MIN_SUP`. Tham số quyết định thật sự là **`MIN_SUP`**, không phải `MIN_COUNT`; không nên mô tả mức 30 là "điểm cân bằng tối ưu".

## 6. Thảo luận

1. **Cụm kỹ năng rõ rệt:** web front-end (HTML/CSS/JavaScript/React), mobile (Android + iOS), Java backend (Spring), DevOps/Cloud (Docker, Kubernetes, AWS, Azure) và Data/ML (ML ⇒ Python).
2. **Quan hệ có hướng:** 73,6% tin có Kubernetes cũng có Docker, so với 62,3% chiều ngược lại. Chênh lệch đến từ việc Kubernetes ít phổ biến hơn Docker (455 so với 538 giỏ), vì tử số (335 giỏ) giống nhau. Điều này phù hợp với cách hiểu Docker là kỹ năng nền, nhưng dữ liệu JD không chứng minh điều đó.
3. **Phổ biến không đồng nghĩa với gắn kết:** Python và SQL xuất hiện nhiều nhất nhưng lift chỉ 1,14, tức phần lớn việc đi cùng nhau được giải thích bởi việc cả hai cùng phổ biến.
4. **Confidence cao không phải lúc nào cũng có giá trị:** các luật 100% còn lại chủ yếu là quan hệ kỹ thuật hiển nhiên (framework ⇒ ngôn ngữ, Scrum ⇒ Agile). Hai bảng xếp hạng cho hai loại thông tin: Lift cho thấy cụm kỹ năng ngành, Confidence cho thấy quan hệ phụ thuộc kỹ thuật.
5. **Cần đếm số tin thay vì dựa vào phần trăm làm tròn:** `spring boot ⇒ java` là 168/169 chứ không phải 100%.
6. **Luật chỉ mô tả sự đồng xuất hiện** trong tin tuyển dụng, không chứng minh quan hệ nhân quả và không nói gì về năng lực người học.

## 7. Hạn chế

- Mẫu gồm 5 nguồn nhưng thu thập trong **một đợt duy nhất** và nghiêng về TopCV (khoảng 37% số giỏ), nên chưa đại diện toàn thị trường.
- Kỹ năng được trích bằng từ điển từ tiêu đề, thẻ và JD; **không phân biệt** kỹ năng "bắt buộc" và "ưu tiên".
- Các ngưỡng do người thực hiện chọn nên kết quả phụ thuộc tham số; `MIN_SUP` là tham số quyết định.
- Mọi tỷ lệ tính trên **2.430 giỏ**, không phải toàn bộ 2.843 tin; đổi mẫu số làm support giảm khoảng 14,5% và lift tăng tới ×1,17.
- Việc bỏ 413 tin có dưới 2 kỹ năng và các tin chưa đủ điều kiện phạm vi hoặc JD có thể gây lệch mẫu. 103 tin `pending_review` (nhiều Scrum Master/PM/BA) bị loại tạm thời; khi nhóm xác nhận phạm vi IT cuối cùng cần chạy lại.
- Ngưỡng support loại luật hiếm một cách có chủ đích nhưng cũng làm mất thông tin về các tổ hợp ngách.

## 8. Kết luận

Từ 3.135 tin ban đầu, quy trình lọc xuống 2.843 tin trong phạm vi IT có JD sạch, rồi 2.430 giỏ kỹ năng, ma trận 2.430 × 121, 856 tập phổ biến, 4.018 luật thô, 1.228 luật qua lọc và cuối cùng là **1.027 luật chất lượng** sau khi loại 201 luật dư thừa. Hai thuật toán Apriori và FP-Growth cho cùng kết quả, và bộ luật tương đối ổn định khi đổi mẫu số (1.225/1.228 luật vẫn đạt ngưỡng).

Kết quả cho thấy thị trường tuyển dụng IT hình thành các cụm kỹ năng khá rõ, đồng thời cảnh báo rằng cần đọc luật cùng với lift, số đếm cụ thể và mẫu số. Các luật confidence 100% cần được diễn giải thận trọng vì phần lớn là quan hệ kỹ thuật hiển nhiên.

**Hướng phát triển:**
- Xây dựng gợi ý lộ trình học ("đã biết A, nên học B") dựa trên luật có lift và confidence cao, kèm kiểm chứng thêm vì luật chỉ mô tả cách nhà tuyển dụng liệt kê kỹ năng.
- Phân cụm công việc theo bộ kỹ năng (dùng thêm `text_for_tfidf`).
- Kết hợp với lương và kinh nghiệm (`Luong_TB`, `experience_group`) để xem cụm kỹ năng nào được trả cao hơn.
- So sánh luật theo cấp độ (Junior/Senior) hoặc theo thành phố; chạy lại khi nhóm chốt phạm vi IT chính thức.
