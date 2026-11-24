# Bài thuyết trình – Nhóm 40 · ITEC3417 Khai phá dữ liệu
## Thị trường việc làm IT Việt Nam qua 3.204 tin tuyển dụng

> **Thời lượng dự kiến:** 12 phút (≈ 1.700 từ, tốc độ nói ~140 từ/phút).
> **Phân công:** Đức (~3 phút) → Tuấn (~2,5 phút) → Thành (~3 phút) → Quân (~3,5 phút).
> Chữ in đậm là con số nên nhấn mạnh khi nói.

---

## Mở đầu (Slide 1–2) · ~1 phút · *Đức*

Xin chào thầy Huỳnh Tấn Phát và các bạn. Chúng em là **nhóm 40**, môn Khai phá dữ liệu. Đề tài của nhóm là: *thị trường việc làm IT Việt Nam qua 3.204 tin tuyển dụng* – đi từ một crawler tự viết cho đến gom cụm, luật kết hợp và phân lớp khoảng lương.

Cả bài được dẫn dắt bởi **năm câu hỏi nghiên cứu**:

- **Q1:** Lương thay đổi thế nào theo cấp bậc và vùng?
- **Q2:** Kỹ năng nào được yêu cầu nhiều nhất?
- **Q3:** Thị trường chia thành những chân dung tuyển dụng nào?
- **Q4:** Những kỹ năng nào hay đi cùng nhau?
- **Q5:** Có dự đoán được khoảng lương từ kỹ năng và kinh nghiệm không?

Nhóm đi theo một quy trình KDD duy nhất: thu thập, làm sạch, rồi lần lượt qua các bước 2.1 đến 2.5. Bốn thành viên mỗi người phụ trách một phần, và em xin bắt đầu với phần thu thập dữ liệu.

---

## Phần 1 · Thu thập và làm sạch dữ liệu (Slide 3–9) · ~2 phút · *Đức*

**Kiến trúc (slide 4).** Nhóm tự viết **5 crawler** cho ITviec, TopDev, VietnamWorks, TopCV và LinkedIn, thu được **7.388 tin thô**. Điểm thiết kế quan trọng là *lọc IT trước, rồi mới mở trang chi tiết*: chỉ **3.216 trang** cần tải thay vì 7.388. Dữ liệu được cào ngày 17/11/2026, và mọi dòng đều có URL gốc cùng thời điểm cào để thầy có thể mở link đối chiếu.

**Năm nguồn, năm cách cào (slide 5).** Mỗi nguồn một cách: ITviec dùng Playwright; TopDev neo vào đường dẫn vì class Tailwind vô nghĩa; VietnamWorks gọi thẳng API JSON; TopCV bị Cloudflare chặn nên chạy bán tự động; LinkedIn cần đăng nhập nên dùng hồ sơ Chrome có sẵn cookie. Nhóm không dùng dịch vụ cào trả phí.

**Thách thức (slide 6).** Trang tải động, chống bot, lương bị ẩn. Với lương ẩn, nhóm đọc trường `baseSalary` trong JSON-LD: có 649 tin có trường này nhưng chỉ **151 tin** ra số thật. Chương trình lưu tiến độ mỗi 20 tin nên dừng giữa chừng không mất dữ liệu.

**Phễu dữ liệu (slide 7).** Từ 7.388 tin thô, khử trùng còn **7.291**, lọc phạm vi IT còn **3.216**, chốt còn **3.204** tin – vượt yêu cầu tối thiểu 800 tin của đề. Sau khi gộp các tin đăng trên nhiều nguồn còn **3.135 việc làm**. Việc lọc IT không dùng AI mà dùng luật trên tiêu đề và tag, ví dụ "IT Sales" là bán hàng nên bị loại.

**Làm sạch (slide 8–9).** Lương được quy về VND/tháng với 1 USD = 25.000 đồng; không biết thì để trống chứ không điền 0. Chỉ **767 tin** có khoảng lương đóng đủ hai cận. Có một giả định duy nhất: TopCV không ghi kỳ trả nên được coi là lương tháng, gắn cờ cho **529 tin**. Địa điểm tập trung ở TP.HCM (1.569) và Hà Nội (1.391). Kỹ năng được trích bằng từ điển tự viết **161 mục**, tránh các bẫy như "test" nằm trong "Contest" hay React Native bị nhầm với React.

Em xin chuyển sang bạn Tuấn.

---

## Phần 2 · Bước 2.1 và 2.2: Tiền xử lý và EDA (Slide 10–22) · ~2,5 phút · *Tuấn*

Em xin cảm ơn bạn Đức. Phần của em là tiền xử lý và khám phá dữ liệu.

**Bước 2.1 (slide 11–14).** Từ `clean_data.csv`, nhóm tạo `clean_data_processed.csv` gồm **3.135 tin đại diện × 47 cột**, kèm **69 bản ghi phụ** để truy vết. Nguyên tắc là giữ nguyên cột gốc để đối chiếu, chỉ bổ sung đặc trưng và cờ chọn mẫu. Riêng kỹ năng, nhóm gộp hai cột `tech` và `keywords`, loại trùng, và tách khái niệm chung như Cloud với lĩnh vực ứng dụng như Fintech. Chuẩn hóa một lần, dùng chung cho gom cụm, luật kết hợp và phân lớp.

**Bước 2.2 (slide 15–22).** Có bảy phát hiện chính:

1. **Cơ cấu dữ liệu:** **2.843 tin** có mô tả công việc (99%), nhưng chỉ **688 tin** có lương hợp lệ (24%), và lệch về TopCV: từ 39,84% mẫu lên **68,02%** tập lương.
2. **Kỹ năng (Q2):** Top 3 là **Python 32,08%, SQL 28,21%, CI/CD 25,01%**. Lưu ý đây là tần suất được nhắc, không phải số người cần tuyển.
3. **Lương theo cấp bậc (Q1):** trung vị tăng từ Intern 4 triệu, Fresher 13,5, Junior 18, Middle 30 lên Senior **37,5 triệu**; Lead là 30,5 triệu, thấp hơn Senior trong mẫu này.
4. **Vùng:** Hà Nội 22,5 triệu, TP.HCM 21 triệu. Đà Nẵng cho 41,25 triệu nhưng chỉ có **10 tin lương**, nên nhóm không kết luận gì.
5. **Kỹ năng theo cấp bậc:** kỹ năng hạ tầng như AWS, CI/CD được nhắc nhiều hơn ở bậc cao, còn nhóm Dữ liệu/AI giữ Python và SQL ở mọi bậc.
6. **Ngoại ngữ và quyền lợi:** tiếng Anh xuất hiện ở khoảng 47–57% tin; thưởng KPI, dự án, lễ tết ở 76–78%.
7. **Độ bền:** top 3 kỹ năng giữ nguyên khi đếm theo công ty. Nhưng Python chiếm 46,9% tin LinkedIn so với 22,5% ở TopCV – nghĩa là kết luận phụ thuộc cơ cấu nguồn, chưa khái quát cho toàn thị trường được.

Tiếp theo là bước gom cụm, em xin nhường lại cho Đức.

---

## Phần 3 · Bước 2.3: Gom cụm K-Means (Slide 23–28) · ~1,5 phút · *Đức*

Câu hỏi Q3: tin tuyển dụng IT chia thành những nhóm kỹ năng nào? Nhóm biểu diễn mỗi tin là một vector 0/1 trên các kỹ năng. Sau khi giữ kỹ năng xuất hiện từ 10 tin và tin có từ 3 kỹ năng, ma trận cuối là **2.218 tin × 143 kỹ năng**, có chuẩn hóa L2 để cụm không chia theo việc tin ghi nhiều hay ít kỹ năng.

**Chọn K.** Biểu đồ Elbow không có khuỷu rõ, Silhouette thấp hơn 0,1 ở mọi K vì Git, Docker, SQL có mặt ở mọi nhóm. Vì vậy nhóm đặt ba tiêu chí: cụm nhỏ nhất ≥ 5%, độ ổn định ARI ≥ 0,80, rồi mới đến Silhouette. Kết quả chọn **K = 8**: với K = 7 mất cụm Backend Java, còn K = 8 không mất hồ sơ nào.

**Tám cụm:** Frontend và Web (363 tin), Dữ liệu doanh nghiệp (339), Backend Java (331), DevOps/Cloud (323), Data Science/ML/LLM (296), Hệ thống–mạng–bảo mật (211), Agile/Scrum (206), và C/C++ với Linux (149). Vì Python, SQL, Git có ở mọi cụm nên nhóm đặt tên bằng chỉ số *lift*, ví dụ Cisco có lift 9,0 trong cụm mạng.

**Độ tin cậy.** Tên cụm bền qua **20/20 lần** chạy lại, nhưng Silhouette chỉ **0,074** nên ranh giới mềm. Kết luận trung thực là: đây là *8 phân khúc kỹ năng chồng lấn*, không phải 8 nghề tách biệt.

Bây giờ mời bạn Thành.

---

## Phần 4 · Bước 2.4: Luật kết hợp kỹ năng (Slide 29–39) · ~3 phút · *Thành*

Em xin cảm ơn Đức. Câu hỏi Q4: kỹ năng nào hay được yêu cầu cùng nhau? Nhóm coi mỗi tin là một giỏ hàng, mỗi kỹ năng là một món hàng; có **2.430 giỏ** từ 2 kỹ năng trở lên, ma trận 2.430 × 121.

**Bốn thước đo.** Support cho biết tổ hợp phổ biến đến đâu, Confidence cho biết có A thì bao nhiêu phần trăm có B, Lift so với mức ngẫu nhiên, và Conviction. Ví dụ tính tay luật *docker ⇒ kubernetes*: support 335/2.430 = **13,79%**, confidence 335/538 = **62,3%**, lift **3,33**. Chiều ngược lại *kubernetes ⇒ docker* có confidence 73,6%.

**Lọc luật.** Nhóm khai phá được **4.018 luật thô** từ 856 tập phổ biến, lọc bằng bốn ngưỡng (support ≥ 0,03; confidence ≥ 0,5; lift > 1,2; conviction ≥ 1,2) còn 1.228 luật, rồi bỏ 201 luật dư thừa, còn **1.027 luật**. Chẳng hạn *python ⇒ sql* bị loại vì lift chỉ 1,14.

**Apriori và FP-Growth.** Hai thuật toán cho cùng **856 tập phổ biến**. Apriori nhanh hơn (0,102 giây so với 0,279 giây) vì dữ liệu nhỏ nên chi phí dựng cây FP-tree không đáng. Nhóm chỉ đo một lần nên không kết luận mạnh.

**Kết quả đọc được:**

- Luật mạnh thường chuyên biệt: **90,8%** luật có support dưới 6%, và 62 luật có lift ≥ 8.
- Chỉ **8 luật** vừa phổ biến (support ≥ 10%) vừa đáng tin (confidence ≥ 70%), như *scrum ⇒ agile* 89,9% và *google cloud ⇒ aws* 87,1%.
- Heatmap lift cho thấy khối **DevOps/Cloud gắn kết chặt nhất**; còn Python và SQL phổ biến nhưng ít đi kèm với kỹ năng khác.
- Lift cao nhất là *android ⇄ ios* (**16,0**) và *{html, react} ⇒ css* (13,3). Confidence 100% như *spring ⇒ java* chỉ là quan hệ hiển nhiên giữa framework và ngôn ngữ.

**Độ tin cậy.** Khi đổi mẫu số sang 2.843 tin, **1.225/1.228 luật** vẫn đạt ngưỡng. Hạn chế là luật chỉ mô tả đồng xuất hiện, không chứng minh nhân quả. Bốn điều rút ra: DevOps/Cloud gắn kết chặt nhất; Python và SQL ít gắn kết; confidence 100% chủ yếu là hiển nhiên; luật mạnh thì chuyên biệt.

Em xin nhường lại cho bạn Quân.

---

## Phần 5 · Bước 2.5: Phân lớp khoảng lương (Slide 40–49) · ~2 phút · *Quân*

Em xin cảm ơn Thành. Câu hỏi Q5: có dự đoán được khoảng lương từ kỹ năng và kinh nghiệm không?

**Dữ liệu và nhãn.** Từ 3.135 tin, có **688 tin** trong phạm vi IT đủ điều kiện lương. Nhãn chia ba nhóm: dưới 15 triệu (157 tin), 15–30 triệu (**323 tin**, 46,95%), và trên 30 triệu (208 tin). Mô hình "luôn đoán lớp đông nhất" chỉ đạt Macro F1 **0,2135**, đây là mốc so sánh.

**Đặc trưng và chống rò rỉ.** Mô hình chỉ học từ kỹ năng (multi-hot) và số năm kinh nghiệm. Nhóm cố tình *không* đưa vào lương, `salary_status`, chức danh hay mô tả công việc vì các cột này chứa gần như đáp án. Khi chia tập, nhóm dùng StratifiedGroupKFold: **550 tin train, 138 tin test**, và các tin cùng nhóm hoặc cùng mô tả được giữ chung một phía.

**Cắt tỉa cây quyết định.** Cây đầy đủ có **248 lá**, overfit mạnh. Sau cắt tỉa bằng `ccp_alpha` còn **6 lá**, Macro F1 test lại *tăng* từ 0,5368 lên **0,5758**, và chênh lệch train–test giảm từ 0,421 xuống 0,068.

**So sánh mô hình.** Random Forest đạt Macro F1 test **0,6000**, cây cắt tỉa 0,5758, đều gấp khoảng 2,7 lần Dummy. Nhưng nhóm *không* kết luận Random Forest tốt hơn: điểm CV chỉ hơn 0,000016, nhỏ hơn nhiều độ lệch chuẩn giữa các fold, trong khi chênh lệch train–test của RF là 0,2171, gấp hơn ba lần cây cắt tỉa.

**Đọc cây.** Cây cắt tỉa chỉ dùng kinh nghiệm với hai mốc: **2 năm** chuyển sang nhóm 15–30 triệu, **4 năm** trở lên chuyển sang nhóm trên 30 triệu.

**Lỗi.** Mô hình sai 55/138 tin test. Recall nhóm giữa chỉ **47,7%**, và lỗi chủ yếu xảy ra giữa hai nhóm liền kề. Khi đọc lại các tin sai, nhóm thấy nguyên nhân thường nằm ở nhãn lương đáng ngờ chứ không phải thuật toán – ví dụ một tin Fresher có lương 35 triệu nhưng thực ra làm việc ở Peru, lương tính bằng USD.

---

## Kết luận (Slide 50–54) · ~1,5 phút · *Quân*

**Trả lời năm câu hỏi:**

| Câu hỏi | Kết quả chính | Mức tin cậy |
|---|---|---|
| Q1 | Fresher 13,5 triệu → Senior 37,5 triệu; Hà Nội 22,5, TP.HCM 21 | Trung bình (lệch TopCV, Đà Nẵng 10 tin) |
| Q2 | Python 32,08%, SQL 28,21%, CI/CD 25,01% | Cao |
| Q3 | 8 phân khúc kỹ năng, K = 8 | Tên cụm bền, ranh giới mềm |
| Q4 | 1.027 luật, DevOps/Cloud gắn kết nhất | Cao (1.225/1.228) |
| Q5 | Macro F1 0,5758–0,6000, gấp 2,7 lần Dummy | Thấp–trung bình |

**Khuyến nghị cho sinh viên IT.** Một, học nền tảng chung: Python, SQL, CI/CD và Git. Hai, học kỹ năng theo cặp, như Kubernetes đi với Docker, Spring với Java, Android với iOS. Ba, kinh nghiệm 2 năm và 4 năm là hai mốc chuyển nhóm lương – chỉ là xu hướng. Bốn, tiếng Anh xuất hiện trong khoảng một nửa số tin. Xin lưu ý khuyến nghị này suy ra từ cách nhà tuyển dụng mô tả công việc, không chứng minh học thêm một kỹ năng sẽ tăng lương.

**Hạn chế.** Dữ liệu chỉ cào một đợt (17/11/2026), TopCV chiếm 68% tập lương, còn **266 tin** chờ rà soát, kỹ năng trích bằng từ điển, và tập test chỉ 138 tin.

**Hướng phát triển.** Rà soát xong 266 tin còn lại, kiểm chứng giả định lương tháng của TopCV, bổ sung đặc trưng như cụm kỹ năng, nhóm nghề, thành phố, cào lặp lại theo tháng, và xây gợi ý lộ trình học "đã biết A thì nên học B" từ luật kết hợp.

## Lời cảm ơn (Slide 54) · ~15 giây

Nhóm 40 xin cảm ơn thầy và các bạn đã lắng nghe. Chúng em sẵn sàng trả lời các câu hỏi.

---

### Gợi ý câu hỏi phản biện có thể gặp

- **Vì sao chọn K = 8 dù Silhouette thấp?** Vì Silhouette < 0,1 ở mọi K do kỹ năng chung; nhóm dùng thêm tiêu chí cụm nhỏ nhất ≥ 5% và ARI ≥ 0,80, K = 8 là K nhỏ nhất giữ đủ 8 hồ sơ kỹ năng.
- **Vì sao không nói Random Forest tốt hơn?** Chênh lệch CV 0,000016 nhỏ hơn độ lệch chuẩn giữa các fold, và RF overfit hơn (0,2171 so với 0,068).
- **Kết quả có đại diện cho toàn thị trường không?** Chưa: một đợt cào, nghiêng về TopCV, lương chỉ có ở 24% tin.
- **Vì sao `python ⇒ sql` bị loại?** Lift 1,14 < 1,2 và confidence 36,8% < 50%.
