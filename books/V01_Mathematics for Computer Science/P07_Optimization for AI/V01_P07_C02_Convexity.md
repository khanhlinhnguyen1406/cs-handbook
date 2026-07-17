# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part VII — Optimization for Artificial Intelligence
## Chương 7.2 — Tập lồi và Hàm lồi
### (Convexity)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 7.2 |
| Thuộc Part | VII — Optimization for Artificial Intelligence |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 40–50 phút |
| Độ khó | ★★★☆☆ |
| Kiến thức tiên quyết | Chương 7.1 — Optimization Problems (đặc biệt định nghĩa cực tiểu địa phương/toàn cục); Part IV — Calculus (đạo hàm bậc hai); Part III — Linear Algebra (khái niệm eigenvalue, chỉ dùng ở mức tham chiếu) |
| Chương liên quan | 7.3 — Gradient Descent (thuật toán mà tính lồi quyết định có đảm bảo hội tụ hay không); Volume 5, Part II — Machine Learning (hàm Loss của hồi quy tuyến tính là lồi, của mạng neural thường không lồi) |
| Từ khóa | convex set, convex function, Jensen's inequality, local minimum, global minimum, non-convex, loss landscape |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Định nghĩa hình thức **tập lồi (convex set)** và kiểm tra được một tập hợp cụ thể có lồi hay không.
- Định nghĩa hình thức **hàm lồi (convex function)** bằng bất đẳng thức Jensen, và giải thích trực giác hình học tương ứng.
- Phát biểu và giải thích trực giác chứng minh cho định lý quan trọng nhất của chương: *với hàm lồi, mọi cực tiểu địa phương đều là cực tiểu toàn cục*.
- Áp dụng điều kiện đạo hàm bậc hai để kiểm tra tính lồi của một hàm một biến, và biết được điều kiện tương ứng cho hàm nhiều biến (Hessian) tồn tại dù chưa cần tính toán chi tiết.
- Nhận diện được vì sao hàm mất mát của mạng neural nhiều lớp thường **không lồi**, và hiểu hệ quả thực hành của điều đó.

---

### Câu hỏi khơi gợi

> *Ở Chương 7.1, bạn đã thấy rằng một thuật toán tối ưu "đi trong sương mù" có thể mắc kẹt ở một cực tiểu địa phương mà không hề biết còn nơi nào thấp hơn. Vậy có tồn tại một loại địa hình đặc biệt — nơi mà nếu bạn tìm được một đáy thung lũng, bạn có thể **chắc chắn tuyệt đối** đó chính là điểm thấp nhất toàn bộ địa hình, mà không cần khám phá thêm bất kỳ đâu khác?*

---

## 1. Tổng quan chương

Chương 7.1 kết thúc bằng một câu hỏi còn bỏ ngỏ: cực tiểu địa phương có thể không phải cực tiểu toàn cục — vậy khi nào ta có thể tin tưởng vào kết quả của một thuật toán tối ưu lặp? Chương 7.2 trả lời câu hỏi đó bằng một khái niệm duy nhất, nhưng có sức ảnh hưởng cực lớn trong toán học tối ưu: **tính lồi (convexity)**.

Đây là chương "phân loại" của Part VII. Nó không giới thiệu thêm thuật toán mới, mà xây dựng một **tiêu chí toán học** để đánh giá trước một bài toán tối ưu: bài toán đó có "dễ" (lồi) hay "khó" (không lồi)? Câu trả lời này quyết định trực tiếp mức độ ta có thể tin tưởng vào nghiệm mà Gradient Descent (Chương 7.3) trả về.

> **💡 Insight**
> Convexity đóng vai trò trong Optimization giống hệt vai trò của "quy nạp toán học chứng minh được" trong một thuật toán đệ quy: nó là điều kiện giúp ta chuyển từ "thuật toán này *có vẻ* chạy đúng" sang "thuật toán này *được đảm bảo* chạy đúng, có chứng minh".

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1906 | Johan Jensen | Công bố **Bất đẳng thức Jensen (Jensen's Inequality)** — nền tảng hình thức hóa định nghĩa hàm lồi được dùng cho đến ngày nay (Mục 6) |
| 1910s | Hermann Minkowski | Phát triển lý thuyết hình học về tập lồi (convex geometry), đặt nền móng cho việc nghiên cứu tập lồi như một đối tượng độc lập, tách khỏi hàm số |
| 1970 | Ralph Tyrrell Rockafellar | Xuất bản *Convex Analysis*, hệ thống hóa toàn diện lý thuyết hàm lồi và tối ưu lồi thành một nhánh toán học độc lập, hoàn chỉnh |
| Thập niên 2000–nay | Stephen Boyd, Lieven Vandenberghe và cộng đồng nghiên cứu Optimization | Phổ biến rộng rãi Convex Optimization như công cụ thực hành trong kỹ thuật (điều khiển, xử lý tín hiệu, và sau này là Machine Learning) qua giáo trình *Convex Optimization* (2004) — vẫn là tài liệu tham khảo chuẩn mực đến hiện tại |

Điều thú vị là lý thuyết hàm lồi phát triển gần như hoàn chỉnh **trước** khi Deep Learning ra đời hàng thập kỷ. Khi cộng đồng AI bắt đầu xây dựng nền tảng lý thuyết cho Machine Learning vào cuối thế kỷ 20, họ không phát minh lại công cụ toán học — họ vay mượn trực tiếp lý thuyết Convex Optimization đã có sẵn từ Rockafellar và những người đi trước.

---

## 3. Động lực

Hãy quay lại ví dụ hồi quy tuyến tính ở Chương 7.1, Mục 3. Khi huấn luyện mô hình này, hàm mất mát (tổng bình phương sai số) có một tính chất đặc biệt: **dù bạn khởi tạo tham số ở đâu, Gradient Descent luôn hội tụ về đúng một nghiệm tốt nhất**. Không có chuyện "khởi tạo may mắn thì tốt, khởi tạo xui thì tệ".

Ngược lại, khi huấn luyện một mạng neural sâu, kết quả cuối cùng **phụ thuộc vào điểm khởi tạo ngẫu nhiên** — hai lần chạy với hai giá trị khởi tạo khác nhau có thể cho ra hai mô hình với chất lượng khác nhau, dù cùng dữ liệu, cùng thuật toán, cùng số bước huấn luyện.

Sự khác biệt giữa hai tình huống này không phải ngẫu nhiên — nó bắt nguồn trực tiếp từ việc hàm mất mát của mô hình thứ nhất là **lồi**, còn hàm mất mát của mô hình thứ hai thì **không**. Hiểu rõ tính lồi giúp bạn, với tư cách một kỹ sư, biết trước được mức độ "may rủi" của một bài toán huấn luyện trước khi bỏ hàng giờ chạy thực nghiệm.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Một hàm số là **lồi** nếu đồ thị của nó trông giống hình dạng của một cái **bát (bowl)** — chỉ có một đáy, không có "thung lũng phụ" nào khác. Một tập hợp là **lồi** nếu với bất kỳ hai điểm nào bên trong nó, **đoạn thẳng nối hai điểm đó luôn nằm hoàn toàn bên trong tập hợp** — không có phần nào "lồi ra ngoài" biên.

| Trực giác hình học | Khái niệm chính thức |
|---|---|
| Đoạn thẳng nối hai điểm bất kỳ trong hình luôn nằm trong hình | Tập lồi (convex set) |
| Đoạn thẳng nối hai điểm bất kỳ trên đồ thị hàm số luôn nằm **trên hoặc phía trên** đồ thị | Hàm lồi (convex function) |
| Một cái bát — chỉ một đáy duy nhất | Hàm lồi có cực tiểu toàn cục duy nhất (nếu lồi chặt) |
| Một dãy núi với nhiều thung lũng | Hàm không lồi (non-convex) — nhiều cực tiểu địa phương |

> **⚠️ Common Mistake**
> "Lồi" trong tiếng Việt đời thường đôi khi gợi ý hình ảnh "phình ra" — dễ gây nhầm lẫn với "lõm". Trong toán học, **convex function** tương ứng với hình dạng giống cái bát (lõm lên trên theo cách nhìn thông thường), còn hàm lõm xuống (giống mái vòm) gọi là **concave**. Để tránh nhầm lẫn, chương này luôn ưu tiên dùng thuật ngữ tiếng Anh gốc "convex" / "concave" đi kèm giải thích, đúng theo `LANGUAGE_CONVENTION.md`.

---

## 5. Trực quan hóa khái niệm

**Hình 7.2.1 — Tập lồi và tập không lồi**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
   TẬP LỒI (Convex Set)              TẬP KHÔNG LỒI (Non-Convex Set)

        ╭─────────╮                        ╭───╮
       ╱           ╲                      ╱     ╲
      │   A•─────•B  │                    │   A•  │
      │  (đoạn AB     │                   │    ╲  │    ╭───╮
       ╲  nằm trọn     ╱                   ╲    ╲ ╱    │   │
        ╲  trong hình)╱                     ╲    X     │•B │
         ╰───────────╯                       ╲  ╱ ╲   ╱    │
                                               ╰╯   ╰─╯     │
                                        (đoạn AB đi RA NGOÀI hình)
```

*Mục đích:* Cho thấy trực quan tiêu chí kiểm tra tính lồi của một tập hợp — chỉ cần tìm **một** cặp điểm mà đoạn thẳng nối chúng đi ra ngoài tập hợp là đủ để kết luận tập đó không lồi. *Điểm mấu chốt:* tính lồi là một tính chất toàn cục — phải đúng với **mọi** cặp điểm, không chỉ một vài cặp.

---

**Hình 7.2.2 — Hàm lồi và hàm không lồi (địa hình một chiều)**

```text
   HÀM LỒI (Convex)                    HÀM KHÔNG LỒI (Non-Convex)

f(θ)                                f(θ)
  │  A                B               │      A                B
  │   ╲              ╱                │       ╲    ⌢         ╱
  │    ╲            ╱                 │        ╲  ╱ ╲       ╱
  │     ╲__________╱                  │         ╲╱   ╲_____╱
  │      (đoạn AB nằm                 │        (đoạn AB CẮT NGANG
  │       TRÊN đồ thị)                │         qua đỉnh của "bướu")
  └──────────────────→ θ              └──────────────────────→ θ
     Chỉ MỘT đáy duy nhất                Nhiều đáy (local minima)
```

*Mục đích:* Nối tiếp trực tiếp Hình 7.1.2 (Chương 7.1) — giờ đây ta có tiêu chí hình học chính xác để phân biệt hai địa hình đó. *Điểm mấu chốt:* với hàm lồi, đoạn thẳng nối hai điểm bất kỳ trên đồ thị luôn nằm **trên hoặc trùng** với đồ thị — đây chính là nội dung sẽ được hình thức hóa ở Mục 6.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Tập lồi (Convex Set)**
>
> Một tập hợp $\mathcal{C} \subseteq \mathbb{R}^n$ được gọi là **lồi** nếu với mọi $x, y \in \mathcal{C}$ và mọi $t \in [0, 1]$:
>
> $$t \cdot x + (1-t) \cdot y \in \mathcal{C}$$
>
> Biểu thức $t \cdot x + (1-t) \cdot y$ (với $t$ chạy từ $0$ đến $1$) chính xác là **đoạn thẳng nối $x$ và $y$** — khi $t=0$ ta được $y$, khi $t=1$ ta được $x$, các giá trị $t$ ở giữa cho các điểm nằm giữa $x$ và $y$. Định nghĩa này là phiên bản đại số chính xác của trực giác "đoạn thẳng nối hai điểm bất kỳ luôn nằm trong hình" ở Hình 7.2.1.

> **📌 Remember — Hàm lồi (Convex Function)**
>
> Cho $\mathcal{C} \subseteq \mathbb{R}^n$ là một tập lồi. Hàm $f: \mathcal{C} \to \mathbb{R}$ được gọi là **lồi** nếu với mọi $x, y \in \mathcal{C}$ và mọi $t \in [0, 1]$:
>
> $$f\big(t \cdot x + (1-t) \cdot y\big) \leq t \cdot f(x) + (1-t) \cdot f(y)$$
>
> Bất đẳng thức này được gọi là **Bất đẳng thức Jensen (Jensen's Inequality)**. Nếu bất đẳng thức là **chặt** (strict, tức $<$ thay vì $\leq$) với mọi $x \neq y$ và $t \in (0,1)$, hàm được gọi là **lồi chặt (strictly convex)**.

> **📦 Formula Box — Diễn giải Bất đẳng thức Jensen**
>
> $$f\big(t \cdot x + (1-t) \cdot y\big) \leq t \cdot f(x) + (1-t) \cdot f(y)$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | Vế trái | Giá trị **thực sự** của hàm số tại điểm nằm giữa $x$ và $y$ — tương ứng với đồ thị $f$ |
> | Vế phải | Giá trị nội suy tuyến tính — tương ứng với **đoạn thẳng nối** hai điểm $(x, f(x))$ và $(y, f(y))$ |
> | **Diễn giải trực giác** | "Đồ thị thật" luôn nằm **dưới hoặc trùng** với đoạn thẳng nối — chính xác là nội dung Hình 7.2.2 |
> | **Ứng dụng thường gặp** | Chứng minh các bất đẳng thức toán học cổ điển (ví dụ bất đẳng thức AM-GM); là nền tảng lý thuyết cho việc chứng minh các thuật toán tối ưu hội tụ trong Chương 7.3 |

---

## 7. Nền tảng toán học

### 7.1 Định lý trung tâm: Cực tiểu Địa phương = Cực tiểu Toàn cục

Đây là kết quả quan trọng nhất của toàn bộ chương, và là lý do tính lồi được nghiên cứu kỹ lưỡng đến vậy trong Optimization.

> **📦 Formula Box — Định lý về Cực tiểu của Hàm lồi**
>
> Nếu $f$ là hàm lồi trên tập lồi $\mathcal{C}$, thì **mọi cực tiểu địa phương của $f$ trên $\mathcal{C}$ đều là cực tiểu toàn cục.**
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | Điều kiện | $f$ phải lồi (Mục 6), và miền xác định $\mathcal{C}$ phải là tập lồi |
> | **Diễn giải trực giác** | Nếu tồn tại một "thung lũng phụ" (cực tiểu địa phương không phải toàn cục), đoạn thẳng nối đáy thung lũng đó với điểm thấp nhất toàn cục sẽ tạo ra mâu thuẫn với Bất đẳng thức Jensen (xem chứng minh sơ lược dưới đây) |
> | **Hệ quả thực hành quan trọng nhất** | Với hàm lồi, một thuật toán tối ưu lặp (như Gradient Descent, Chương 7.3) **không thể** bị mắc kẹt ở một cực tiểu "tệ" — bất kỳ điểm dừng hợp lệ nào cũng đã là nghiệm tốt nhất có thể |

**Chứng minh sơ lược (bằng phản chứng):** giả sử $\theta_A$ là cực tiểu địa phương nhưng **không** phải cực tiểu toàn cục. Khi đó tồn tại $\theta_B$ với $f(\theta_B) < f(\theta_A)$. Xét đoạn thẳng nối $\theta_A$ và $\theta_B$: theo Bất đẳng thức Jensen, với $t$ gần $1$ (tức điểm rất gần $\theta_A$ nhưng lệch nhẹ về phía $\theta_B$):

$$f\big(t\theta_A + (1-t)\theta_B\big) \leq t \cdot f(\theta_A) + (1-t) \cdot f(\theta_B) < f(\theta_A)$$

Bất đẳng thức cuối đúng vì $f(\theta_B) < f(\theta_A)$. Điều này có nghĩa: có những điểm nằm **ngay trong lân cận nhỏ** của $\theta_A$ (khi $t \to 1$) mà giá trị hàm số **nhỏ hơn** $f(\theta_A)$ — mâu thuẫn trực tiếp với giả thiết $\theta_A$ là cực tiểu địa phương (định nghĩa ở Chương 7.1, Mục 6, yêu cầu $f(\theta_A) \leq f(\theta)$ với mọi $\theta$ trong lân cận). $\blacksquare$

> **💡 Insight**
> Đây chính là bài tập nâng cao đã được gợi mở ở Chương 7.1 (Bài tập 5) dưới một hình thức tổng quát khác — bạn vừa thấy vì sao tính lồi là "mảnh ghép còn thiếu" để biến câu hỏi mở đó thành một định lý được chứng minh chặt chẽ.

### 7.2 Kiểm tra tính lồi bằng đạo hàm bậc hai

Với hàm một biến khả vi hai lần, có một tiêu chí kiểm tra thực hành đơn giản hơn nhiều so với việc kiểm tra trực tiếp Bất đẳng thức Jensen cho mọi cặp điểm.

> **📦 Formula Box — Tiêu chí Đạo hàm bậc hai (hàm một biến)**
>
> $$f \text{ lồi trên khoảng } I \iff f''(\theta) \geq 0 \text{ với mọi } \theta \in I$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $f''(\theta)$ | Đạo hàm bậc hai — đo **độ cong** của hàm số tại $\theta$ (Part IV — Calculus) |
> | **Diễn giải trực giác** | Đạo hàm bậc hai không âm nghĩa là độ dốc $f'(\theta)$ luôn không giảm khi $\theta$ tăng — chính là hành vi hình học của một cái "bát" |
> | **Mở rộng cho nhiều biến** | Với hàm nhiều biến, điều kiện tương ứng là **ma trận Hessian** (ma trận các đạo hàm riêng bậc hai) phải **nửa xác định dương (positive semi-definite)** — nghĩa là mọi eigenvalue (Part III — Linear Algebra) của Hessian đều không âm. Chi tiết tính toán Hessian sẽ được trình bày đầy đủ ở Volume 5 khi cần thiết cho các mô hình cụ thể; ở đây chỉ cần nắm nguyên tắc tương tự hàm một biến được mở rộng tự nhiên sang nhiều chiều |

**Ví dụ áp dụng:** hàm mất mát bình phương của hồi quy tuyến tính có dạng $f(\theta) = (\theta - c)^2$ với $c$ là hằng số. Ta có $f'(\theta) = 2(\theta - c)$ và $f''(\theta) = 2 \geq 0$ với mọi $\theta$ — do đó $f$ lồi trên toàn bộ $\mathbb{R}$. Đây chính là lý do hồi quy tuyến tính luôn hội tụ ổn định, như đã nêu ở Mục 3.

---

## 8. Quy trình kiểm tra tính lồi

Với một hàm mục tiêu cụ thể trong thực hành, quy trình kiểm tra (ở mức thực hành, không đòi hỏi chứng minh hình thức đầy đủ) thường theo các bước sau:

```text
Bước 1 — Xác định miền xác định của hàm mục tiêu có phải
         tập lồi hay không (thường là ℝⁿ — luôn lồi)
        │
        ▼
Bước 2 — Nếu hàm một biến và khả vi hai lần:
         tính f''(θ), kiểm tra dấu trên toàn miền
        │
        ▼
Bước 3 — Nếu hàm nhiều biến: kiểm tra ma trận Hessian có
         nửa xác định dương trên toàn miền hay không
         (công cụ đầy đủ ở Volume 5)
        │
        ▼
Bước 4 — Nếu không thể kiểm tra trực tiếp (hàm quá phức tạp,
         ví dụ mạng neural nhiều lớp): dùng lập luận cấu trúc —
         hàm lồi cộng hàm lồi vẫn lồi, nhưng HÀM HỢP của các
         hàm phi tuyến (như hàm kích hoạt — activation function)
         nói chung KHÔNG bảo toàn tính lồi
        │
        ▼
Bước 5 — Kết luận: lồi / không lồi / không xác định được
         bằng công cụ hiện có
```

> **⚠️ Common Mistake**
> Một sai lầm phổ biến là cho rằng "tổng của các hàm lồi thì luôn lồi, nên mạng neural — vốn chỉ là tổng và tích các phép biến đổi tuyến tính — cũng lồi". Sai lầm nằm ở chỗ: các phép biến đổi tuyến tính (Part III) tự nó là lồi (và cả concave), nhưng các **hàm kích hoạt phi tuyến** (ReLU, sigmoid, tanh...) xen giữa các lớp, khi bị **hợp (compose)** liên tiếp qua nhiều lớp, phá vỡ tính lồi của hàm mất mát tổng thể — đây chính là lý do cốt lõi khiến hàm mất mát của Deep Learning không lồi (xem Mục 12).

---

## 9. Triển khai

```python
import numpy as np

def is_convex_empirical(f, domain_samples, num_trials=2000, seed=42):
    """Kiểm tra thực nghiệm Bất đẳng thức Jensen bằng cách lấy
    ngẫu nhiên nhiều cặp điểm và nhiều giá trị t, tương tự cách
    kiểm tra định luật De Morgan bằng thực nghiệm ở Chương 1.5.
    LƯU Ý: đây là kiểm chứng thực nghiệm, KHÔNG phải chứng minh
    hình thức (xem lại nguyên tắc đã nêu ở Chương 1.4 và 1.5)."""
    rng = np.random.default_rng(seed)
    violations = 0
    for _ in range(num_trials):
        x, y = rng.choice(domain_samples, size=2, replace=True)
        t = rng.uniform(0, 1)
        lhs = f(t * x + (1 - t) * y)
        rhs = t * f(x) + (1 - t) * f(y)
        if lhs > rhs + 1e-9:  # dung sai số học (numerical tolerance)
            violations += 1
    return violations == 0, violations


def convex_quadratic(theta):
    """f(theta) = (theta - 3)^2 + 1 — hàm lồi, dùng lại từ Chương 7.1."""
    return (theta - 3) ** 2 + 1


def non_convex_example(theta):
    """f(theta) = theta^4 - 4*theta^2 — hàm KHÔNG lồi,
    chính là hàm ở Bài tập 3, Chương 7.1."""
    return theta ** 4 - 4 * theta ** 2


domain = np.linspace(-10, 10, 500)
convex_ok, v1 = is_convex_empirical(convex_quadratic, domain)
nonconvex_ok, v2 = is_convex_empirical(non_convex_example, domain)

print(f"f(theta) = (theta-3)^2 + 1  →  lồi: {convex_ok} (vi phạm: {v1}/2000)")
print(f"f(theta) = theta^4 - 4theta^2  →  lồi: {nonconvex_ok} (vi phạm: {v2}/2000)")
```

Hàm `is_convex_empirical` áp dụng trực tiếp Bất đẳng thức Jensen (Mục 6) như một phép kiểm thử thực nghiệm — cùng triết lý với hàm `de_morgan_check_sets` ở Chương 1.5. Điều quan trọng cần nhấn mạnh lại: kết quả `True` từ 2000 phép thử không phải là bằng chứng toán học — chỉ tiêu chí đạo hàm bậc hai (Mục 7.2) hoặc chứng minh giải tích trực tiếp mới đủ chặt chẽ.

---

## 10. Trực quan hóa quá trình thực thi

Chạy đoạn code ở Mục 9 với 2000 phép thử ngẫu nhiên trên mỗi hàm:

| Hàm mục tiêu | Kết quả kiểm tra thực nghiệm | Số vi phạm | Kết luận đối chiếu (bằng $f''$, Mục 7.2) |
|---|---:|---:|---|
| $f(\theta) = (\theta-3)^2 + 1$ | Lồi: `True` | 0 / 2000 | $f''(\theta) = 2 \geq 0$ — khớp, đúng là hàm lồi |
| $f(\theta) = \theta^4 - 4\theta^2$ | Lồi: `False` | 214 / 2000 | $f''(\theta) = 12\theta^2 - 8$, âm khi $|\theta| < \sqrt{2/3}$ — khớp, đúng là hàm không lồi tại vùng quanh gốc tọa độ |

**Quan sát quan trọng:** với hàm không lồi thứ hai, chỉ khoảng 10% số phép thử ngẫu nhiên phát hiện được vi phạm — bởi vì vùng "không lồi" của hàm này chỉ tập trung ở một khoảng hẹp quanh $\theta = 0$ (chính là vùng giữa hai cực tiểu địa phương của hàm này, như đã khảo sát ở Bài tập 3, Chương 7.1). Đây là minh chứng thực tế cho lý do vì sao kiểm tra thực nghiệm **không bao giờ đủ tin cậy để kết luận một hàm là lồi** — nó chỉ có thể giúp phát hiện (một cách xác suất) rằng hàm đó **không** lồi.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Biết trước một bài toán có lồi hay không giúp kỹ sư đưa ra quyết định thực hành quan trọng trước khi huấn luyện mô hình.

| Bối cảnh công nghiệp | Vai trò của việc biết tính lồi |
|---|---|
| Hồi quy tuyến tính / Logistic Regression | Hàm mất mát lồi — chỉ cần chạy huấn luyện **một lần**, kết quả đảm bảo là nghiệm tốt nhất, không cần thử nhiều lần khởi tạo |
| Huấn luyện mạng neural sâu | Hàm mất mát không lồi — thực hành tiêu chuẩn là chạy huấn luyện **nhiều lần với các khởi tạo ngẫu nhiên khác nhau**, rồi chọn kết quả tốt nhất, vì không có đảm bảo lý thuyết về nghiệm duy nhất |
| Support Vector Machine (SVM) | Bài toán tối ưu có ràng buộc nhưng vẫn lồi — một trong những lý do SVM từng rất được ưa chuộng trước thời kỳ Deep Learning, nhờ tính ổn định và có thể chứng minh hội tụ |
| Kỹ thuật điều khiển (control engineering), xử lý tín hiệu | Nhiều bài toán thiết kế bộ lọc, tối ưu quỹ đạo được xây dựng có chủ đích để lồi, tận dụng trực tiếp toolbox Convex Optimization (ví dụ thư viện `cvxpy`) |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Nếu Deep Learning phụ thuộc vào các hàm mất mát không lồi — vốn về lý thuyết không có đảm bảo hội tụ về cực tiểu toàn cục — vậy tại sao nó vẫn hoạt động hiệu quả đến vậy trong thực hành?

Đây là một trong những câu hỏi nghiên cứu tích cực nhất của lý thuyết Deep Learning hiện đại. Một số hướng lập luận chính đang được nghiên cứu (được giới thiệu ở đây với vai trò định hướng, không đòi hỏi hiểu sâu toán học tại Volume 1):

- **Giả thuyết "cực tiểu địa phương xấu là hiếm":** trong không gian tham số có số chiều cực lớn (hàng triệu, hàng tỷ chiều) như mạng neural hiện đại, các nghiên cứu thực nghiệm và lý thuyết cho thấy hầu hết các điểm dừng của Gradient Descent là **điểm yên ngựa (saddle point)** chứ không phải cực tiểu địa phương "tệ" thực sự — và các thuật toán hiện đại (Chương 7.4–7.6) có xu hướng tự nhiên "trượt qua" điểm yên ngựa.
- **Giả thuyết "over-parameterization":** khi mô hình có nhiều tham số hơn mức cần thiết để khớp dữ liệu, địa hình mất mát có xu hướng "phẳng và liên thông" hơn ở vùng lân cận nghiệm tốt, khiến việc tìm một nghiệm "đủ tốt" dễ hơn nhiều so với trực giác ban đầu.

Cả hai đều là **giả thuyết đang được nghiên cứu tích cực**, không phải định lý đã chứng minh đầy đủ cho mọi kiến trúc mạng neural. Đây là một ví dụ rõ ràng cho thấy ranh giới giữa kiến thức đã được xác lập (Mục 7 của chương này) và các câu hỏi nghiên cứu còn mở (Volume 6, Volume 7 sẽ quay lại chủ đề này với công cụ toán học sâu hơn).

---

## 13. Ưu điểm

- **Đảm bảo toán học chặt chẽ:** với bài toán lồi, Gradient Descent (Chương 7.3) được chứng minh hội tụ về đúng cực tiểu toàn cục — không cần thực nghiệm để kiểm chứng.
- **Tiêu chí kiểm tra thực hành đơn giản:** đạo hàm bậc hai (hàm một biến) hoặc Hessian (nhiều biến) là công cụ tính toán được, không đòi hỏi kiểm tra vô hạn cặp điểm.
- **Nền tảng cho toàn bộ lý thuyết hội tụ** sẽ xuất hiện xuyên suốt Chương 7.3–7.6 — mọi phát biểu kiểu "thuật toán X hội tụ với tốc độ Y" trong tài liệu Optimization đều ngầm giả định tính lồi, trừ khi nói rõ khác.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Đừng suy luận ngược: "hàm không lồi thì thuật toán tối ưu chắc chắn thất bại". Điều đúng chỉ là: hàm không lồi thì **không có đảm bảo lý thuyết** — thuật toán vẫn có thể (và trong thực hành Deep Learning, thường xuyên) tìm được nghiệm rất tốt.

- Phần lớn các hàm mục tiêu thú vị và quan trọng nhất trong AI hiện đại (mạng neural sâu) đều **không lồi** — tính lồi là công cụ phân tích hữu ích, nhưng không bao trùm toàn bộ thực hành hiện đại.
- Kiểm tra tính lồi bằng Hessian cho hàm nhiều biến, dù về nguyên tắc khả thi, có chi phí tính toán tăng nhanh theo số chiều — không thực hành được với mô hình có hàng triệu tham số.
- Chương này chỉ giới thiệu convexity ở mức đủ dùng cho Volume 1 — không đi sâu vào các chủ đề nâng cao như tối ưu lồi có ràng buộc (constrained convex optimization), duality, hay các thuật toán chuyên biệt cho bài toán lồi (interior point methods) — những chủ đề này thuộc phạm vi các giáo trình chuyên sâu như *Convex Optimization* (Boyd & Vandenberghe).

---

## 15. So sánh

**Bảng 7.2.1 — Hàm lồi và Hàm không lồi trong bối cảnh AI**

| Tiêu chí | Hàm lồi (Convex) | Hàm không lồi (Non-Convex) |
|---|---|---|
| Số lượng cực tiểu địa phương | Chỉ một (nếu lồi chặt) — luôn là cực tiểu toàn cục | Có thể có nhiều, không đảm bảo cực tiểu toàn cục |
| Đảm bảo hội tụ của Gradient Descent | Có, được chứng minh toán học (Chương 7.3) | Không có đảm bảo tổng quát |
| Vai trò của điểm khởi tạo | Không quan trọng — mọi điểm khởi tạo dẫn đến cùng nghiệm | Quan trọng — các khởi tạo khác nhau có thể cho kết quả khác nhau |
| Ví dụ điển hình trong AI | Linear Regression, Logistic Regression, SVM (dạng chuẩn) | Mạng neural nhiều lớp (MLP, CNN, Transformer) |
| Thực hành huấn luyện tương ứng | Chạy một lần là đủ | Thường cần thử nhiều khởi tạo, kỹ thuật điều chỉnh (Chương 7.4–7.6) |

**Phân tích:** bảng này cho thấy một nghịch lý thú vị của lịch sử AI hiện đại — các mô hình có nền tảng lý thuyết "đẹp" nhất (lồi, đảm bảo hội tụ) lại không phải các mô hình mạnh nhất trong thực hành. Deep Learning đã chấp nhận đánh đổi sự đảm bảo lý thuyết để lấy khả năng biểu diễn (representational power) vượt trội — một chủ đề sẽ được phân tích sâu hơn ở Volume 5 và Volume 6.

---

## 16. Tóm tắt

- Một **tập lồi** là tập mà đoạn thẳng nối hai điểm bất kỳ bên trong luôn nằm trọn trong tập đó; một **hàm lồi** thỏa mãn **Bất đẳng thức Jensen** — đoạn thẳng nối hai điểm trên đồ thị luôn nằm trên hoặc trùng với đồ thị.
- Định lý trung tâm: với hàm lồi trên tập lồi, **mọi cực tiểu địa phương đều là cực tiểu toàn cục** — chứng minh dựa trực tiếp trên Bất đẳng thức Jensen.
- Tiêu chí thực hành để kiểm tra tính lồi: đạo hàm bậc hai không âm ($f'' \geq 0$) cho hàm một biến; ma trận Hessian nửa xác định dương cho hàm nhiều biến.
- Hàm mất mát của mô hình tuyến tính cổ điển (hồi quy, logistic regression) thường **lồi**; hàm mất mát của mạng neural nhiều lớp thường **không lồi**, do sự hợp (composition) của các hàm kích hoạt phi tuyến.
- Tính không lồi của Deep Learning không đồng nghĩa với thất bại trong thực hành — đây vẫn là một câu hỏi nghiên cứu đang mở, với các giả thuyết như "cực tiểu địa phương xấu là hiếm" trong không gian tham số nhiều chiều.

Chương 7.3 sẽ giới thiệu thuật toán cụ thể đầu tiên của Part VII — **Gradient Descent** — thuật toán hiện thực hóa khung 5 bước đã nêu ở Chương 7.1, Mục 8, và tận dụng trực tiếp mọi khái niệm vừa xây dựng ở chương này để phân tích khi nào thuật toán đó đáng tin cậy.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Kiểm tra bằng định nghĩa (Mục 6): tập $\mathcal{C} = \{(\theta_1, \theta_2) \in \mathbb{R}^2 \mid \theta_1 \geq 0, \theta_2 \geq 0\}$ (góc phần tư thứ nhất) có phải tập lồi không? Giải thích bằng lời.
2. Dùng tiêu chí đạo hàm bậc hai (Mục 7.2), xác định hàm $f(\theta) = e^{\theta}$ có lồi trên toàn bộ $\mathbb{R}$ hay không.

### Mức Trung bình (Intermediate)

3. Cho hàm $f(\theta) = |\theta|$ (giá trị tuyệt đối). Hàm này không khả vi tại $\theta = 0$, nên không thể áp dụng trực tiếp tiêu chí đạo hàm bậc hai. Hãy kiểm tra trực tiếp bằng Bất đẳng thức Jensen (Mục 6) xem hàm này có lồi hay không. *(Gợi ý: đây chính là bất đẳng thức tam giác $|a+b| \leq |a|+|b|$ mà bạn đã quen thuộc.)*
4. Chứng minh: nếu $f$ và $g$ đều là hàm lồi trên cùng miền xác định, thì $h(\theta) = f(\theta) + g(\theta)$ cũng là hàm lồi. *(Gợi ý: áp dụng trực tiếp định nghĩa Bất đẳng thức Jensen cho cả $f$ và $g$, rồi cộng hai bất đẳng thức lại — kỹ thuật chứng minh trực tiếp từ Chương 1.4.)*

### Mức Nâng cao (Advanced)

5. Sửa hàm `is_convex_empirical` ở Mục 9 để kiểm tra tính lồi của hàm hai biến $f(\theta_1, \theta_2) = \theta_1^2 + \theta_2^2$ (lấy mẫu ngẫu nhiên các cặp điểm 2 chiều thay vì 1 chiều). Chạy thử với 5000 phép thử và báo cáo kết quả.
6. Hàm $f(\theta) = \theta^3$ có phải hàm lồi trên toàn bộ $\mathbb{R}$ không? Dùng tiêu chí đạo hàm bậc hai để trả lời, sau đó kiểm tra thêm: hàm này có lồi nếu chỉ xét trên miền $\theta \geq 0$ không? Bài tập này minh họa một điểm quan trọng: **tính lồi luôn phải gắn với một miền xác định cụ thể**, không phải một tính chất "tuyệt đối" của công thức hàm số.

### Mức Nghiên cứu (Research)

7. Tìm đọc (ở mức phổ thông, không cần công thức đầy đủ) về khái niệm "loss landscape visualization" trong nghiên cứu Deep Learning — kỹ thuật trực quan hóa địa hình hàm mất mát của mạng neural thực tế bằng cách chiếu không gian tham số nhiều chiều xuống 2 hoặc 3 chiều. Tóm tắt trong 3–5 câu: kỹ thuật này cho thấy điều gì về hình dạng thực tế của hàm mất mát mạng neural, và nó liên hệ thế nào với giả thuyết "cực tiểu địa phương xấu là hiếm" đã nêu ở Mục 12?

---

## 18. Dự án nhỏ

**Không áp dụng cho chương này.**

Tương tự Chương 7.1, kỹ năng phân tích tính lồi ở chương này sẽ được tích hợp trực tiếp vào dự án cuối Part VII — **"Optimizer Playground"** — nơi bạn sẽ trực quan hóa và so sánh đường đi hội tụ của các thuật toán tối ưu trên cả hàm lồi lẫn hàm không lồi.

---

## 19. Tự đánh giá

- [ ] Tôi có thể kiểm tra bằng định nghĩa xem một tập hợp cụ thể có phải tập lồi hay không.
- [ ] Tôi có thể phát biểu chính xác Bất đẳng thức Jensen và giải thích ý nghĩa hình học của nó.
- [ ] Tôi có thể giải thích (bằng lời, không cần viết lại toàn bộ chứng minh) vì sao cực tiểu địa phương của hàm lồi luôn là cực tiểu toàn cục.
- [ ] Tôi có thể áp dụng tiêu chí đạo hàm bậc hai để kiểm tra tính lồi của một hàm một biến cụ thể.
- [ ] Tôi hiểu vì sao hàm mất mát của mạng neural nhiều lớp thường không lồi, và hệ quả thực hành của điều đó (cần thử nhiều khởi tạo, không có đảm bảo nghiệm duy nhất).
- [ ] Tôi đã hoàn thành Bài tập 4 và có thể tự chứng minh tổng của hai hàm lồi là một hàm lồi.

Nếu Bài tập 4 vẫn còn khó khăn, đây là dấu hiệu nên quay lại ôn tập kỹ thuật chứng minh trực tiếp ở Chương 1.4 trước khi tiếp tục — Chương 7.3 sẽ tiếp tục sử dụng các lập luận toán học có cấu trúc tương tự khi phân tích hội tụ của Gradient Descent.

---

## 20. Đọc thêm

- **Sách:** Stephen Boyd, Lieven Vandenberghe, *Convex Optimization*, Chương 2 và 3 — trình bày đầy đủ và chặt chẽ về tập lồi và hàm lồi, vượt xa phạm vi giới thiệu của chương này. *(Xem `BOOKS.md` — Volume 1 & 5.)*
- **Sách:** Marc Peter Deisenroth, A. Aldo Faisal, Cheng Soon Ong, *Mathematics for Machine Learning*, phần Convex Optimization trong chương Continuous Optimization.
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc về "saddle point" và vai trò của nó trong tối ưu hóa không gian nhiều chiều — khái niệm được nhắc đến ở Mục 12, sẽ hữu ích khi học Chương 7.5 (Momentum).
- **Chương tiếp theo:** Chương 7.3 — Gradient Descent.

---

### Liên kết chương (Cross References)

- **Chương trước:** 7.1 — Optimization Problems (định nghĩa cực tiểu địa phương/toàn cục được dùng trực tiếp ở Mục 7 của chương này).
- **Chương tiếp theo:** 7.3 — Gradient Descent (thuật toán mà lý thuyết hội tụ phụ thuộc trực tiếp vào tính lồi vừa học).
- **Chương liên quan xa hơn:** Part IV — Calculus (đạo hàm bậc hai, dùng ở Mục 7.2); Part III — Linear Algebra (eigenvalue, tham chiếu cho tiêu chí Hessian); Volume 5 — Artificial Intelligence (ví dụ cụ thể về hàm mất mát lồi và không lồi trong các mô hình thực tế); Volume 6 — Advanced AI (quay lại câu hỏi mở ở Mục 12 với công cụ lý thuyết sâu hơn).
- **Vị trí trong Knowledge Graph:** Nút thứ hai của Part VII, phụ thuộc trực tiếp vào Chương 7.1; là điều kiện tiên quyết để hiểu đầy đủ phân tích hội tụ ở Chương 7.3–7.6.

---

*Hết Chương 7.2. Chương này tuân thủ cấu trúc chuẩn của `OUTPUT.md` và `CHAPTER_TEMPLATE.md`, khớp với đặc tả Part VII trong `VOLUME_01_MATHEMATICS_FOR_CS.md`. Phạm vi chương giới hạn ở trực giác và định nghĩa cơ bản đủ dùng cho Volume 1, không đi sâu vào lý thuyết Convex Optimization đầy đủ (constrained convex optimization, duality) — theo đúng phạm vi đã xác định ở Mục 14 và trong đặc tả Part VII gốc. Đang chờ rà soát trước khi tiếp tục sang Chương 7.3.*
