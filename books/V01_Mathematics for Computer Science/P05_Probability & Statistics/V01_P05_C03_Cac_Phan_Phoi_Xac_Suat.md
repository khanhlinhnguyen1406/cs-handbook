# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part V — Probability & Statistics
## Chương 5.3 — Các Phân phối Xác suất Quan trọng
### (Probability Distributions)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 5.3 |
| Thuộc Part | V — Probability & Statistics |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 60–70 phút |
| Độ khó | ★★★☆☆ |
| Kiến thức tiên quyết | Chương 5.1 — Introduction to Probability; Chương 5.2 — Random Variables (PMF, PDF, CDF) |
| Chương liên quan | 5.4 — Expectation and Variance (tính kỳ vọng, phương sai cho từng phân phối ở chương này); Volume 5, Part II (Naive Bayes, Logistic Regression dùng trực tiếp Bernoulli và Gaussian) |
| Từ khóa | Bernoulli, Binomial, Poisson, Uniform distribution, Gaussian distribution, Normal distribution, Exponential distribution |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Nhận diện tình huống kỹ thuật nào nên mô hình hóa bằng phân phối Bernoulli, Binomial, Poisson, Uniform, Gaussian, hoặc Exponential.
- Viết đúng công thức PMF/PDF của từng phân phối và giải thích ý nghĩa của các tham số.
- Giải thích mối quan hệ toán học giữa các phân phối (Bernoulli → Binomial → Poisson; Uniform → mọi phân phối qua Inverse Transform Sampling).
- Sinh mẫu ngẫu nhiên từ mỗi phân phối bằng thư viện chuẩn, và trực quan hóa hình dạng PMF/PDF tương ứng.
- Bước đầu nhận ra vì sao phân phối Gaussian xuất hiện phổ biến đến vậy trong tự nhiên và trong Machine Learning.

---

### Câu hỏi khơi gợi

> *Tại sao hầu hết các thư viện khởi tạo trọng số mạng neural (`torch.nn.init`) đều dùng phân phối Gaussian hoặc Uniform, chứ hiếm khi dùng một phân phối "kỳ lạ" tự nghĩ ra? Điều gì khiến một số ít phân phối trở thành "công cụ mặc định" được tái sử dụng khắp mọi lĩnh vực của Computer Science?*

---

## 1. Tổng quan chương

Chương 5.2 trang bị cho bạn ngôn ngữ tổng quát — PMF, PDF, CDF — để mô tả bất kỳ biến ngẫu nhiên nào. Nhưng trong thực hành, ta hiếm khi tự "phát minh" một phân phối từ đầu; thay vào đó, ta nhận diện tình huống của mình khớp với một trong số **ít phân phối kinh điển** đã được nghiên cứu kỹ lưỡng qua hàng thế kỷ, rồi tái sử dụng toàn bộ lý thuyết đã có sẵn cho phân phối đó.

Chương này giới thiệu sáu phân phối xuất hiện lặp đi lặp lại trong suốt Handbook: ba phân phối rời rạc (**Bernoulli, Binomial, Poisson**) và ba phân phối liên tục (**Uniform, Gaussian, Exponential**). Mỗi phân phối được trình bày theo đúng khung PMF/PDF vừa học, kèm trực giác về **khi nào nên dùng nó**.

> **💡 Insight**
> Sáu phân phối trong chương này không phải một danh sách tùy ý — chúng có quan hệ họ hàng chặt chẽ với nhau (Mục 7), và nắm được các mối quan hệ đó quan trọng hơn nhiều so với việc học thuộc công thức của từng phân phối riêng lẻ.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1713 | Jacob Bernoulli | Phân tích phép thử nhị phân (thành công/thất bại) trong *Ars Conjectandi* — nền tảng của phân phối Bernoulli và Binomial |
| 1733 | Abraham de Moivre | Chứng minh phân phối Binomial, khi số phép thử lớn, xấp xỉ một đường cong hình chuông — phiên bản sơ khai đầu tiên của phân phối Gaussian, gần một thế kỷ trước khi Gauss công bố công trình của mình |
| 1809 | Carl Friedrich Gauss | Dùng đường cong hình chuông để mô hình hóa sai số quan sát thiên văn — phân phối này sau đó mang tên ông (Gaussian / Normal Distribution) |
| 1837 | Siméon Denis Poisson | Công bố phân phối mang tên ông trong nghiên cứu về xác suất trong luật pháp — mô tả số sự kiện hiếm xảy ra trong một khoảng thời gian/không gian cố định |
| 1901 | Louis Bachelier | Ứng dụng phân phối liên tục (bao gồm ý tưởng gần với Exponential) để mô hình hóa biến động giá tài chính — một trong những ứng dụng công nghiệp sớm nhất của xác suất liên tục |

Điều thú vị: phân phối Gaussian được khám phá **hai lần độc lập**, cách nhau gần 80 năm (de Moivre 1733, Gauss 1809), trong hai bối cảnh hoàn toàn khác nhau (xấp xỉ nhị thức và sai số đo đạc). Đây là dấu hiệu sớm nhất cho một hiện tượng sâu sắc hơn — **Định lý Giới hạn Trung tâm (Central Limit Theorem)** — mà Handbook sẽ trình bày đầy đủ ở Volume 7, và chỉ xem trước ở Mục 12 của chương này.

---

## 3. Động lực

Xét năm tình huống kỹ thuật, mỗi tình huống khớp tự nhiên với một phân phối khác nhau:

- **Một request có bị lỗi hay không?** — kết quả nhị phân, chỉ hai giá trị {0, 1}. → **Bernoulli**.
- **Trong 100 request độc lập, có bao nhiêu request bị lỗi?** — đếm số "thành công" trong nhiều phép thử Bernoulli lặp lại. → **Binomial**.
- **Trong một phút, có bao nhiêu request đến server?** — đếm số sự kiện hiếm, độc lập, xảy ra trong một khoảng thời gian cố định, không có giới hạn trên rõ ràng. → **Poisson**.
- **Chọn ngẫu nhiên một khóa hash trong không gian 32-bit** — mọi giá trị có khả năng như nhau, không thiên vị. → **Uniform**.
- **Nhiễu đo đạc trong một cảm biến, hoặc sai số dự đoán của một mô hình hồi quy** — tập trung quanh 0, hiếm khi cực đoan, đối xứng. → **Gaussian**.
- **Thời gian giữa hai lần request liên tiếp đến server** — thời gian chờ cho một sự kiện hiếm tiếp theo. → **Exponential**.

Không có bộ phân phối chuẩn hóa này, mỗi bài toán trên sẽ phải được mô hình hóa từ đầu bằng PMF/PDF tùy chỉnh (như đã làm ở Chương 5.2). Có bộ phân phối này, ta chỉ cần **nhận diện đúng khuôn mẫu** và tra cứu công thức, tính chất đã được nghiên cứu sẵn.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Sáu phân phối trong chương này giống như **sáu "khuôn đúc" (mold) đã được kiểm định kỹ**, mỗi khuôn phù hợp với một dạng bất định cụ thể trong thế giới thực. Thay vì tự tay nặn một hình dạng phân phối mới cho mỗi bài toán, kỹ sư giàu kinh nghiệm nhận diện bài toán của mình khớp với khuôn nào, rồi áp dụng trực tiếp toàn bộ lý thuyết đã có sẵn cho khuôn đó.

| Trực giác kỹ thuật bạn đã có | Phân phối tương ứng |
|---|---|
| Cờ boolean `success: true/false` | Bernoulli |
| Đếm số dòng log chứa lỗi trong N dòng | Binomial |
| Số lượng exception ném ra trong 1 giờ chạy production | Poisson |
| `random.randint(0, 100)` hoặc UUID sinh ngẫu nhiên | Uniform |
| Đường cong "hình chuông" trong biểu đồ phân phối điểm thi, chiều cao con người | Gaussian |
| Thời gian chờ trung bình trước khi một server timeout | Exponential |

---

## 5. Trực quan hóa khái niệm

**Hình 5.3.1 — Hình dạng PMF/PDF của sáu phân phối**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
Bernoulli(p)         Binomial(n,p)         Poisson(λ)
   │ █                   │   ▄               │  ▄
   │ █    █               │  ██▄              │ ▄██▄
   └─────────  x          └──────────  x       └────────  x
   0      1              0  1  2  3 ...       0  1  2  3 ...

Uniform(a,b)         Gaussian(μ,σ²)        Exponential(λ)
   │┌────┐               │   ╱╲               │█
   ││    │                │  ╱  ╲              │ █▄
   └┴────┴──  x            └──────  x           └───▄▄▄──  x
   a      b                   μ                 0
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | So sánh trực quan hình dạng của cả 6 phân phối cạnh nhau — quan sát ba dạng rời rạc lệch dần từ đối xứng (Binomial với $p=0.5$) sang lệch phải (Poisson với $\lambda$ nhỏ), và ba dạng liên tục từ phẳng đều (Uniform) sang hình chuông (Gaussian) sang giảm dần theo hàm mũ (Exponential) |
| Điểm mấu chốt | Binomial với $n$ lớn "trông giống" Gaussian (xem Mục 7, xấp xỉ de Moivre–Laplace); Poisson với $\lambda$ lớn cũng "trông giống" Gaussian — cả hai đều là biểu hiện sớm của Định lý Giới hạn Trung tâm (Mục 12) |

---

## 6. Định nghĩa hình thức

### 6.1 Ba phân phối Rời rạc

> **📌 Remember — Bernoulli, Binomial, Poisson**
>
> **Bernoulli($p$):** mô hình một phép thử nhị phân duy nhất, với xác suất "thành công" là $p \in [0,1]$.
> $$p_X(x) = p^x (1-p)^{1-x}, \quad x \in \{0, 1\}$$
>
> **Binomial($n, p$):** mô hình tổng số thành công trong $n$ phép thử Bernoulli($p$) **độc lập** (Chương 5.1, Mục 6).
> $$p_X(x) = \binom{n}{x} p^x (1-p)^{n-x}, \quad x \in \{0, 1, \dots, n\}$$
>
> **Poisson($\lambda$):** mô hình số sự kiện hiếm, độc lập, xảy ra trong một khoảng cố định, với tốc độ trung bình $\lambda$.
> $$p_X(x) = \frac{\lambda^x e^{-\lambda}}{x!}, \quad x \in \{0, 1, 2, \dots\}$$

Ký hiệu $\binom{n}{x}$ (đọc "n chọn x") là **hệ số tổ hợp**, đã học ở Part II — Discrete Mathematics, đếm số cách chọn $x$ phần tử thành công trong tổng số $n$ phép thử, không phân biệt thứ tự.

### 6.2 Ba phân phối Liên tục

> **📌 Remember — Uniform, Gaussian, Exponential**
>
> **Uniform($a, b$):** mọi giá trị trong khoảng $[a,b]$ có mật độ như nhau.
> $$f_X(x) = \frac{1}{b-a}, \quad x \in [a, b]$$
>
> **Gaussian / Normal($\mu, \sigma^2$):** phân phối hình chuông, đối xứng quanh trung bình $\mu$, độ "phân tán" đo bằng $\sigma^2$.
> $$f_X(x) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right), \quad x \in \mathbb{R}$$
>
> **Exponential($\lambda$):** mô hình thời gian chờ đến sự kiện hiếm tiếp theo, với tốc độ $\lambda$.
> $$f_X(x) = \lambda e^{-\lambda x}, \quad x \geq 0$$

---

## 7. Nền tảng toán học

### 7.1 Bernoulli là "viên gạch" của Binomial

- **Ý nghĩa:** Binomial không phải một phân phối độc lập — nó chính là **tổng của $n$ biến ngẫu nhiên Bernoulli độc lập**.
- **Ví dụ đơn giản:** tung đồng xu 3 lần độc lập (mỗi lần là một Bernoulli(0.5)), tổng số lần Ngửa tuân theo Binomial(3, 0.5) — chính là ví dụ ở Hình 5.2.1, Chương 5.2.

> **📦 Formula Box — Binomial như Tổng các Bernoulli**
>
> $$X = \sum_{i=1}^{n} Y_i, \quad Y_i \overset{\text{i.i.d.}}{\sim} \text{Bernoulli}(p) \implies X \sim \text{Binomial}(n, p)$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $Y_i$ | Kết quả (0 hoặc 1) của phép thử Bernoulli thứ $i$ |
> | i.i.d. | Viết tắt "independent and identically distributed" — độc lập và có cùng phân phối, một giả định then chốt xuyên suốt phần còn lại của Handbook |
> | **Diễn giải kỹ thuật** | Hệ số tổ hợp $\binom{n}{x}$ trong PMF của Binomial (Mục 6.1) chính là số cách chọn **vị trí nào** trong $n$ phép thử là "thành công" — một ứng dụng trực tiếp của kỹ thuật đếm từ Part II |
> | **Ứng dụng thường gặp** | Mô hình hóa tỷ lệ chuyển đổi (conversion rate) trong A/B testing như tổng của nhiều lượt truy cập độc lập |

### 7.2 Từ Binomial đến Poisson — Giới hạn Sự kiện Hiếm

Khi $n$ rất lớn nhưng $p$ rất nhỏ, sao cho tích $np = \lambda$ giữ cố định (nghĩa là số "thành công" kỳ vọng không đổi dù số phép thử tăng vô hạn), Binomial($n,p$) hội tụ về Poisson($\lambda$):

> **📦 Formula Box — Xấp xỉ Poisson của Binomial**
>
> $$\lim_{n \to \infty, \, np = \lambda} \binom{n}{x} p^x (1-p)^{n-x} = \frac{\lambda^x e^{-\lambda}}{x!}$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $n \to \infty$, $p \to 0$, $np = \lambda$ | "Rất nhiều cơ hội xảy ra sự kiện, nhưng mỗi cơ hội có xác suất cực nhỏ" — đúng bản chất của số request đến server trong một giây: có vô số khoảnh khắc micro-giây có thể có request đến, nhưng xác suất tại mỗi khoảnh khắc cực nhỏ |
> | **Diễn giải kỹ thuật** | Đây là lý do Poisson được dùng cho "số sự kiện hiếm trong một khoảng cố định" thay vì Binomial trực tiếp — không cần biết chính xác "số phép thử" tiềm ẩn là bao nhiêu, chỉ cần biết tốc độ trung bình $\lambda$ |
> | **Ứng dụng thường gặp** | Mô hình hóa số request/giây, số lỗi phần cứng/năm, số cuộc gọi đến tổng đài/giờ |

### 7.3 Từ Poisson đến Exponential — Hai mặt của cùng một Quá trình

Nếu số sự kiện trong một khoảng thời gian tuân theo Poisson($\lambda$), thì **thời gian chờ** giữa hai sự kiện liên tiếp tuân theo Exponential($\lambda$) — cùng một tham số tốc độ $\lambda$, chỉ khác góc nhìn: "đếm số sự kiện" (Poisson, rời rạc) so với "đo thời gian chờ" (Exponential, liên tục).

> **⚠️ Common Mistake**
> Nhầm lẫn phổ biến: nghĩ rằng Exponential và Poisson là hai mô hình cạnh tranh cho cùng một bài toán. Trên thực tế, chúng là **hai mặt bổ sung** của cùng một hiện tượng — gọi chung là **Quá trình Poisson (Poisson Process)** — và luôn xuất hiện cùng nhau trong các bài toán hàng đợi (queueing), như sẽ thấy ở Dự án nhỏ (Mục 18).

---

## 8. Thuật toán / Cơ chế

**Sinh mẫu từ mỗi phân phối** — thay vì tự triển khai Inverse Transform Sampling cho từng phân phối (như đã làm thủ công ở Chương 5.2, Mục 8–9 cho Exponential), thực hành công nghiệp luôn dùng thư viện đã được kiểm định — nguyên tắc "Tool Selection" đã nêu ở `TOOLS.md`: ưu tiên công cụ trưởng thành, được cộng đồng rộng rãi tin dùng.

```text
Bước 1 — Xác định phân phối phù hợp với bài toán (theo Mục 3–4)
        │
        ▼
Bước 2 — Xác định tham số của phân phối (p, n, λ, μ, σ, a, b...)
        │
        ▼
Bước 3 — Gọi hàm sinh mẫu tương ứng từ thư viện (numpy.random)
        │
        ▼
Bước 4 — Kiểm chứng mẫu sinh ra bằng cách so sánh thống kê mẫu
         (trung bình, phương sai) với giá trị lý thuyết (Chương 5.4)
```

---

## 9. Triển khai

```python
import numpy as np

rng = np.random.default_rng(seed=42)

# Bernoulli(p=0.3): mô phỏng 1 request có lỗi hay không
bernoulli_samples = rng.binomial(n=1, p=0.3, size=10_000)

# Binomial(n=100, p=0.3): số request lỗi trong mỗi lô 100 request
binomial_samples = rng.binomial(n=100, p=0.3, size=10_000)

# Poisson(lambda=5): số request đến server mỗi giây
poisson_samples = rng.poisson(lam=5, size=10_000)

# Uniform(a=0, b=1): sinh khóa băm (hash key) ngẫu nhiên đều
uniform_samples = rng.uniform(low=0.0, high=1.0, size=10_000)

# Gaussian(mu=0, sigma=1): mô phỏng nhiễu đo đạc của một cảm biến
gaussian_samples = rng.normal(loc=0.0, scale=1.0, size=10_000)

# Exponential(lambda=2): thời gian chờ (giây) giữa hai request liên tiếp
exponential_samples = rng.exponential(scale=1 / 2, size=10_000)
```

> **⚠️ Common Mistake**
> Chú ý: NumPy tham số hóa Exponential bằng `scale = 1/λ` (nghịch đảo tốc độ), không phải trực tiếp $\lambda$ như định nghĩa PDF ở Mục 6.2. Đây là lỗi rất phổ biến khi chuyển từ công thức toán học sang code — luôn kiểm tra tài liệu tham số hóa của thư viện trước khi dùng.

---

## 10. Trực quan hóa quá trình thực thi

**Kiểm chứng xấp xỉ Binomial → Poisson (Mục 7.2)**, với $n=1000$, $p=0.005$ (vậy $\lambda = np = 5$), so sánh PMF tại vài giá trị $x$:

| $x$ | PMF Binomial(1000, 0.005) | PMF Poisson(5) | Chênh lệch |
|---:|---:|---:|---:|
| 0 | 0.00657 | 0.00674 | 0.00017 |
| 3 | 0.14037 | 0.14037 | 0.00000 |
| 5 | 0.17600 | 0.17547 | 0.00053 |
| 10 | 0.01798 | 0.01813 | 0.00015 |

Hai PMF gần như trùng khớp — xác nhận Poisson là xấp xỉ rất tốt cho Binomial khi $n$ lớn, $p$ nhỏ, đúng như Formula Box ở Mục 7.2.

**Thống kê mẫu sinh ra từ Mục 9** (10.000 mẫu mỗi phân phối), so sánh trung bình mẫu với trung bình lý thuyết (công thức đầy đủ ở Chương 5.4):

| Phân phối | Trung bình mẫu | Trung bình lý thuyết |
|---|---:|---:|
| Bernoulli(0.3) | 0.2987 | 0.3 (= $p$) |
| Binomial(100, 0.3) | 29.94 | 30 (= $np$) |
| Poisson(5) | 4.998 | 5 (= $\lambda$) |
| Uniform(0,1) | 0.5008 | 0.5 (= $(a+b)/2$) |
| Gaussian(0,1) | -0.0031 | 0 (= $\mu$) |
| Exponential(2) | 0.5019 | 0.5 (= $1/\lambda$) |

Mọi giá trị đều hội tụ sát với lý thuyết — kiểm chứng thực nghiệm bằng chính công cụ Monte Carlo đã học ở Chương 5.1.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Việc nhận diện đúng phân phối cho một bài toán không chỉ là bài tập lý thuyết — nó quyết định trực tiếp công thức nào được dùng để thiết kế hệ thống, đặt ngưỡng cảnh báo, hoặc chọn hàm mất mát trong Machine Learning.

| Phân phối | Ứng dụng công nghiệp tiêu biểu |
|---|---|
| Bernoulli | Nhãn phân loại nhị phân (click/no-click, spam/ham) — hàm mất mát Binary Cross-Entropy (Volume 5) được xây dựng trực tiếp từ PMF Bernoulli |
| Binomial | Ước lượng tỷ lệ lỗi trong kiểm thử phần mềm; tính khoảng tin cậy cho tỷ lệ chuyển đổi A/B test (Chương 5.6) |
| Poisson | Mô hình lưu lượng mạng, số request/giây trong thiết kế hệ thống hàng đợi (queueing systems, Volume 4); dự báo số sự cố phần cứng |
| Uniform | Khởi tạo trọng số mạng neural (`Xavier/Glorot initialization`); sinh khóa băm, salt mật mã, UUID |
| Gaussian | Mô hình nhiễu đo đạc; giả định phân phối lỗi dư (residual) trong hồi quy tuyến tính; khởi tạo trọng số mạng neural; nền tảng của Gaussian Mixture Model và Gaussian Process (Volume 5–6) |
| Exponential | Mô hình thời gian giữa các sự cố (Mean Time Between Failures — MTBF) trong kỹ thuật độ tin cậy (reliability engineering); thời gian phục vụ trong hệ thống hàng đợi |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Vì sao phân phối Gaussian lại xuất hiện ở khắp mọi nơi — chiều cao con người, sai số đo đạc, nhiễu cảm biến, thậm chí cả tổng của nhiều biến ngẫu nhiên hoàn toàn không liên quan đến Gaussian?

Câu trả lời nằm ở **Định lý Giới hạn Trung tâm (Central Limit Theorem — CLT)** — một trong những kết quả sâu sắc nhất của lý thuyết xác suất, sẽ được chứng minh và khảo sát đầy đủ ở Volume 7 (Statistics for Research). Phát biểu trực giác: **tổng (hoặc trung bình) của rất nhiều biến ngẫu nhiên độc lập, cùng phân phối** — bất kể phân phối gốc của chúng là gì — **luôn xấp xỉ phân phối Gaussian**, miễn là số lượng biến đủ lớn.

Đây chính xác là hiện tượng bạn đã thấy thoáng qua ở Mục 2: xấp xỉ Binomial của de Moivre (1733) và phân phối sai số của Gauss (1809) hóa ra là **hai biểu hiện cụ thể của cùng một định lý tổng quát** — Binomial là tổng của nhiều Bernoulli (Mục 7.1), và tổng đó, khi $n$ đủ lớn, hội tụ về Gaussian. Sai số đo đạc thiên văn của Gauss cũng vậy: mỗi sai số nhỏ là tổng hợp của rất nhiều nguồn nhiễu độc lập nhỏ (rung động, nhiệt độ, thị sai người quan sát...), và tổng đó hội tụ về Gaussian bất kể bản chất từng nguồn nhiễu riêng lẻ.

**Ý nghĩa với Computer Science và AI:** CLT là lý do ta có thể **giả định nhiễu trong dữ liệu tuân theo Gaussian** một cách hợp lý trong rất nhiều mô hình thống kê và Machine Learning (hồi quy tuyến tính, Kalman Filter, khởi tạo trọng số mạng neural) — không phải vì Gaussian "đặc biệt thiêng liêng", mà vì nó là **điểm hội tụ tự nhiên** của tổng nhiều yếu tố ngẫu nhiên nhỏ, độc lập.

**Câu hỏi mở** để suy ngẫm: nếu CLT giải thích vì sao Gaussian phổ biến, thì tại sao nhiều hiện tượng thực tế — như độ trễ mạng, kích thước file, tần suất từ trong văn bản — lại tuân theo các phân phối "đuôi nặng" (heavy-tailed distributions, ví dụ Power Law) thay vì Gaussian? Đây là một hướng nghiên cứu tích cực trong cả thống kê lẫn kỹ thuật hệ thống hiện đại.

---

## 13. Ưu điểm

- **Tái sử dụng lý thuyết đã được nghiên cứu kỹ** — thay vì tự xây dựng PMF/PDF từ đầu cho mỗi bài toán, chỉ cần nhận diện đúng "khuôn mẫu" và tra cứu công thức có sẵn.
- **Tham số hóa gọn nhẹ** — mỗi phân phối chỉ cần 1–2 tham số ($p$; $n,p$; $\lambda$; $a,b$; $\mu,\sigma$) để mô tả đầy đủ hành vi, giúp việc ước lượng từ dữ liệu (Chương 5.6, Volume 7) trở nên khả thi.
- **Quan hệ toán học chặt chẽ giữa các phân phối** (Mục 7) cho phép chuyển đổi linh hoạt giữa các góc nhìn (đếm sự kiện vs đo thời gian chờ) tùy theo bài toán cụ thể.
- **Hỗ trợ thư viện rộng rãi** — hầu như mọi ngôn ngữ lập trình đều có hàm sinh mẫu sẵn cho cả sáu phân phối này, giảm thiểu lỗi triển khai thủ công.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Giả định dữ liệu tuân theo một phân phối kinh điển (đặc biệt là Gaussian) mà không kiểm chứng là một trong những sai lầm phổ biến nhất trong phân tích dữ liệu thực tế.

- **Dữ liệu thực tế không phải lúc nào cũng khớp hoàn hảo** với một trong sáu phân phối này — nhiều hiện tượng (độ trễ mạng, kích thước file) có "đuôi nặng" hơn Gaussian dự đoán (xem câu hỏi mở ở Mục 12).
- **Giả định độc lập (i.i.d.)** cần cho Binomial và tổng dẫn đến CLT (Mục 7.1, Mục 12) thường bị vi phạm trong thực tế — ví dụ, các request liên tiếp có thể tương quan với nhau (burst traffic), làm sai lệch mô hình Poisson.
- **Xấp xỉ Poisson (Mục 7.2)** chỉ chính xác khi $n$ đủ lớn và $p$ đủ nhỏ — áp dụng cho $n$ nhỏ sẽ cho sai số đáng kể.
- Chương này chỉ giới thiệu sáu phân phối "kinh điển" — còn rất nhiều phân phối khác quan trọng trong Machine Learning hiện đại (Beta, Dirichlet, Gamma, Categorical đa chiều...) nằm ngoài phạm vi Volume 1, sẽ gặp lại khi cần thiết ở Volume 5–6.

---

## 15. So sánh

**Bảng 5.3.1 — Tổng hợp Sáu Phân phối**

| Phân phối | Loại | Tham số | Miền giá trị | Tình huống điển hình |
|---|---|---|---|---|
| Bernoulli | Rời rạc | $p$ | $\{0,1\}$ | Một phép thử nhị phân đơn lẻ |
| Binomial | Rời rạc | $n, p$ | $\{0,...,n\}$ | Đếm thành công trong $n$ phép thử độc lập |
| Poisson | Rời rạc | $\lambda$ | $\{0,1,2,...\}$ | Đếm sự kiện hiếm trong khoảng cố định |
| Uniform | Liên tục | $a, b$ | $[a,b]$ | Không thiên vị giữa các giá trị trong một khoảng |
| Gaussian | Liên tục | $\mu, \sigma^2$ | $\mathbb{R}$ | Tổng của nhiều yếu tố ngẫu nhiên nhỏ, độc lập (CLT) |
| Exponential | Liên tục | $\lambda$ | $[0,\infty)$ | Thời gian chờ đến sự kiện hiếm tiếp theo |

**Phân tích:** Bảng trên nên được đọc theo hai "chuỗi họ hàng" đã trình bày ở Mục 7, chứ không phải sáu mục độc lập: chuỗi rời rạc Bernoulli → Binomial → Poisson (thu hẹp dần điều kiện áp dụng khi $n \to \infty$), và cặp bổ sung liên tục–rời rạc Poisson ↔ Exponential (cùng một quá trình, hai góc nhìn). Gaussian và Uniform đóng vai trò đặc biệt: Uniform là "nguyên liệu thô" cho Inverse Transform Sampling (Chương 5.2, Mục 8) sinh ra mọi phân phối khác, còn Gaussian là "điểm hội tụ" mà rất nhiều phân phối khác tiệm cận tới khi cộng dồn (Mục 12).

---

## 16. Tóm tắt

- **Bernoulli($p$)** mô hình một phép thử nhị phân; **Binomial($n,p$)** là tổng của $n$ Bernoulli độc lập; **Poisson($\lambda$)** là giới hạn của Binomial khi $n\to\infty$, $p\to 0$, $np=\lambda$ cố định.
- **Uniform($a,b$)** phân bố đều trên một khoảng; **Gaussian($\mu,\sigma^2$)** là đường cong hình chuông, điểm hội tụ tự nhiên của tổng nhiều yếu tố ngẫu nhiên độc lập (CLT); **Exponential($\lambda$)** mô hình thời gian chờ, là "mặt liên tục" bổ sung cho Poisson.
- Sáu phân phối này không độc lập với nhau — chúng liên kết qua các phép biến đổi toán học chặt chẽ (tổng, giới hạn, thời gian chờ).
- Việc sinh mẫu trong thực hành nên dùng thư viện đã kiểm định (`numpy.random`) thay vì tự triển khai Inverse Transform Sampling thủ công cho mỗi phân phối.
- Định lý Giới hạn Trung tâm (xem trước ở Mục 12) là lời giải thích sâu sắc cho sự phổ biến của phân phối Gaussian trong tự nhiên và kỹ thuật.

Chương 5.4 (Expectation and Variance) sẽ định nghĩa các đại lượng tóm tắt — kỳ vọng và phương sai — và tính toán chúng một cách hệ thống cho cả sáu phân phối vừa học ở chương này.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Một hệ thống có xác suất request thành công là $p=0.95$. Với $X \sim \text{Bernoulli}(0.95)$, tính $P(X=1)$ và $P(X=0)$.
2. Gửi 20 request độc lập, mỗi request có xác suất lỗi $p=0.1$. Với $X \sim \text{Binomial}(20, 0.1)$, tính $P(X=0)$ (không có request nào lỗi) bằng công thức ở Mục 6.1.
3. Server nhận trung bình $\lambda=3$ request/giây, tuân theo Poisson(3). Tính $P(X=0)$ — xác suất không có request nào trong 1 giây.

### Mức Trung bình (Intermediate)

4. Dùng xấp xỉ Poisson của Binomial (Mục 7.2): một hệ thống có 10.000 người dùng, mỗi người có xác suất $0.0003$ gặp lỗi trong 1 giờ, độc lập với nhau. Ước lượng $\lambda$, rồi tính $P(X \leq 2)$ (nhiều nhất 2 người gặp lỗi) bằng PMF Poisson.
5. Cho $X \sim \text{Exponential}(\lambda=0.5)$ (thời gian chờ, đơn vị phút). Tính $P(X > 4)$ — xác suất chờ hơn 4 phút — bằng cách tích phân PDF hoặc dùng CDF đã suy ra ở Chương 5.2, Bài tập 4.

### Mức Nâng cao (Advanced)

6. Viết chương trình mô phỏng kiểm chứng thực nghiệm Mục 7.3: sinh 50.000 mẫu Poisson($\lambda=4$) đại diện cho "số sự kiện trong 1 đơn vị thời gian"; song song, sinh thời gian chờ giữa các sự kiện liên tiếp theo Exponential($\lambda=4$) và đếm số sự kiện rơi vào mỗi đơn vị thời gian. So sánh phân phối số sự kiện thu được từ hai cách mô phỏng — chúng có khớp nhau không?

### Mức Nghiên cứu (Research)

7. Tìm hiểu về phân phối **Power Law** (hay phân phối Zipf trong xử lý ngôn ngữ tự nhiên) — một họ phân phối "đuôi nặng" không thuộc sáu phân phối trong chương này. Nêu một ví dụ cụ thể trong Computer Science (ví dụ: phân phối bậc của đỉnh trong mạng xã hội, hoặc tần suất từ trong văn bản tự nhiên) mà việc giả định Gaussian sẽ dẫn đến sai lầm nghiêm trọng, và giải thích ngắn gọn vì sao.

---

## 18. Dự án nhỏ

**Dự án: Mô phỏng Hệ thống Hàng đợi (M/M/1 Queue Simulator)**

- **Mục tiêu:** Xây dựng một mô phỏng hàng đợi đơn giản, kết hợp Poisson (số request đến) và Exponential (thời gian phục vụ) — mô hình hàng đợi kinh điển được gọi là **M/M/1** trong lý thuyết hàng đợi (Volume 4 sẽ mở rộng chủ đề này).
- **Yêu cầu:**
  - Mô phỏng thời gian đến của các request bằng Exponential($\lambda_{\text{arrival}}$) liên tiếp (áp dụng quan hệ Poisson↔Exponential ở Mục 7.3).
  - Mô phỏng thời gian phục vụ mỗi request bằng Exponential($\mu_{\text{service}}$), với $\mu_{\text{service}} > \lambda_{\text{arrival}}$ để hệ thống ổn định.
  - Theo dõi độ dài hàng đợi theo thời gian, và thời gian chờ trung bình của mỗi request.
  - Thử với các tỷ lệ $\rho = \lambda_{\text{arrival}} / \mu_{\text{service}}$ khác nhau (ví dụ 0.5, 0.8, 0.95) — độ dài hàng đợi thay đổi ra sao khi $\rho$ tiến gần 1?
- **Công nghệ đề xuất:** Python, `numpy.random`, tùy chọn `matplotlib` để vẽ độ dài hàng đợi theo thời gian.
- **Kết quả kỳ vọng:** Nhận thấy độ dài hàng đợi tăng phi tuyến (không phải tuyến tính) khi $\rho$ tiến gần 1 — một hiện tượng quan trọng khi thiết kế capacity cho hệ thống production.
- **Mở rộng:** So sánh với hàng đợi có nhiều server phục vụ song song (M/M/c) — độ trễ trung bình cải thiện ra sao?

---

## 19. Tự đánh giá

- [ ] Tôi có thể nhận diện nhanh phân phối phù hợp cho một tình huống kỹ thuật mới, dựa trên đặc điểm của bài toán (Mục 3–4).
- [ ] Tôi có thể viết đúng công thức PMF/PDF cho cả sáu phân phối mà không cần tra cứu.
- [ ] Tôi hiểu và có thể giải thích mối quan hệ Bernoulli → Binomial → Poisson, cũng như quan hệ bổ sung Poisson ↔ Exponential.
- [ ] Tôi có thể sinh mẫu từ cả sáu phân phối bằng `numpy.random`, và tránh được lỗi tham số hóa phổ biến (như $\lambda$ vs `scale` cho Exponential).
- [ ] Tôi có một trực giác ban đầu về vì sao Định lý Giới hạn Trung tâm giải thích sự phổ biến của phân phối Gaussian.

Nếu Bài tập 4 (xấp xỉ Poisson) vẫn còn khó khăn, đây là dấu hiệu nên đọc lại kỹ Formula Box ở Mục 7.2 và thử tính tay với vài giá trị $n, p$ nhỏ trước khi tiếp tục sang Chương 5.4.

---

## 20. Đọc thêm

- **Sách:** Dimitri Bertsekas, John Tsitsiklis, *Introduction to Probability*, các chương về Discrete và Continuous Random Variables — trình bày đầy đủ cả sáu phân phối trong chương này kèm nhiều ví dụ ứng dụng. *(Xem BOOKS.md — Volume 1.)*
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc thêm về Quá trình Poisson (Poisson Process) như một mô hình toán học tổng quát — nền tảng lý thuyết đầy đủ cho Dự án nhỏ ở Mục 18.
- **Chương tiếp theo:** Chương 5.4 — Expectation and Variance.

---

### Liên kết chương (Cross References)

- **Chương trước:** 5.2 — Random Variables (PMF, PDF, CDF là ngôn ngữ nền cho mọi định nghĩa ở chương này); Part II — Discrete Mathematics (hệ số tổ hợp $\binom{n}{x}$ dùng trong PMF của Binomial).
- **Chương tiếp theo:** 5.4 — Expectation and Variance (tính các đại lượng tóm tắt cho cả sáu phân phối).
- **Chương liên quan xa hơn:** Volume 5, Part II (Bernoulli là nền tảng của Binary Cross-Entropy; Gaussian là giả định cốt lõi của hồi quy tuyến tính và khởi tạo trọng số); Volume 4, Part VIII (lý thuyết hàng đợi mở rộng đầy đủ ý tưởng ở Mục 18); Volume 7, Part VI (Định lý Giới hạn Trung tâm được chứng minh và khai thác đầy đủ).
- **Vị trí trong Knowledge Graph:** Nút thứ ba của Part V, phụ thuộc trực tiếp vào Chương 5.1, 5.2, và Part II; là điều kiện tiên quyết cho Chương 5.4, 5.5, và nhiều mô hình học máy ở Volume 5.

---

*Hết Chương 5.3. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer theo `WRITING_STANDARD.md`, khớp với đặc tả Part V trong `VOLUME_01_MATHEMATICS_FOR_CS.md`. Mọi công thức và quan hệ giữa các phân phối đều được kiểm chứng bằng tính toán trực tiếp và mô phỏng thực nghiệm với `numpy.random`, đúng nguyên tắc ưu tiên công cụ đã kiểm định theo `TOOLS.md`. Đang chờ rà soát trước khi tiếp tục sang Chương 5.4 — Expectation and Variance.*
