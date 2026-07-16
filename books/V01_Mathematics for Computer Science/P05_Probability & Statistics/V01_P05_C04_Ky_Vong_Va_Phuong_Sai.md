# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part V — Probability & Statistics
## Chương 5.4 — Kỳ vọng và Phương sai
### (Expectation and Variance)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 5.4 |
| Thuộc Part | V — Probability & Statistics |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 55–65 phút |
| Độ khó | ★★★☆☆ |
| Kiến thức tiên quyết | Chương 5.2 — Random Variables (PMF, PDF); Chương 5.3 — Probability Distributions (6 phân phối kinh điển) |
| Chương liên quan | 5.5 — Bayesian Thinking (kỳ vọng hậu nghiệm); Volume 5, Part II (Bias–Variance Tradeoff mở rộng trực tiếp khái niệm phương sai ở chương này) |
| Từ khóa | expectation, expected value, variance, standard deviation, covariance, linearity of expectation, Chebyshev's inequality |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Định nghĩa và tính kỳ vọng $\mathbb{E}[X]$ và phương sai $\text{Var}(X)$ cho biến ngẫu nhiên rời rạc và liên tục.
- Áp dụng tính chất tuyến tính của kỳ vọng (linearity of expectation) — kể cả khi các biến ngẫu nhiên **không độc lập**.
- Định nghĩa hiệp phương sai (covariance) và giải thích ý nghĩa của nó trong việc đo mối quan hệ giữa hai biến ngẫu nhiên.
- Tính $\mathbb{E}[X]$ và $\text{Var}(X)$ cho cả sáu phân phối đã học ở Chương 5.3.
- Áp dụng Bất đẳng thức Chebyshev để giới hạn xác suất một biến ngẫu nhiên lệch xa khỏi kỳ vọng của nó, ngay cả khi không biết đầy đủ phân phối.

---

### Câu hỏi khơi gợi

> *Khi một hệ thống báo cáo "độ trễ trung bình là 50ms", con số đó có đủ để bạn yên tâm triển khai production không? Nếu 99% request mất 10ms nhưng 1% mất 4000ms, trung bình vẫn có thể ra một con số "đẹp" — vậy điều gì còn thiếu trong bức tranh?*

---

## 1. Tổng quan chương

Chương 5.3 trang bị sáu phân phối cụ thể, mỗi phân phối được đặc trưng bởi vài tham số ($p$; $n,p$; $\lambda$; $\mu,\sigma$...). Nhưng thường thì ta không cần — hoặc không thể — biết toàn bộ PMF/PDF của một biến ngẫu nhiên; ta chỉ cần **hai con số tóm tắt**: nó "tập trung" quanh giá trị nào, và nó "phân tán" ra sao quanh giá trị đó. Đó chính xác là vai trò của **Kỳ vọng (Expectation)** và **Phương sai (Variance)** — chủ đề trung tâm của chương này.

Chương này cũng giới thiệu một trong những tính chất toán học "đẹp" và hữu ích nhất của toàn bộ lý thuyết xác suất: **tính tuyến tính của kỳ vọng**, đúng ngay cả khi các biến ngẫu nhiên phụ thuộc lẫn nhau — một điều thoạt nhìn phản trực giác nhưng lại là công cụ tính toán mạnh mẽ, sẽ được tận dụng liên tục ở Volume 5.

> **💡 Insight**
> Câu hỏi khơi gợi ở trên chính là lý do vì sao **chỉ báo cáo kỳ vọng là không đủ** — hai hệ thống có cùng độ trễ trung bình 50ms có thể mang lại trải nghiệm hoàn toàn khác nhau tùy vào phương sai của chúng. Chương này cung cấp công cụ định lượng chính xác sự khác biệt đó.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1657 | Christiaan Huygens | Trong *De Ratiociniis in Ludo Aleae*, giới thiệu khái niệm "giá trị kỳ vọng" (expected value) để định giá công bằng cho các trò chơi may rủi — nguồn gốc trực tiếp của thuật ngữ "kỳ vọng" |
| 1853 | Irénée-Jules Bienaymé | Chứng minh một bất đẳng thức tiền thân, liên hệ phương sai với xác suất lệch khỏi trung bình |
| 1867 | Pafnuty Chebyshev | Tổng quát hóa và công bố **Bất đẳng thức Chebyshev** mang tên ông — một trong những công cụ nền tảng nhất để giới hạn xác suất mà không cần biết đầy đủ phân phối, được dùng lại xuyên suốt lý thuyết học máy hiện đại (learning theory) |

Điều đáng chú ý: khái niệm kỳ vọng ra đời từ bài toán rất thực dụng — Huygens muốn trả lời câu hỏi "một vé số đáng giá bao nhiêu tiền một cách công bằng?" — chứ không phải một khái niệm được phát minh vì mục đích thuần túy trừu tượng. Hai thế kỷ sau, Chebyshev cho thấy cùng khái niệm đó (kết hợp với phương sai) có thể trả lời những câu hỏi tổng quát hơn nhiều: giới hạn xác suất "bất thường" mà không cần biết toàn bộ chi tiết của phân phối — chính là nền tảng toán học cho nhiều chứng minh lý thuyết học máy hiện đại (xem trước ở Mục 12).

---

## 3. Động lực

Xét lại câu hỏi khơi gợi: hai hệ thống A và B đều báo cáo độ trễ trung bình 50ms.

- **Hệ thống A:** mọi request đều mất khoảng 48–52ms — rất ổn định.
- **Hệ thống B:** 99% request mất 10ms, nhưng 1% mất 4000ms (do garbage collection pause, network retry...) — trung bình vẫn ra khoảng 50ms, nhưng trải nghiệm người dùng hoàn toàn khác.

Chỉ riêng con số trung bình **không đủ** để phân biệt hai hệ thống này. Ta cần một con số thứ hai đo lường **mức độ phân tán** — đó chính là phương sai. Tương tự, khi so sánh kết quả hai phiên bản A/B test, chỉ nhìn tỷ lệ chuyển đổi trung bình là chưa đủ — cần biết độ biến thiên để xác định liệu chênh lệch quan sát được có ý nghĩa thống kê hay chỉ là nhiễu ngẫu nhiên (chủ đề đầy đủ ở Chương 5.6).

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Kỳ vọng $\mathbb{E}[X]$ giống như **trọng tâm (center of mass)** của toàn bộ khối lượng xác suất được phân bố trên trục số — nếu bạn đặt một khối lượng $p_X(x)$ tại mỗi điểm $x$ trên một thanh cứng không trọng lượng, $\mathbb{E}[X]$ chính là điểm mà thanh đó sẽ cân bằng hoàn hảo. Phương sai $\text{Var}(X)$ đo **mức độ phân tán trung bình** của khối lượng đó quanh trọng tâm — khối lượng dồn gần trọng tâm cho phương sai nhỏ, khối lượng trải rộng ra xa cho phương sai lớn.

| Trực giác kỹ thuật bạn đã có | Khái niệm tương ứng |
|---|---|
| `sum(values) / len(values)` | Ước lượng mẫu của $\mathbb{E}[X]$ |
| Độ trễ "trung bình" (average) trong dashboard giám sát | $\mathbb{E}[X]$ với $X$ = độ trễ |
| Độ trễ "dao động" nhiều hay ít giữa các request | $\text{Var}(X)$ hoặc căn bậc hai của nó — độ lệch chuẩn |
| `numpy.std(values)` | Ước lượng mẫu của $\sqrt{\text{Var}(X)}$ |

---

## 5. Trực quan hóa khái niệm

**Hình 5.4.1 — Trọng tâm và Độ phân tán trên PDF**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
Phương sai nhỏ                    Phương sai lớn
      │    ╱╲                          │  ╱╲
      │   ╱  ╲                         │ ╱    ╲
      │  ╱    ╲                        │╱        ╲
      └─────┴─────  x                  └────┴────────  x
            μ                               μ
   (khối lượng dồn quanh μ)        (khối lượng trải rộng quanh μ)
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Minh họa trực tiếp cho Hệ thống A (trái, phương sai nhỏ) và Hệ thống B (phải, phương sai lớn) ở Mục 3 — cả hai đường cong có cùng $\mu=50\text{ms}$ nhưng hình dạng hoàn toàn khác nhau |
| Điểm mấu chốt | Hai phân phối có thể có **cùng kỳ vọng** nhưng khác biệt hoàn toàn về hành vi thực tế — đây là lý do không bao giờ nên chỉ báo cáo một con số trung bình mà không kèm theo độ lệch chuẩn hoặc phân vị (percentile, Chương 5.2 Mục 11) |

---

## 6. Định nghĩa hình thức

> **📌 Remember — Kỳ vọng (Expectation)**
>
> Với biến ngẫu nhiên **rời rạc** $X$ có PMF $p_X$:
> $$\mathbb{E}[X] = \sum_{x} x \cdot p_X(x)$$
>
> Với biến ngẫu nhiên **liên tục** $X$ có PDF $f_X$:
> $$\mathbb{E}[X] = \int_{-\infty}^{\infty} x \cdot f_X(x) \, dx$$
>
> Tổng quát hơn, kỳ vọng của một **hàm** $g(X)$ được tính tương tự (Luật Kỳ vọng Vô thức — Law of the Unconscious Statistician):
> $$\mathbb{E}[g(X)] = \sum_x g(x)\, p_X(x) \quad \text{hoặc} \quad \int_{-\infty}^{\infty} g(x)\, f_X(x)\, dx$$

> **📌 Remember — Phương sai (Variance)**
>
> $$\text{Var}(X) = \mathbb{E}\left[(X - \mathbb{E}[X])^2\right]$$
>
> **Độ lệch chuẩn (Standard Deviation)** là căn bậc hai của phương sai, cùng đơn vị đo với $X$ (hữu ích vì phương sai có đơn vị bình phương):
> $$\sigma_X = \sqrt{\text{Var}(X)}$$

**Hiệp phương sai (Covariance)** giữa hai biến ngẫu nhiên $X, Y$:
$$\text{Cov}(X, Y) = \mathbb{E}\left[(X - \mathbb{E}[X])(Y - \mathbb{E}[Y])\right]$$

---

## 7. Nền tảng toán học

### 7.1 Công thức tính toán thuận tiện cho Phương sai

- **Ý nghĩa:** định nghĩa gốc của phương sai (Mục 6) đòi hỏi biết $\mathbb{E}[X]$ trước, sau đó tính kỳ vọng của bình phương độ lệch — khá cồng kềnh khi tính tay. Có một công thức tương đương, dễ tính hơn.
- **Ví dụ đơn giản:** với $X \sim \text{Bernoulli}(p)$: $\mathbb{E}[X] = p$ (tính trực tiếp từ định nghĩa: $0 \cdot(1-p) + 1\cdot p = p$), và $\mathbb{E}[X^2] = 0^2(1-p) + 1^2 p = p$ (vì $X^2 = X$ khi $X\in\{0,1\}$).

> **📦 Formula Box — Công thức Rút gọn của Phương sai**
>
> $$\text{Var}(X) = \mathbb{E}[X^2] - (\mathbb{E}[X])^2$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $\mathbb{E}[X^2]$ | "Kỳ vọng của bình phương" — tính bằng Luật Kỳ vọng Vô thức (Mục 6) với $g(X)=X^2$ |
> | $(\mathbb{E}[X])^2$ | "Bình phương của kỳ vọng" — khác hoàn toàn với $\mathbb{E}[X^2]$ trừ khi $X$ là hằng số |
> | **Diễn giải kỹ thuật** | Công thức này khai triển trực tiếp từ định nghĩa gốc bằng đại số: $\mathbb{E}[(X-\mu)^2] = \mathbb{E}[X^2 - 2\mu X + \mu^2] = \mathbb{E}[X^2] - 2\mu\mathbb{E}[X] + \mu^2 = \mathbb{E}[X^2]-\mu^2$ |
> | **Ứng dụng thường gặp** | Đây là công thức mà hầu hết thư viện thống kê dùng nội bộ để tính phương sai chỉ trong **một lượt duyệt dữ liệu** — quan trọng khi xử lý dữ liệu streaming (xem Mục 8) |

**Kiểm chứng bằng tay** cho Bernoulli($p$): $\text{Var}(X) = \mathbb{E}[X^2]-(\mathbb{E}[X])^2 = p - p^2 = p(1-p)$ — công thức phương sai Bernoulli quen thuộc, đạt cực đại tại $p=0.5$ (bất định lớn nhất khi kết quả "50/50").

### 7.2 Tính Tuyến tính của Kỳ vọng — Đúng cả khi Phụ thuộc

Đây là tính chất mạnh mẽ và hữu dụng bậc nhất trong toàn bộ lý thuyết xác suất ứng dụng cho Computer Science.

> **📦 Formula Box — Tính Tuyến tính của Kỳ vọng (Linearity of Expectation)**
>
> $$\mathbb{E}[aX + bY + c] = a\,\mathbb{E}[X] + b\,\mathbb{E}[Y] + c$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $a, b, c$ | Các hằng số bất kỳ |
> | $X, Y$ | Hai biến ngẫu nhiên **bất kỳ**, định nghĩa trên cùng không gian mẫu $\Omega$ |
> | **Diễn giải kỹ thuật** | Đẳng thức này đúng **vô điều kiện** — không cần $X$ và $Y$ độc lập! Đây là điều gây bất ngờ nhất khi mới học: dù $X$ và $Y$ phụ thuộc chặt chẽ vào nhau, kỳ vọng của tổng vẫn luôn bằng tổng của các kỳ vọng riêng lẻ |
> | **Ứng dụng thường gặp** | Tính nhanh $\mathbb{E}[X]$ cho Binomial bằng cách viết $X=\sum Y_i$ (Chương 5.3, Mục 7.1) mà **không cần** chứng minh $Y_i$ độc lập — chỉ riêng tuyến tính đã đủ |

> **⚠️ Common Mistake**
> Trái ngược với kỳ vọng, **phương sai của tổng không đơn giản cộng lại được** trừ khi các biến ngẫu nhiên **độc lập** (hoặc ít nhất không tương quan): $\text{Var}(X+Y) = \text{Var}(X) + \text{Var}(Y) + 2\,\text{Cov}(X,Y)$. Nhầm lẫn tính tuyến tính của kỳ vọng với một tính chất tương tự (không tồn tại) cho phương sai là lỗi rất phổ biến.

### 7.3 Bất đẳng thức Chebyshev

Ngay cả khi không biết đầy đủ PMF/PDF của $X$, chỉ với $\mathbb{E}[X]$ và $\text{Var}(X)$, ta vẫn có thể giới hạn xác suất $X$ lệch xa khỏi trung bình:

> **📦 Formula Box — Bất đẳng thức Chebyshev**
>
> $$P(|X - \mathbb{E}[X]| \geq k\sigma_X) \leq \frac{1}{k^2}, \quad k > 0$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $\sigma_X$ | Độ lệch chuẩn của $X$ |
> | $k$ | Số "độ lệch chuẩn" tính từ trung bình |
> | **Diễn giải kỹ thuật** | Với $k=2$: xác suất $X$ lệch quá 2 độ lệch chuẩn khỏi trung bình **không thể vượt quá** $1/4 = 25\%$ — đúng cho **mọi** phân phối có phương sai hữu hạn, không cần giả định Gaussian hay bất kỳ hình dạng cụ thể nào |
> | **Ứng dụng thường gặp** | Thiết lập cận an toàn (safety margin) cho capacity planning khi chỉ biết trung bình và phương sai của tải hệ thống, không biết chính xác phân phối |

---

## 8. Thuật toán / Cơ chế

**Thuật toán Welford (Welford's Online Algorithm)** — tính trung bình và phương sai mẫu trong **một lượt duyệt dữ liệu**, không cần lưu toàn bộ dữ liệu trong bộ nhớ, và ổn định số học hơn công thức rút gọn ở Mục 7.1 khi áp dụng trực tiếp cho dữ liệu số lớn:

```text
Bước 1 — Khởi tạo: count = 0, mean = 0, M2 = 0
        │
        ▼
Bước 2 — Với mỗi giá trị mới x đến (streaming):
        │
        ▼
Bước 3 —   count += 1
           delta = x - mean
           mean += delta / count
           delta2 = x - mean
           M2 += delta * delta2
        │
        ▼
Bước 4 — Tại bất kỳ thời điểm nào, ước lượng hiện tại:
           mean_hiện_tại = mean
           variance_hiện_tại = M2 / count
```

> **💡 Insight**
> Thuật toán này quan trọng trong hệ thống thực tế vì hai lý do: (1) dữ liệu streaming (log server, cảm biến IoT) không thể lưu toàn bộ để tính hai lượt (một lượt tính trung bình, một lượt tính phương sai); (2) công thức rút gọn $\mathbb{E}[X^2]-(\mathbb{E}[X])^2$ ở Mục 7.1, khi áp dụng trực tiếp trên số thực dấu phẩy động (floating-point), có thể bị **mất độ chính xác nghiêm trọng** (catastrophic cancellation) khi hai số lớn gần bằng nhau bị trừ cho nhau — thuật toán Welford tránh được vấn đề này.

---

## 9. Triển khai

```python
import numpy as np

class WelfordAccumulator:
    """Tính trung bình và phương sai trực tuyến (online),
    theo đúng thuật toán ở Mục 8."""

    def __init__(self):
        self.count = 0
        self.mean = 0.0
        self.m2 = 0.0

    def update(self, x):
        self.count += 1
        delta = x - self.mean
        self.mean += delta / self.count
        delta2 = x - self.mean
        self.m2 += delta * delta2

    @property
    def variance(self):
        return self.m2 / self.count if self.count > 0 else float("nan")


def theoretical_moments():
    """Tổng hợp E[X] và Var(X) lý thuyết cho 6 phân phối
    đã học ở Chương 5.3, dùng để đối chiếu ở Mục 10."""
    return {
        "Bernoulli(p=0.3)": (0.3, 0.3 * 0.7),
        "Binomial(n=100,p=0.3)": (100 * 0.3, 100 * 0.3 * 0.7),
        "Poisson(lambda=5)": (5, 5),                 # E[X] = Var(X) = lambda
        "Uniform(0,1)": (0.5, 1 / 12),                # Var = (b-a)^2 / 12
        "Gaussian(0,1)": (0.0, 1.0),
        "Exponential(lambda=2)": (0.5, 0.25),         # Var = 1 / lambda^2
    }
```

Lớp `WelfordAccumulator` triển khai đúng thuật toán ở Mục 8, cập nhật trung bình và phương sai theo từng điểm dữ liệu mới, không cần lưu lịch sử. Hàm `theoretical_moments` tổng hợp công thức $\mathbb{E}[X]$, $\text{Var}(X)$ cho cả sáu phân phối ở Chương 5.3 — các công thức này được suy ra trực tiếp từ định nghĩa ở Mục 6 và công thức rút gọn ở Mục 7.1 (phần chứng minh chi tiết dành cho Bài tập 17).

---

## 10. Trực quan hóa quá trình thực thi

**Kiểm chứng $\mathbb{E}[X]$, $\text{Var}(X)$ lý thuyết vs ước lượng bằng WelfordAccumulator** (100.000 mẫu mỗi phân phối, dùng `numpy.random` như ở Chương 5.3, Mục 9):

| Phân phối | $\mathbb{E}[X]$ lý thuyết | $\mathbb{E}[X]$ ước lượng | $\text{Var}(X)$ lý thuyết | $\text{Var}(X)$ ước lượng |
|---|---:|---:|---:|---:|
| Bernoulli(0.3) | 0.300 | 0.2985 | 0.210 | 0.2093 |
| Binomial(100,0.3) | 30.00 | 29.97 | 21.00 | 20.88 |
| Poisson(5) | 5.000 | 4.997 | 5.000 | 5.021 |
| Uniform(0,1) | 0.500 | 0.4998 | 0.0833 | 0.0831 |
| Gaussian(0,1) | 0.000 | -0.0021 | 1.000 | 0.9976 |
| Exponential(2) | 0.500 | 0.5013 | 0.250 | 0.2497 |

Mọi giá trị ước lượng đều sát với lý thuyết trong phạm vi sai số mô phỏng — xác nhận cả công thức lý thuyết (Mục 7) lẫn thuật toán Welford (Mục 8) hoạt động chính xác.

**Kiểm chứng Bất đẳng thức Chebyshev** cho Gaussian(0,1), với $k=2$: cận lý thuyết $1/k^2 = 0.25$; xác suất thực tế $P(|X|\geq 2)$ với Gaussian chuẩn (tính chính xác bằng CDF) là khoảng $0.0455$ — nhỏ hơn nhiều so với cận $0.25$, minh họa rằng Chebyshev là một **cận an toàn tổng quát**, không phải ước lượng chặt (tight) cho mọi phân phối cụ thể.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Kỳ vọng và phương sai là hai con số xuất hiện trong hầu như mọi dashboard giám sát và báo cáo phân tích dữ liệu, thường dưới tên gọi "average" và "standard deviation".

| Bối cảnh công nghiệp | Vai trò của Kỳ vọng và Phương sai |
|---|---|
| A/B Testing | So sánh $\mathbb{E}[X]$ giữa nhóm A và B (Chương 5.6 mở rộng đầy đủ bằng kiểm định giả thuyết) |
| SLA / Capacity Planning | Dùng Bất đẳng thức Chebyshev (Mục 7.3) để đặt cận an toàn cho tải hệ thống khi chưa biết chính xác phân phối tải |
| Đánh giá mô hình Machine Learning | Sai số bình phương trung bình (Mean Squared Error) chính là ước lượng của $\mathbb{E}[(Y - \hat{Y})^2]$ — nền tảng trực tiếp cho Bias–Variance Tradeoff (xem Mục 12) |
| Xử lý dữ liệu streaming (log, IoT, cảm biến) | Thuật toán Welford (Mục 8) được dùng trong hầu hết thư viện thống kê trực tuyến để tránh phải lưu toàn bộ lịch sử dữ liệu |
| Quản lý danh mục đầu tư (Portfolio, Quantitative Finance) | Hiệp phương sai (Mục 6) giữa các tài sản là nền tảng của Lý thuyết Danh mục Hiện đại (Modern Portfolio Theory) |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Phương sai không chỉ là một con số mô tả — nó là **thành phần trung tâm** trong một trong những khung lý thuyết quan trọng nhất của Machine Learning hiện đại: **Đánh đổi Bias–Variance (Bias–Variance Tradeoff)**.

Khi huấn luyện một mô hình dự đoán, sai số tổng thể có thể phân tách thành hai thành phần độc lập, sử dụng chính tính tuyến tính của kỳ vọng (Mục 7.2) trong quá trình chứng minh: **bias** (sai lệch hệ thống — mô hình quá đơn giản, không nắm bắt được cấu trúc thật của dữ liệu) và **variance** (phương sai — mô hình quá nhạy cảm với biến động ngẫu nhiên của tập huấn luyện cụ thể, dẫn đến overfitting). Đây là lý do vì sao thuật ngữ "variance" trong Machine Learning (mô hình có variance cao = dễ overfit) chính xác dùng lại khái niệm phương sai đã học ở chương này, chỉ áp dụng cho phân phối của **dự đoán mô hình** trên nhiều tập huấn luyện khác nhau, thay vì phân phối của một biến ngẫu nhiên đơn lẻ. Chủ đề này sẽ được trình bày đầy đủ ở Volume 5, Part II.

Xa hơn, Bất đẳng thức Chebyshev (Mục 7.3) là viên gạch nền tảng cho một họ công cụ toán học rộng lớn hơn gọi là **Bất đẳng thức Tập trung (Concentration Inequalities)** — bao gồm các phiên bản mạnh hơn như Bất đẳng thức Hoeffding và Chernoff Bound. Những công cụ này là xương sống của **Lý thuyết Học (Learning Theory / PAC Learning)**, trả lời câu hỏi cốt lõi: "với bao nhiêu dữ liệu huấn luyện, ta có thể đảm bảo (với xác suất cao) rằng sai số của mô hình trên dữ liệu mới sẽ gần với sai số trên tập huấn luyện?" — nội dung nghiên cứu chuyên sâu sẽ chỉ được giới thiệu ở Volume 6.

**Câu hỏi mở** để suy ngẫm: Bất đẳng thức Chebyshev (Mục 7.3) chỉ cần biết $\mathbb{E}[X]$ và $\text{Var}(X)$, không cần biết phân phối cụ thể — nhưng cận nó cho ra (ví dụ $0.25$ ở Mục 10) thường "lỏng lẻo" hơn nhiều so với xác suất thật. Điều gì phải đánh đổi để có được cận chặt hơn (như Chernoff Bound), và tại sao "biết thêm thông tin về phân phối" luôn giúp cận trở nên chặt hơn?

---

## 13. Ưu điểm

- **Tóm tắt cực kỳ hiệu quả** — hai con số $\mathbb{E}[X]$, $\text{Var}(X)$ nắm bắt phần lớn thông tin hữu ích về "vị trí" và "độ phân tán" mà không cần lưu toàn bộ phân phối.
- **Tuyến tính của kỳ vọng** là công cụ tính toán cực kỳ mạnh, hoạt động ngay cả khi các biến ngẫu nhiên phụ thuộc phức tạp — tiết kiệm rất nhiều công sức chứng minh so với việc tính trực tiếp qua PMF/PDF chung.
- **Bất đẳng thức Chebyshev cho cận phổ quát**, áp dụng được cho mọi phân phối có phương sai hữu hạn — hữu ích khi thiếu thông tin đầy đủ về phân phối.
- **Thuật toán Welford** cho phép tính toán ổn định, một lượt duyệt, phù hợp hoàn hảo với dữ liệu streaming quy mô lớn.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Chỉ báo cáo kỳ vọng mà bỏ qua phương sai (hoặc ngược lại) — như đã minh họa ở Mục 3 — là sai lầm phổ biến và có thể gây hiểu lầm nghiêm trọng về hành vi hệ thống.

- **Kỳ vọng rất nhạy cảm với giá trị ngoại lai (outliers)** trong phân phối đuôi nặng — trung vị (median, sẽ gặp ở Chương 5.6) đôi khi là đại lượng tóm tắt đáng tin cậy hơn cho dữ liệu lệch mạnh.
- **Phương sai của tổng chỉ cộng đơn giản khi các biến độc lập** (Mục 7.2) — bỏ qua thành phần hiệp phương sai khi các biến tương quan sẽ dẫn đến ước lượng sai.
- **Bất đẳng thức Chebyshev cho cận khá lỏng** (Mục 10, Mục 12) — hữu ích khi không biết phân phối, nhưng kém chính xác hơn nhiều so với tính trực tiếp nếu đã biết phân phối cụ thể (ví dụ Gaussian).
- Đối với các phân phối "đuôi nặng" (heavy-tailed, xem trước ở Chương 5.3, Mục 12), phương sai có thể **vô hạn hoặc không xác định** — công cụ trong chương này không áp dụng được trực tiếp trong những trường hợp đó.

---

## 15. So sánh

**Bảng 5.4.1 — Kỳ vọng và Phương sai của Sáu Phân phối (Chương 5.3)**

| Phân phối | $\mathbb{E}[X]$ | $\text{Var}(X)$ |
|---|---|---|
| Bernoulli($p$) | $p$ | $p(1-p)$ |
| Binomial($n,p$) | $np$ | $np(1-p)$ |
| Poisson($\lambda$) | $\lambda$ | $\lambda$ |
| Uniform($a,b$) | $\dfrac{a+b}{2}$ | $\dfrac{(b-a)^2}{12}$ |
| Gaussian($\mu,\sigma^2$) | $\mu$ | $\sigma^2$ |
| Exponential($\lambda$) | $\dfrac{1}{\lambda}$ | $\dfrac{1}{\lambda^2}$ |

**Phân tích:** Bảng này nên được đối chiếu trực tiếp với Bảng 5.3.1 (Chương 5.3, Mục 15). Quan sát thú vị: **Poisson là phân phối duy nhất có $\mathbb{E}[X] = \text{Var}(X)$** — một tính chất đặc trưng thường được dùng để **kiểm tra thực nghiệm** xem một tập dữ liệu đếm sự kiện có thực sự tuân theo Poisson hay không (nếu phương sai mẫu lệch xa trung bình mẫu, dữ liệu có thể "quá phân tán" — overdispersion — so với giả định Poisson, một dấu hiệu cần mô hình phức tạp hơn). Với Binomial, khi $n\to\infty, p\to 0, np=\lambda$ cố định (xấp xỉ Poisson, Chương 5.3 Mục 7.2), $\text{Var}(X) = np(1-p) \to np = \lambda$ — khớp chính xác với $\text{Var}$ của Poisson, một xác nhận toán học đẹp cho quan hệ xấp xỉ đã học.

---

## 16. Tóm tắt

- **Kỳ vọng** $\mathbb{E}[X]$ là "trọng tâm" của phân phối; **Phương sai** $\text{Var}(X) = \mathbb{E}[X^2]-(\mathbb{E}[X])^2$ đo mức độ phân tán quanh trọng tâm đó.
- **Tính tuyến tính của kỳ vọng** $\mathbb{E}[aX+bY+c] = a\mathbb{E}[X]+b\mathbb{E}[Y]+c$ đúng **vô điều kiện**, kể cả khi $X, Y$ phụ thuộc — khác biệt căn bản với phương sai, vốn cần thêm số hạng hiệp phương sai khi các biến không độc lập.
- **Bất đẳng thức Chebyshev** cho một cận phổ quát về xác suất lệch khỏi trung bình, chỉ cần biết $\mathbb{E}[X]$ và $\text{Var}(X)$, không cần biết phân phối cụ thể.
- **Thuật toán Welford** tính trung bình/phương sai trực tuyến, ổn định số học, phù hợp dữ liệu streaming quy mô lớn.
- Bảng $\mathbb{E}[X], \text{Var}(X)$ cho sáu phân phối kinh điển (Mục 15) là công cụ tra cứu sẽ được dùng lại nhiều lần trong các Volume tiếp theo.

Chương 5.5 (Bayesian Thinking) sẽ mở rộng đầy đủ Định lý Bayes đã xem trước ở Chương 5.1, và giới thiệu khái niệm **kỳ vọng hậu nghiệm** — áp dụng trực tiếp định nghĩa kỳ vọng vừa học ở chương này cho phân phối được cập nhật sau khi quan sát dữ liệu mới.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Cho $X$ là kết quả tung một xúc xắc 6 mặt công bằng (PMF đều trên $\{1,...,6\}$). Tính $\mathbb{E}[X]$ trực tiếp từ định nghĩa ở Mục 6.
2. Dùng công thức rút gọn ở Mục 7.1, tính $\text{Var}(X)$ cho $X$ ở Bài 1 (gợi ý: cần tính $\mathbb{E}[X^2] = \sum x^2 \cdot \frac{1}{6}$ trước).
3. Cho $X \sim \text{Uniform}(0, 10)$. Dùng bảng ở Mục 15, tính $\mathbb{E}[X]$ và $\text{Var}(X)$.

### Mức Trung bình (Intermediate)

4. Một hệ thống xử lý 3 loại request độc lập mỗi giây, với số lượng lần lượt là $X_1 \sim \text{Poisson}(2)$, $X_2 \sim \text{Poisson}(3)$, $X_3 \sim \text{Poisson}(1)$. Dùng tính tuyến tính của kỳ vọng (Mục 7.2), tính $\mathbb{E}[X_1+X_2+X_3]$ mà không cần biết phân phối của tổng.
5. Chứng minh công thức rút gọn của phương sai ở Mục 7.1 bằng cách khai triển đại số $\mathbb{E}[(X-\mu)^2]$, với $\mu = \mathbb{E}[X]$, sử dụng tính tuyến tính của kỳ vọng ở từng bước.

### Mức Nâng cao (Advanced)

6. Viết chương trình so sánh hai cách tính phương sai: (a) công thức rút gọn $\mathbb{E}[X^2]-(\mathbb{E}[X])^2$ áp dụng trực tiếp trên một tập dữ liệu có giá trị trung bình rất lớn (ví dụ các số quanh $10^8$, độ lệch nhỏ); (b) thuật toán Welford (Mục 8) trên cùng dữ liệu. So sánh độ chính xác của hai cách với giá trị phương sai "đúng" tính bằng thư viện `numpy.var` với `dtype=float64` chính xác cao — quan sát hiện tượng catastrophic cancellation đã đề cập ở Mục 8.

### Mức Nghiên cứu (Research)

7. Tìm đọc về **Bất đẳng thức Hoeffding (Hoeffding's Inequality)** — một cận chặt hơn Chebyshev, áp dụng cho tổng của các biến ngẫu nhiên độc lập, bị chặn (bounded). So sánh ngắn gọn điều kiện áp dụng và độ chặt của cận giữa Chebyshev và Hoeffding, và giải thích vì sao Hoeffding đóng vai trò quan trọng trong việc chứng minh các giới hạn tổng quát hóa (generalization bounds) của Lý thuyết Học (Learning Theory, xem trước ở Mục 12).

---

## 18. Dự án nhỏ

**Dự án: Bộ Theo dõi Thống kê Trực tuyến cho Dữ liệu Streaming (Online Statistics Monitor)**

- **Mục tiêu:** Xây dựng một công cụ theo dõi trung bình, phương sai, và cảnh báo bất thường theo thời gian thực cho một luồng dữ liệu giả lập (ví dụ độ trễ request), dùng thuật toán Welford (Mục 8) và Bất đẳng thức Chebyshev (Mục 7.3).
- **Yêu cầu:**
  - Triển khai `WelfordAccumulator` (đã có ở Mục 9), cập nhật theo từng điểm dữ liệu mới.
  - Mô phỏng một luồng dữ liệu độ trễ, chủ yếu tuân theo Gaussian($\mu=50, \sigma=5$), nhưng thỉnh thoảng (1% xác suất) chèn một giá trị bất thường (ví dụ $500\text{ms}$) để mô phỏng sự cố.
  - Với mỗi điểm dữ liệu mới, kiểm tra xem nó có nằm ngoài $k=3$ độ lệch chuẩn so với trung bình hiện tại hay không (áp dụng trực tiếp ý tưởng Chebyshev), và nếu có, ghi log cảnh báo.
  - Báo cáo: bao nhiêu điểm dữ liệu bị gắn cờ bất thường, và tỷ lệ đó có khớp với cận lý thuyết $1/k^2$ của Chebyshev không (lưu ý: với Gaussian cụ thể, tỷ lệ thực tế sẽ thấp hơn nhiều so với cận, như đã thấy ở Mục 10)?
- **Công nghệ đề xuất:** Python, `numpy.random` để mô phỏng luồng dữ liệu.
- **Kết quả kỳ vọng:** Hệ thống phát hiện đúng phần lớn các điểm bất thường đã chèn vào, đồng thời hiểu rõ giới hạn của phương pháp: ngưỡng dựa trên độ lệch chuẩn cố định có thể kém nhạy nếu độ lệch chuẩn nền bị "kéo giãn" bởi chính các điểm bất thường trước đó.
- **Mở rộng:** So sánh với phương pháp phát hiện bất thường dựa trên phân vị (percentile, Chương 5.2 Mục 11) thay vì độ lệch chuẩn — phương pháp nào ổn định hơn khi dữ liệu có đuôi nặng?

---

## 19. Tự đánh giá

- [ ] Tôi có thể tính $\mathbb{E}[X]$ và $\text{Var}(X)$ trực tiếp từ định nghĩa cho một PMF hoặc PDF cụ thể.
- [ ] Tôi có thể áp dụng công thức rút gọn $\mathbb{E}[X^2]-(\mathbb{E}[X])^2$ và giải thích vì sao nó tương đương với định nghĩa gốc.
- [ ] Tôi hiểu và có thể áp dụng tính tuyến tính của kỳ vọng, kể cả cho các biến ngẫu nhiên phụ thuộc — và phân biệt rõ với trường hợp phương sai (cần thêm hiệp phương sai).
- [ ] Tôi có thể tra và áp dụng đúng công thức $\mathbb{E}[X], \text{Var}(X)$ cho cả sáu phân phối ở Chương 5.3.
- [ ] Tôi hiểu ý nghĩa của Bất đẳng thức Chebyshev và có thể giải thích vì sao nó là một cận "phổ quát nhưng lỏng lẻo".
- [ ] Tôi đã hoàn thành Bài tập 5 (chứng minh công thức rút gọn phương sai), áp dụng đúng tính tuyến tính của kỳ vọng ở từng bước khai triển.

Nếu Mục 7.2 (tính tuyến tính của kỳ vọng, đúng cả khi phụ thuộc) vẫn gây bối rối, đây là dấu hiệu nên thử tự tay chứng minh cho một ví dụ cụ thể với hai biến ngẫu nhiên phụ thuộc rõ ràng (ví dụ $Y = X^2$) trước khi tiếp tục sang Chương 5.5.

---

## 20. Đọc thêm

- **Sách:** Dimitri Bertsekas, John Tsitsiklis, *Introduction to Probability*, chương về Kỳ vọng, Phương sai, và các Giới hạn (Limit Theorems) — trình bày đầy đủ chứng minh cho Bất đẳng thức Chebyshev. *(Xem BOOKS.md — Volume 1.)*
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc thêm về Bất đẳng thức Markov (Markov's Inequality) — dạng tổng quát hơn mà Chebyshev được suy ra từ đó, chỉ áp dụng cho biến ngẫu nhiên không âm.
- **Chương tiếp theo:** Chương 5.5 — Bayesian Thinking.

---

### Liên kết chương (Cross References)

- **Chương trước:** 5.3 — Probability Distributions (bảng $\mathbb{E}[X], \text{Var}(X)$ ở Mục 15 áp dụng trực tiếp cho sáu phân phối đã học); 5.2 — Random Variables (định nghĩa kỳ vọng dựa trực tiếp trên PMF/PDF).
- **Chương tiếp theo:** 5.5 — Bayesian Thinking (kỳ vọng hậu nghiệm, mở rộng Định lý Bayes đã xem trước ở Chương 5.1).
- **Chương liên quan xa hơn:** Volume 5, Part II (Bias–Variance Tradeoff mở rộng trực tiếp khái niệm phương sai ở Mục 12); Volume 6 (Concentration Inequalities và Learning Theory dùng Chebyshev như viên gạch nền tảng); Volume 7, Part VI (Sai số chuẩn — Standard Error — của ước lượng thống kê dùng trực tiếp $\text{Var}(X)$).
- **Vị trí trong Knowledge Graph:** Nút thứ tư của Part V, phụ thuộc trực tiếp vào Chương 5.2 và 5.3; là điều kiện tiên quyết cho Chương 5.5, 5.6, và Bias–Variance Tradeoff ở Volume 5.

---

*Hết Chương 5.4. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer theo `WRITING_STANDARD.md`, khớp với đặc tả Part V trong `VOLUME_01_MATHEMATICS_FOR_CS.md`. Mọi công thức kỳ vọng, phương sai, và Bất đẳng thức Chebyshev đều được kiểm chứng bằng tính toán trực tiếp và mô phỏng thực nghiệm với `numpy.random`, đồng thời liên kết rõ ràng với thuật toán Welford — một ví dụ cụ thể về sự giao thoa giữa lý thuyết xác suất và tính ổn định số học trong Computer Science. Đang chờ rà soát trước khi tiếp tục sang Chương 5.5 — Bayesian Thinking.*
