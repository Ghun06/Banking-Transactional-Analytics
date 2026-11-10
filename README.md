# 🏦 Banking Transaction Analytics: Phân tích dữ liệu giao dịch ngân hàng

## 🎯 1. Bối cảnh & Mục tiêu dự án

Dự án đóng vai một **Chuyên viên phân tích dữ liệu ngân hàng**, thực hiện phân tích toàn diện trên bộ dữ liệu mô phỏng giao dịch nội bộ gồm **20.000 giao dịch** của **8.025 khách hàng** tại **8 thành phố** (khoảng thời gian **01/01/2023 → 20/05/2025**). Dữ liệu trải rộng trên 5 nhóm sản phẩm (Checking, Savings, Credit Card, Loan, Mortgage), 4 kênh giao dịch (Mobile, Branch, ATM, Online) và 3 phân khúc thu nhập.

Thay vì chỉ mô tả số liệu, dự án kiểm định xem mỗi "khác biệt" có **thực sự có ý nghĩa thống kê** hay chỉ là nhiễu, từ đó tìm ra những đòn bẩy thật sự của ngân hàng.

**Mục tiêu chính:**

* 👥 **Thấu hiểu hành vi phân khúc khách hàng:** Phân khúc nào năng nổ nhất, sản phẩm nào phổ biến, điểm tín dụng có dự báo được hành vi hay không.

* 💳 **Phân tích hành vi giao dịch theo loại, khu vực và kênh:** Tìm "điểm nóng", điểm kém hiệu quả và sự khác biệt giữa các kênh.

* 💰 **Giải mã cấu trúc doanh thu phí & chi phí:** Nguồn phí nào đóng góp nhiều nhất, nhóm khách nào đang chịu gánh nặng phí không tương xứng.

* 📈 **Đánh giá xu hướng & chất lượng cá nhân hóa:** Mùa vụ, mức độ phù hợp của offer, khả năng giữ chân và các xu hướng mới nổi.

## 💻 2. Techstack (Công cụ sử dụng)

Dự án được xây dựng trên **Google Colab** với hệ sinh thái Python cho khoa học dữ liệu:

* **Ngôn ngữ:** `Python` 🐍

* **Xử lý dữ liệu:** `pandas`, `numpy`

* **Thống kê & kiểm định:** `scipy` (Spearman, χ², Kruskal-Wallis, hồi quy tuyến tính)

* **Khai phá luật kết hợp:** `mlxtend` (Apriori, Association Rules)

* **Trực quan hóa dữ liệu:**

  * `matplotlib` & `seaborn`: Biểu đồ tĩnh, heatmap, Pareto, boxplot.

  * `plotly`: Sankey, Funnel và bản đồ hiệu suất tương tác.

* **Công cụ hỗ trợ:** `Excel` (dữ liệu nguồn), `Jupyter Notebook / Google Colab` 📓

### 🧭 Phương pháp phân tích áp dụng

| Phương pháp | Mục đích sử dụng |
|---|---|
| **Thống kê mô tả & kiểm định** | So sánh phân khúc, kênh, loại giao dịch; xác nhận khác biệt có ý nghĩa hay không |
| **RFM** | Phân nhóm khách hàng theo Recency – Frequency – Monetary |
| **Pareto 80/20** | Xác định loại giao dịch, loại phí, sản phẩm đóng góp chính |
| **ABC** | Xếp hạng thành phố và điểm phục vụ (Thành phố × Kênh) |
| **Burden Index** | Đo mức độ chịu phí vượt tỷ trọng giá trị giao dịch |
| **Market Basket Analysis** | Tìm các nhóm sản phẩm hay đi cùng nhau để bán chéo |
| **Funnel** | Đo ma sát phí lặp lại và mức độ bán chéo (product holdings) |
| **Sankey** | Trực quan dòng chảy Phân khúc → Sản phẩm → Offer, Kênh → Loại giao dịch |
| **Cohort** | Đo khả năng giữ chân khách theo tháng giao dịch đầu tiên |

## 💡 3. Insight Chiến lược (Key Insights)

### 👥 3.1. Thông tin chuyên sâu & Phân khúc khách hàng

* **Middle Income là phân khúc năng nổ nhất:** chiếm **44,4%** số giao dịch (8.885 GD) và **~1,56 GD/khách** (High: 1,43; Low: 1,27). Giá trị trung bình mỗi giao dịch giữa các phân khúc gần như bằng nhau (~4.900–5.100), nghĩa là khác biệt nằm ở *số lượng khách/giao dịch* chứ không phải "độ lớn" giao dịch.

* **RFM cho thấy sự tập trung giá trị:** nhóm **Champions** chỉ chiếm **19,6%** khách nhưng tạo **34,8%** giá trị giao dịch. Nhóm **At Risk** (12,2% khách, ~17,9% giá trị, trung bình ~447 ngày không giao dịch) là mục tiêu win-back có ROI cao nhất.

* **Sản phẩm phổ biến gần như hòa giữa các phân khúc:** mỗi nhóm sản phẩm chiếm ~19–21% trong mọi phân khúc (χ² p = 0,20 → *không có khác biệt có ý nghĩa*).

* **Điểm tín dụng không dự báo được hành vi:** Spearman ρ ≈ 0 (p từ 0,56 đến 0,89) giữa điểm tín dụng với tần suất giao dịch lẫn tổng phí tích lũy. Mọi nhóm điểm đều có ~2,4–2,6 GD/khách và ~50% giao dịch phát sinh phí.

### 💳 3.2. Hành vi giao dịch: Một hệ thống rất "đồng đều"

* **Không có loại giao dịch nào thống trị:** 6 loại giao dịch chia đều **16,5–17,0%** cả về số lượng lẫn giá trị. Quy luật 80/20 không xuất hiện (cần tới 5/6 loại mới đạt 80%; Kruskal-Wallis p = 0,72).

* **Điểm nóng theo khu vực:** `Barcelona`, `Malaga`, `Murcia` dẫn đầu (hạng A). `Madrid` thấp nhất (12,18 triệu, thấp hơn Barcelona ~7%) và cùng `Bilbao` thuộc hạng B. Ở mức điểm phục vụ (Thành phố × Kênh): 22 điểm hạng A, 7 hạng B và **3 điểm hạng C** gồm `Seville | Online`, `Barcelona | ATM`, `Madrid | Online`.

* **Các kênh được dùng theo cùng một cách:** Mobile dẫn đầu số lượng (26,1%), ATM có giá trị trung bình cao nhất (~5.141), nhưng mọi kiểm định χ²/Kruskal đều không có ý nghĩa (p từ 0,19 đến 0,77). Không có "kênh chuyên rút tiền" hay "kênh chuyên chuyển khoản".

### 💰 3.3. Cấu trúc doanh thu phí & Gánh nặng chi phí

* **Doanh thu phí tập trung cực mạnh vào Loan Payment:** tổng doanh thu phí ~**637 nghìn**, trong đó `LatePaymentAmount` chiếm **52,3%**, `InsuranceFees` 31,3%, `CreditCardFees` 16,4%. Riêng giao dịch **Loan Payment tạo ~60,2%** tổng phí.

* **"Ma sát" lớn nhất cũng là nguồn tiền lớn nhất:** khoản phí trên Loan Payment trung bình **~98,9/lần**, gấp ~2 lần phí bảo hiểm (~49,9) và ~3,9 lần phí thẻ (~25,6). Đáng chú ý, **100% giao dịch Loan Payment đều có khoản phí này** → đây là cơ chế tính phí gắn với loại giao dịch hơn là hiện tượng trễ hạn ngẫu nhiên.

* **Gánh nặng phí mang tính lũy thoái:** phí/giao dịch gần như phẳng giữa các phân khúc (~31–32) nhưng **phí/thu nhập tháng của Low Income là 1,68% so với 0,38% của High Income (gấp ~4,4 lần)**.

* **Sản phẩm chịu phí vượt tỷ trọng:** `Loan` (Burden Index **1,65**) và `Credit Card` (**1,36**); ngược lại `Mortgage` chỉ 0,32. Điểm tín dụng và hạng sản phẩm không tạo khác biệt đáng kể.

* **Ma sát lặp lại ở cấp khách hàng:** 8.025 KH → 6.082 KH từng bị tính phí (75,8%) → 2.778 KH bị tính phí ≥2 lần (34,6%) → 938 KH bị ≥3 lần (11,7%). Khoản phí lớn nhất trên Loan Payment chủ yếu đánh vào khách *một lần* (chỉ 6,3% khách có Loan Payment ≥2 lần) nên nguồn thu này mang tính một lần hơn là bền vững.

### 📈 3.4. Xu hướng, Offer & Giữ chân khách hàng

* **Không có tính mùa vụ đáng kể:** chỉ số mùa vụ dao động hẹp **96–102** (cao nhất tháng 4, 10, 11; thấp nhất tháng 5, 2, 8), Kruskal-Wallis p = 0,77. Không nên lập kế hoạch nhân sự/thanh khoản theo mùa dựa trên dữ liệu này.

* **Offer được gán theo quy tắc cố định, chưa dựa trên hành vi:** mỗi cặp (Phân khúc × Nhóm sản phẩm) ứng với đúng 1 trong 7 offer. **91,5% khách có ≥2 giao dịch nhận ≥2 offer khác nhau** (trung bình 2,46 offer/khách) → trải nghiệm cá nhân hóa không nhất quán. Chỉ ~**49,3%** giao dịch có offer khớp chủ đề với sản phẩm đang dùng; `No-Fee Basic Account` khớp 0%.

* **Không có combo bán chéo tự nhiên:** các cặp nhóm sản phẩm có lift **0,89–0,91 (< 1)**; ở cấp hạng sản phẩm lift tối đa chỉ ~1,18 với confidence ~11–12%.

* **Mức độ bán chéo thấp:** 8.025 KH → 5.287 dùng ≥2 nhóm SP (65,9%) → 2.266 dùng ≥3 nhóm → chỉ **41 khách (0,5%)** dùng đủ 5 nhóm.

* **Giữ chân là điểm yếu lớn nhất:** retention gộp chỉ **~7,7% sau 1 tháng**, ~8,5% sau 6 tháng và gần như phẳng theo thời gian; **27,1% khách chỉ giao dịch đúng 1 lần**.

* **Tín hiệu mới nổi (cần theo dõi, chưa khẳng định):** so sánh cùng kỳ Jan–Apr 2025 vs 2024, giao dịch **+3,7%**, giá trị **+5,6%**, doanh thu phí **+4,1%**. Nổi lên: `Mortgage` +12,1%, `Loan` +6,5%, kênh `Online` +6,6%; doanh thu phí của nhóm `Low Income` +13,8%. Tuy nhiên mỗi ô chỉ ~600–1.300 giao dịch nên các chênh lệch gần ngưỡng nhiễu.

### 📝 3.5. Bảng trả lời nhanh 12 câu hỏi của thử thách

| # | Câu hỏi | Trả lời ngắn gọn |
|---|---|---|
| 1.1 | Phân khúc năng nổ nhất | **Middle Income** (44% GD, ~1,56 GD/khách) |
| 1.2 | Sản phẩm phổ biến theo phân khúc | Đồng đều ~20%, không khác biệt ý nghĩa |
| 1.3 | Điểm tín dụng vs tần suất/phí | **Không** có tương quan (ρ ≈ 0) |
| 2.1 | Loại giao dịch chiếm ưu thế | **Không có** (6 loại chia đều ~16,5–17%) |
| 2.2 | Điểm nóng / kém hiệu quả | Nóng: Barcelona, Malaga, Murcia; kém tương đối: Madrid, Bilbao |
| 2.3 | Khác biệt giữa các kênh | **Không có** khác biệt ý nghĩa |
| 3.1 | Loại GD sinh nhiều phí nhất | **Loan Payment** (~60% doanh thu phí) |
| 3.2 | Nhóm chịu chi phí không tương xứng | **Low Income** (phí/thu nhập gấp ~4,4 lần High); sản phẩm Loan & Credit Card |
| 3.3 | Ma sát nhưng sinh doanh thu | Khoản phí trên **Loan Payment** (~98,9/lần) |
| 4.1 | Mùa vụ | **Không có** mùa vụ đáng kể |
| 4.2 | Offer có phù hợp hành vi? | **Một phần**: khớp thu nhập/sản phẩm, chưa dựa trên hành vi |
| 4.3 | Xu hướng mới nổi | Retention phẳng ~8%/tháng; theo dõi Mortgage, Online/Branch, phí Low Income |

## 🚀 4. Khuyến nghị chiến lược (Recommendations)

### 🔴 1. Win-back nhóm khách giá trị cao đã rời bỏ (Marketing & CRM) — *Ưu tiên cao*

* Triển khai chiến dịch ưu đãi cá nhân hóa cho nhóm **At Risk** (~980 khách, ~18% giá trị giao dịch).

* Xây chương trình kéo khách quay lại sau giao dịch đầu tiên để cải thiện retention ~8%/tháng và giảm tỷ lệ 27% khách chỉ giao dịch 1 lần.

### 🔴 2. Rà soát cơ chế phí trên Loan Payment (Product & Compliance) — *Ưu tiên cao*

* Đa dạng hóa nguồn thu phí, giảm sự phụ thuộc vào một cặp duy nhất `LatePayment | Loan Payment` (52,3% tổng phí) để hạn chế rủi ro uy tín/pháp lý.

* Cân nhắc **trần phí hoặc ưu đãi phí** cho nhóm Low Income và khách có Loan, vì phí/thu nhập của nhóm này cao gấp ~4,4 lần nhóm High Income.

### 🟠 3. Xây lại logic offer theo hành vi (Data & Marketing) — *Ưu tiên trung bình*

* Chuyển từ quy tắc (Phân khúc × Sản phẩm) sang logic dựa trên **recency, tần suất, phí đã chịu và sản phẩm đang thiếu**.

* Cố định offer theo **khách hàng** thay vì theo từng giao dịch để trải nghiệm nhất quán; A/B test offer theo hành vi với offer theo quy tắc hiện tại.

### 🟠 4. Chiến dịch bán chéo có mục tiêu (Sales) — *Ưu tiên trung bình*

* Nhắm tới **2.738 khách** mới chỉ dùng 1 nhóm sản phẩm; vì dữ liệu không cho thấy combo tự nhiên (lift < 1.2) nên nên bán chéo theo nhu cầu/giai đoạn sống thay vì theo cặp sản phẩm cố định.

### 🟡 5. Tối ưu vận hành & theo dõi tín hiệu mới (Operations) — *Theo dõi*

* Rà soát các điểm phục vụ hạng C: `Seville | Online`, `Barcelona | ATM`, `Madrid | Online`. Chênh lệch giữa các thành phố chỉ ~7% nên chỉ cần rà soát điểm yếu, không cần tái cấu trúc mạng lưới.

* Vì khách dùng các kênh theo cùng một cách, có thể đẩy mạnh chuyển dịch sang kênh số (Mobile/Online) để giảm tải Branch/ATM mà ít rủi ro đổi trải nghiệm theo loại nghiệp vụ.

* Theo dõi sát `Mortgage`, kênh `Online`/`Branch` và doanh thu phí của nhóm `Low Income` trong các kỳ tiếp theo.

## ⚠️ 5. Hạn chế của phân tích

* `CustomerSegment`, `MonthlyIncome`, `CustomerScore` **thay đổi giữa các giao dịch** của cùng một `CustomerID` nên được coi là thuộc tính *tại thời điểm giao dịch*.

* Dữ liệu chỉ có `BranchCity` (không có `BranchID`) và không có trạng thái giao dịch, vì vậy Funnel được điều chỉnh sang **funnel phí** và **product holdings**.

* Có 2 đơn vị tiền tệ (EUR, USD) nhưng không có tỷ giá nên **không quy đổi** (có thể bật `NORMALIZE_CURRENCY` trong notebook nếu có tỷ giá).

* Tháng **05/2025 chỉ có dữ liệu đến 20/05** nên bị loại khỏi phân tích mùa vụ/xu hướng.

* Bảng ánh xạ `OFFER_TARGET` (offer ↔ sản phẩm liên quan) là **giả định của người phân tích**, có thể chỉnh theo nghiệp vụ thực tế.

* Phần lớn khác biệt nhỏ chưa vượt ngưỡng ý nghĩa thống kê; các số liệu YoY dựa trên cỡ mẫu nhỏ.

---

*Dự án được thực hiện bởi Hung (HagaNeil) - Hanoi 2026* 🖋️
