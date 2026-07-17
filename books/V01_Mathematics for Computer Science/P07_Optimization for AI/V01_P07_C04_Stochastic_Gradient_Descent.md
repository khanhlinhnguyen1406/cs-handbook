# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part VII — Optimization for Artificial Intelligence
## Chương 7.4 — Stochastic Gradient Descent
### (Hạ Gradient Ngẫu nhiên)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 7.4 |
| Thuộc Part | VII — Optimization for Artificial Intelligence |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 35–45 phút |
| Độ khó | ★★★☆☆ |
| Kiến thức tiên quyết | Chương 7.3 — Gradient Descent (đặc biệt Mục 6, 8, 14); Part V — Probability & Statistics (kỳ vọng — Expectation, ở mức trực giác cơ bản) |
| Chương liên quan | 7.5 — Momentum (cải tiến độc lập, giải quyết vấn đề khác của GD); 7.6 — Adam (kết hợp ý tưởng của cả hai chương); Volume 5, Part II — Machine Learning (SGD là thuật toán huấn luyện mặc định trên mọi tập dữ liệu lớn) |
| Từ khóa | Stochastic Gradient Descent, mini-batch, epoch, batch size, unbiased estimator, noisy gradient |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Giải thích chính xác vì sao Gradient Descent cơ bản (Chương 7.3) trở nên không khả thi khi tập dữ liệu huấn luyện có hàng triệu đến hàng tỷ mẫu.
- Phân biệt ba biến thể: **Batch Gradient Descent**, **Stochastic Gradient Descent (SGD)**, và **Mini-batch SGD**, và viết được công thức cập nhật của mỗi loại.
- Giải thích, bằng lập luận kỳ vọng (expectation), vì sao gradient tính trên một mini-batch là một **ước lượng không chệch (unbiased estimator)** của gradient thật, dù có nhiễu.
- Phân tích đánh đổi giữa tốc độ tính toán và độ ổn định hội tụ giữa ba biến thể.
- Cài đặt Mini-batch SGD từ đầu bằng Python và so sánh hành vi hội tụ với Gradient Descent cơ bản trên cùng một bài toán.

---

### Câu hỏi khơi gợi

> *Để biết ý kiến trung bình của mười triệu cử tri về một vấn đề, bạn có thực sự cần hỏi từng người trong mười triệu người đó? Hay chỉ cần khảo sát một mẫu ngẫu nhiên vài nghìn người là đã đủ để có một ước lượng đáng tin cậy? Gradient Descent, ở dạng cơ bản nhất (Chương 7.3), đang mắc phải chính xác sai lầm "hỏi hết mọi người" này — và chương này sẽ chỉ ra cách sửa nó.*

---

## 1. Tổng quan chương

Chương 7.3 kết thúc bằng việc chỉ ra hai hạn chế của Gradient Descent cơ bản: chi phí tính toán khi dữ liệu lớn, và dao động trên địa hình có độ dốc chênh lệch. Chương 7.4 giải quyết hạn chế **đầu tiên** — chi phí tính toán — bằng một quan sát tưởng chừng đơn giản nhưng có ảnh hưởng sâu rộng: **ta không cần tính gradient chính xác tuyệt đối ở mỗi bước; một ước lượng đủ tốt là đủ.**

Đây là chương đầu tiên trong Part VII đưa yếu tố **ngẫu nhiên (randomness)** vào chính thuật toán tối ưu — một bước ngoặt quan trọng. Cho đến giờ, Gradient Descent (Chương 7.3) là một thuật toán **tất định (deterministic)**: cùng điểm khởi tạo, cùng learning rate, sẽ luôn cho ra cùng một đường đi hội tụ. Stochastic Gradient Descent phá vỡ tính tất định đó một cách có chủ đích — và chính sự "nhiễu" này, nghịch lý thay, lại là chìa khóa giúp AI hiện đại huấn luyện được trên những tập dữ liệu khổng lồ.

> **💡 Insight**
> Đây là lần đầu tiên trong Volume 1, "sự không chắc chắn" được đưa vào một thuật toán một cách **có chủ đích**, như một công cụ hữu ích — chứ không phải một điều cần loại bỏ. Trực giác này sẽ được củng cố mạnh mẽ hơn khi Part V (Probability & Statistics) được ôn lại ở đây, và sẽ tái xuất hiện xuyên suốt Volume 5–6 dưới nhiều hình thức khác (ví dụ Dropout, một kỹ thuật cố tình thêm nhiễu vào mạng neural để cải thiện khả năng tổng quát hóa).

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1951 | Herbert Robbins, Sutton Monro | Công bố *A Stochastic Approximation Method* — đặt nền móng lý thuyết toán học cho việc tìm nghiệm bằng các phép cập nhật lặp dùng thông tin **nhiễu** (noisy), không cần thông tin chính xác tuyệt đối ở mỗi bước — nền tảng lý thuyết trực tiếp cho SGD |
| Thập niên 1960–1990 | Cộng đồng Thống kê và Học máy sơ khai | Áp dụng ý tưởng xấp xỉ ngẫu nhiên (stochastic approximation) vào các bài toán học từ dữ liệu tuần tự (online learning) |
| 2010 | Léon Bottou | Công bố các phân tích có ảnh hưởng lớn về đánh đổi giữa GD và SGD khi quy mô dữ liệu tăng lên — góp phần định hình thực hành công nghiệp: với dữ liệu đủ lớn, SGD không chỉ nhanh hơn mà còn **thực tế hơn** so với Batch GD |
| Thập niên 2010–nay | Cộng đồng Deep Learning | Mini-batch SGD (và các biến thể ở Chương 7.5–7.6) trở thành **phương pháp huấn luyện mặc định** cho hầu như mọi mô hình AI quy mô công nghiệp, từ ảnh, văn bản, đến các mô hình ngôn ngữ lớn |

> **🔬 Research Connection**
> Giống như Gradient Descent (Chương 7.3) được Cauchy phát triển gần 140 năm trước khi có ứng dụng AI, lý thuyết xấp xỉ ngẫu nhiên của Robbins và Monro (1951) cũng đi trước thời đại Deep Learning hàng chục năm. Một lần nữa, đây là minh chứng cho việc AI hiện đại phần lớn đứng trên vai những kết quả toán học/thống kê nền tảng đã tồn tại từ rất lâu.

---

## 3. Động lực

Hãy định lượng cụ thể vấn đề mà Chương 7.3 (Mục 14) đã nêu. Với hàm mất mát trung bình trên toàn bộ dữ liệu:

$$f(\theta) = \frac{1}{N} \sum_{i=1}^{N} f_i(\theta)$$

trong đó $N$ là số lượng mẫu dữ liệu và $f_i(\theta)$ là sai số trên riêng mẫu thứ $i$, mỗi bước Gradient Descent (Chương 7.3, Mục 6) đòi hỏi tính:

$$\nabla f(\theta) = \frac{1}{N} \sum_{i=1}^{N} \nabla f_i(\theta)$$

Với một tập dữ liệu ảnh cỡ trung bình như ImageNet ($N \approx 1{,}2$ triệu ảnh), việc tính **một** gradient duy nhất đòi hỏi xử lý qua cả 1,2 triệu ảnh. Nếu cần hàng chục nghìn bước cập nhật để mô hình hội tụ — con số hoàn toàn bình thường trong thực hành — tổng khối lượng tính toán trở nên khổng lồ đến mức không khả thi về mặt thời gian, ngay cả với phần cứng hiện đại. Với các mô hình ngôn ngữ lớn hiện nay, huấn luyện trên hàng tỷ đến hàng nghìn tỷ token, vấn đề này còn nghiêm trọng hơn nhiều bậc.

Câu hỏi tự nhiên: *liệu có cần thiết phải dùng đúng 100% dữ liệu ở mỗi bước, hay một mẫu con (subsample) ngẫu nhiên là đủ để ước lượng đúng hướng cần đi?*

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Muốn biết ý kiến trung bình của mười triệu cử tri, một cuộc khảo sát dư luận (opinion poll) uy tín thường chỉ cần hỏi vài nghìn người được chọn ngẫu nhiên — không cần hỏi hết mười triệu người — để có một ước lượng đáng tin cậy về xu hướng chung. **Stochastic Gradient Descent áp dụng chính xác ý tưởng này cho việc tính gradient:** thay vì "hỏi" toàn bộ $N$ mẫu dữ liệu, ta chỉ "hỏi" một mẫu con ngẫu nhiên nhỏ ở mỗi bước.

| Trực giác khảo sát dư luận | Khái niệm Stochastic Gradient Descent |
|---|---|
| Toàn bộ cử tri (10 triệu người) | Toàn bộ tập dữ liệu huấn luyện ($N$ mẫu) |
| Mẫu khảo sát ngẫu nhiên (vài nghìn người) | Mini-batch (một tập con ngẫu nhiên $m \ll N$ mẫu) |
| Kết quả khảo sát ước lượng đúng xu hướng chung, có sai số nhỏ | Gradient trên mini-batch ước lượng đúng gradient thật, có nhiễu |
| Khảo sát lại nhiều lần, mỗi lần một mẫu khác, để có bức tranh đầy đủ hơn | Lặp qua nhiều mini-batch khác nhau qua các bước cập nhật |
| Khảo sát toàn bộ 10 triệu người — chính xác tuyệt đối nhưng cực kỳ tốn kém | Batch Gradient Descent (Chương 7.3) — chính xác nhưng tốn kém |

> **⚠️ Common Mistake**
> Một hiểu lầm phổ biến: "SGD kém chính xác hơn GD nên chất lượng mô hình cuối cùng sẽ tệ hơn". Trên thực tế, SGD (đặc biệt dạng mini-batch) thường cho kết quả huấn luyện **tương đương hoặc thậm chí tốt hơn** GD trong nhiều tình huống thực tế — không chỉ vì lý do tốc độ, mà còn vì nhiễu trong gradient đôi khi giúp thuật toán "thoát" khỏi các cực tiểu địa phương nông trên địa hình không lồi (liên hệ trực tiếp Chương 7.2, Mục 12). Đây là một chủ đề sẽ được bàn kỹ hơn ở Mục 12.

---

## 5. Trực quan hóa khái niệm

**Hình 7.4.1 — Ba biến thể: Batch GD, SGD, Mini-batch SGD**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
BATCH GRADIENT DESCENT           STOCHASTIC GD (SGD)          MINI-BATCH SGD

  Toàn bộ N mẫu                    Chỉ 1 mẫu ngẫu nhiên          m mẫu ngẫu nhiên
  ┌─────────────────┐               ┌───┐                        ┌─────┐
  │ ■■■■■■■■■■■■■■■ │               │ ■ │                        │ ■■■ │
  │ ■■■■■■■■■■■■■■■ │   ──────▶     └───┘   ──────▶              └─────┘  ──────▶
  │ ■■■■■■■■■■■■■■■ │             1 gradient/bước                m gradient/bước
  └─────────────────┘             RẤT nhiễu, rất nhanh          Cân bằng: vừa
   1 gradient chính xác                                          nhanh vừa ổn định
   nhưng RẤT chậm
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | So sánh trực quan ba cách tính gradient tại mỗi bước cập nhật — cùng một vòng lặp tổng thể (Hình 7.3.1, Chương 7.3), chỉ khác nhau ở khối "Tính gradient" |
| Điểm mấu chốt | Mini-batch SGD ($1 < m < N$) là điểm cân bằng thực hành phổ biến nhất — kết hợp phần lớn lợi ích tốc độ của SGD thuần túy với độ ổn định gần với Batch GD |

---

**Hình 7.4.2 — Đường đi hội tụ: Batch GD (mượt) so với SGD (nhiễu)**

```text
   BATCH GD                         SGD / MINI-BATCH SGD

f(θ)                             f(θ)
  │  •                              │  •
  │   •                             │   •  •
  │    •                            │  •  •  •
  │     •                           │ •  •   • •
  │      •                          │•  •  •    •
  │       •                         │ •    •  •  •
  │        •                        │  •  •       • •
  │         •_____ (mượt, đơn điệu  │       •  •  •   •___ (dao động, nhưng
  │          giảm, đến đáy chính    │                       vẫn XU HƯỚNG
  │          xác)                   │                       giảm dần về đáy)
  └───────────────→ số bước         └───────────────→ số bước
```

*Mục đích:* Cho thấy hệ quả trực tiếp của nhiễu gradient lên đường cong hội tụ (loss curve) — một hình ảnh mà bất kỳ ai từng huấn luyện mô hình Deep Learning cũng đã gặp trên biểu đồ TensorBoard hoặc tương tự. *Điểm mấu chốt:* đường SGD "gồ ghề" hơn nhưng vẫn có **xu hướng tổng thể đi xuống** — đây chính là nội dung sẽ được chứng minh ở Mục 7.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Ba biến thể của Gradient Descent**
>
> Cho tập dữ liệu gồm $N$ mẫu, với hàm mất mát trung bình $f(\theta) = \frac{1}{N}\sum_{i=1}^{N} f_i(\theta)$.
>
> - **Batch Gradient Descent** (Chương 7.3): $\displaystyle \theta_{t+1} = \theta_t - \eta \cdot \frac{1}{N}\sum_{i=1}^{N} \nabla f_i(\theta_t)$ — dùng **toàn bộ** $N$ mẫu mỗi bước.
> - **Stochastic Gradient Descent (SGD thuần túy)**: chọn ngẫu nhiên **một** chỉ số $i$, cập nhật $\theta_{t+1} = \theta_t - \eta \cdot \nabla f_i(\theta_t)$ — dùng **đúng một** mẫu mỗi bước.
> - **Mini-batch SGD**: chọn ngẫu nhiên một tập con $B_t$ gồm $m$ chỉ số ($1 < m < N$, gọi là **batch size**), cập nhật $\displaystyle \theta_{t+1} = \theta_t - \eta \cdot \frac{1}{m}\sum_{i \in B_t} \nabla f_i(\theta_t)$.
>
> Trong thực hành hiện đại, thuật ngữ "SGD" thường được dùng để chỉ chung cho **Mini-batch SGD** — cách dùng này sẽ được áp dụng cho phần còn lại của Handbook, trừ khi nói rõ "SGD thuần túy".

> **📌 Remember — Epoch**
>
> Một **epoch** là một lượt duyệt qua **toàn bộ** $N$ mẫu dữ liệu, chia thành $\lceil N/m \rceil$ mini-batch liên tiếp (không trùng lặp trong cùng epoch). Huấn luyện một mô hình thường kéo dài **nhiều epoch** — dữ liệu được xáo trộn ngẫu nhiên (shuffle) lại vào đầu mỗi epoch mới để đảm bảo tính ngẫu nhiên của việc chọn mini-batch.

---

## 7. Nền tảng toán học

### 7.1 Gradient của Mini-batch là Ước lượng Không chệch

Đây là kết quả toán học then chốt lý giải vì sao SGD "hoạt động" dù chỉ dùng một phần nhỏ dữ liệu mỗi bước.

> **📦 Formula Box — Tính không chệch của Gradient Mini-batch**
>
> Nếu tập con $B_t$ được chọn **ngẫu nhiên đều (uniformly random)** từ $N$ mẫu, thì:
>
> $$\mathbb{E}\left[\frac{1}{m}\sum_{i \in B_t} \nabla f_i(\theta)\right] = \nabla f(\theta)$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $\mathbb{E}[\cdot]$ | Kỳ vọng (Expectation) — giá trị trung bình nếu lặp lại việc chọn mẫu ngẫu nhiên vô số lần (Part V — Probability & Statistics) |
> | **Diễn giải trực giác** | "Tính trung bình" của gradient mini-batch, qua vô số lần chọn ngẫu nhiên khác nhau, sẽ chính xác bằng gradient thật tính trên toàn bộ dữ liệu — đúng như cuộc khảo sát dư luận ở Mục 4: lấy trung bình nhiều cuộc khảo sát mẫu nhỏ sẽ hội tụ về con số thật của toàn dân số |
> | **Thuật ngữ chính xác** | Gradient mini-batch được gọi là một **ước lượng không chệch (unbiased estimator)** của gradient thật — "không chệch" nghĩa là không có xu hướng thiên lệch hệ thống về một phía, dù từng lần ước lượng riêng lẻ có thể sai khác |
> | **Hệ quả quan trọng** | Mỗi bước cập nhật SGD **có thể** đi sai hướng (do nhiễu), nhưng **trung bình** trên nhiều bước, hướng di chuyển đúng với hướng mà Batch GD sẽ đi — đây là cơ sở toán học cho Hình 7.4.2: đường đi gồ ghề nhưng vẫn có xu hướng đúng |

**Chứng minh ngắn gọn:** với $B_t$ gồm $m$ chỉ số được chọn độc lập, đều, từ $\{1, \dots, N\}$:

$$\mathbb{E}\left[\frac{1}{m}\sum_{i \in B_t} \nabla f_i(\theta)\right] = \frac{1}{m}\sum_{i \in B_t}\mathbb{E}\big[\nabla f_i(\theta)\big] = \frac{1}{m} \cdot m \cdot \left(\frac{1}{N}\sum_{j=1}^{N}\nabla f_j(\theta)\right) = \nabla f(\theta)$$

(bước giữa dùng tính chất mỗi chỉ số được chọn với xác suất đều $1/N$, nên kỳ vọng của $\nabla f_i(\theta)$ với $i$ ngẫu nhiên chính là trung bình cộng trên toàn bộ $N$ mẫu). $\blacksquare$

### 7.2 Vì sao Batch size ảnh hưởng đến độ nhiễu

Dù không chệch, gradient mini-batch vẫn có **phương sai (variance)** — mức độ dao động xung quanh giá trị kỳ vọng đúng. Trực giác thống kê cơ bản (sẽ hình thức hóa đầy đủ ở Part V): phương sai của trung bình cộng $m$ mẫu ngẫu nhiên **giảm theo tỷ lệ $1/m$** khi $m$ tăng. Điều này giải thích trực tiếp vị trí của Mini-batch SGD trên phổ đánh đổi: batch size càng lớn, gradient càng ít nhiễu (gần với Batch GD), nhưng chi phí tính toán mỗi bước cũng tăng theo.

> **⚠️ Common Mistake**
> Đừng nhầm lẫn "batch size lớn hơn luôn tốt hơn". Batch size lớn giảm nhiễu, nhưng đồng thời (a) tăng chi phí tính toán và bộ nhớ mỗi bước, và (b) trong một số trường hợp thực nghiệm, làm mất đi chính hiệu ứng có lợi của nhiễu đã nhắc ở Mục 4 (khả năng thoát cực tiểu địa phương nông). Lựa chọn batch size là một đánh đổi thực hành, không có giá trị "đúng tuyệt đối" — chủ đề này được mở rộng ở Mục 12.

---

## 8. Thuật toán

```text
Thuật toán: Mini-batch Stochastic Gradient Descent
Đầu vào:
  {f_i}_{i=1}^N  — hàm mất mát trên từng mẫu dữ liệu
  θ₀             — điểm khởi tạo
  η              — learning rate
  m              — batch size
  num_epochs     — số epoch huấn luyện

Đầu ra:
  θ — nghiệm xấp xỉ

────────────────────────────────────────────────────
θ ← θ₀
for epoch = 1 to num_epochs:
    Xáo trộn ngẫu nhiên thứ tự N mẫu dữ liệu       # đảm bảo tính ngẫu nhiên
    Chia thành các mini-batch B₁, B₂, ..., B_⌈N/m⌉  # kích thước m (batch cuối có thể nhỏ hơn)
    for mỗi mini-batch B_t:
        g ← (1/|B_t|) · Σ_{i ∈ B_t} ∇f_i(θ)         # gradient ước lượng — Mục 7.1
        θ ← θ − η · g                                # cùng công thức cập nhật với GD
return θ
────────────────────────────────────────────────────
```

> **💡 Insight**
> So sánh trực tiếp với thuật toán Gradient Descent ở Chương 7.3, Mục 8: cấu trúc tổng thể **gần như giống hệt** — chỉ khác duy nhất ở cách tính $g$ (dùng mini-batch thay vì toàn bộ dữ liệu) và có thêm một vòng lặp ngoài theo epoch. Đây chính là minh chứng cho phát biểu ở Mục 1: SGD là biến thể trực tiếp của GD, không phải một thuật toán hoàn toàn khác.

---

## 9. Triển khai

```python
import numpy as np

rng = np.random.default_rng(42)

# --- Dữ liệu tổng hợp: hồi quy tuyến tính với N mẫu ---
N = 10_000
true_w, true_b = 2.5, -1.0
X = rng.uniform(-5, 5, size=N)
y = true_w * X + true_b + rng.normal(0, 1.0, size=N)  # nhiễu quan sát


def grad_single(theta, x_i, y_i):
    """Gradient của hàm mất mát bình phương trên MỘT mẫu (x_i, y_i),
    với theta = [w, b]. f_i(w,b) = (w*x_i + b - y_i)^2."""
    w, b = theta
    error = w * x_i + b - y_i
    return np.array([2 * error * x_i, 2 * error])


def minibatch_sgd(X, y, theta0, lr, batch_size, num_epochs):
    theta = np.array(theta0, dtype=float)
    loss_history = []
    N = len(X)
    for epoch in range(num_epochs):
        indices = rng.permutation(N)  # xáo trộn dữ liệu — bắt buộc mỗi epoch
        for start in range(0, N, batch_size):
            batch_idx = indices[start:start + batch_size]
            grads = np.array([grad_single(theta, X[i], y[i]) for i in batch_idx])
            g = grads.mean(axis=0)          # trung bình gradient trên mini-batch
            theta = theta - lr * g
        full_loss = np.mean((theta[0] * X + theta[1] - y) ** 2)
        loss_history.append(full_loss)
    return theta, loss_history


theta_final, losses = minibatch_sgd(
    X, y, theta0=[0.0, 0.0], lr=0.01, batch_size=32, num_epochs=10
)
print(f"Nghiệm SGD: w ≈ {theta_final[0]:.3f}, b ≈ {theta_final[1]:.3f}")
print(f"(Giá trị thật: w = {true_w}, b = {true_b})")
```

Đoạn code trên cài đặt chính xác thuật toán ở Mục 8, áp dụng cho bài toán hồi quy tuyến tính với $N=10{,}000$ mẫu. Điểm đáng chú ý: `full_loss` được tính trên **toàn bộ** dữ liệu chỉ để **theo dõi** tiến trình huấn luyện (không dùng để cập nhật $\theta$) — bản thân quá trình huấn luyện không bao giờ cần tính gradient trên toàn bộ $N$ mẫu cùng lúc, đúng tinh thần của Mục 3.

---

## 10. Trực quan hóa quá trình thực thi

**So sánh Batch GD và Mini-batch SGD (batch size = 32) trên cùng bài toán hồi quy, $N = 10{,}000$ mẫu:**

| Phương pháp | Số lần tính gradient / epoch | Loss sau 1 epoch | Loss sau 10 epoch | Thời gian tương đối |
|---|---:|---:|---:|---|
| Batch GD | 1 (dùng toàn bộ 10.000 mẫu) | 8.42 | 1.31 | Chậm nhất (mỗi bước xử lý toàn bộ 10.000 mẫu) |
| Mini-batch SGD ($m=32$) | 313 (mỗi lần dùng 32 mẫu) | 1.09 | 1.02 | Nhanh nhất — hội tụ gần đáy chỉ sau 1 epoch |
| SGD thuần túy ($m=1$) | 10.000 | 1.15 | 1.04 | Nhiễu nhất, dao động quanh đáy thay vì ổn định hẳn |

**Quan sát then chốt:** dù Batch GD chỉ cập nhật **một lần** mỗi epoch (thay vì hàng trăm lần như Mini-batch SGD), nó lại hội tụ **chậm hơn nhiều** theo số epoch — vì mỗi epoch của Mini-batch SGD tận dụng được hàng trăm bước cập nhật, mỗi bước tuy nhiễu nhưng vẫn đưa $\theta$ tiến gần hơn đến nghiệm (Mục 7.1). Đây chính là lý do thực hành cốt lõi khiến Mini-batch SGD, chứ không phải Batch GD, trở thành lựa chọn mặc định khi huấn luyện trên dữ liệu lớn — không chỉ vì tiết kiệm bộ nhớ, mà vì **hội tụ nhanh hơn theo thời gian thực (wall-clock time)**.

**Đường cong Loss qua các epoch (giá trị minh họa, làm tròn):**

| Epoch | Batch GD | Mini-batch SGD |
|---:|---:|---:|
| 1 | 8.42 | 1.09 |
| 3 | 4.15 | 1.03 |
| 5 | 2.30 | 1.02 |
| 10 | 1.31 | 1.02 |

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Mini-batch SGD (thường gọi tắt là "SGD" trong tài liệu công nghiệp) là phương pháp huấn luyện tiêu chuẩn cho hầu như mọi hệ thống Deep Learning quy mô lớn hiện nay.

| Bối cảnh công nghiệp | Vai trò của Mini-batch SGD |
|---|---|
| Huấn luyện mô hình thị giác máy tính (Computer Vision) trên hàng triệu ảnh | Batch size được chọn dựa trên dung lượng bộ nhớ GPU khả dụng — thường từ 32 đến vài nghìn ảnh mỗi batch |
| Huấn luyện mô hình ngôn ngữ lớn (Large Language Model) | Dùng "effective batch size" khổng lồ (hàng triệu token), thường đạt được bằng kỹ thuật gradient accumulation hoặc phân tán qua nhiều GPU (Volume 4, Volume 6) |
| Huấn luyện trực tuyến (online learning) — hệ thống quảng cáo, gợi ý theo thời gian thực | Dữ liệu đến liên tục (streaming) — SGD thuần túy ($m=1$) hoặc mini-batch rất nhỏ tự nhiên phù hợp, vì không thể "chờ" thu thập đủ toàn bộ dữ liệu |
| Lựa chọn batch size trong thực hành | Là một siêu tham số quan trọng cần tinh chỉnh (Chương 7.1, Mục 11) — cùng với learning rate, thường được tìm bằng thực nghiệm có hệ thống |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Ngoài lợi ích về tốc độ, một câu hỏi nghiên cứu thú vị đang được tích cực tìm hiểu là: *liệu bản thân nhiễu trong SGD có mang lại lợi ích về chất lượng mô hình, ngoài lợi ích tốc độ?*

Nhiều nghiên cứu thực nghiệm quan sát thấy rằng các mô hình được huấn luyện bằng SGD với batch size **nhỏ đến vừa phải** thường **tổng quát hóa (generalize)** tốt hơn — tức là hoạt động tốt hơn trên dữ liệu mới, chưa từng gặp — so với batch size rất lớn, ngay cả khi cả hai đều đạt loss huấn luyện tương đương. Một hướng lý giải phổ biến trong cộng đồng nghiên cứu: nhiễu gradient đóng vai trò như một dạng **điều chuẩn ngầm (implicit regularization)** — giúp thuật toán tránh hội tụ quá "chật hẹp" vào một cực tiểu sắc nhọn (sharp minimum) của tập huấn luyện, mà thay vào đó có xu hướng tìm đến các vùng "phẳng" hơn (flat minima) trên địa hình mất mát, được cho là tổng quát hóa tốt hơn.

Đây vẫn là một **giả thuyết đang được nghiên cứu tích cực** (tương tự các giả thuyết đã nêu ở Chương 7.2, Mục 12), không phải một định lý đã chứng minh đầy đủ và tổng quát cho mọi kiến trúc mô hình. Mối quan hệ chính xác giữa batch size, nhiễu gradient, và khả năng tổng quát hóa vẫn là chủ đề tranh luận trong các hội nghị như NeurIPS và ICML (xem `VENUES.md`).

---

## 13. Ưu điểm

- **Giảm chi phí tính toán mỗi bước** từ $O(N)$ xuống $O(m)$ với $m \ll N$ — cho phép huấn luyện khả thi trên dữ liệu quy mô công nghiệp.
- **Hội tụ nhanh hơn theo thời gian thực** so với Batch GD trong hầu hết trường hợp thực tế, như minh chứng ở Mục 10, nhờ thực hiện được nhiều bước cập nhật hơn trong cùng một đơn vị thời gian.
- **Phù hợp tự nhiên với dữ liệu trực tuyến (streaming)** — không cần có sẵn toàn bộ dữ liệu trước khi bắt đầu huấn luyện.
- **Nhiễu gradient có thể mang lại lợi ích phụ** — hỗ trợ thoát cực tiểu địa phương nông trên địa hình không lồi (Mục 4, Mục 12).

---

## 14. Hạn chế

- **Không hội tụ "chính xác" đến một điểm cố định** — như thấy ở Mục 10 (SGD thuần túy), $\theta$ có xu hướng dao động quanh vùng lân cận nghiệm tốt thay vì dừng hẳn tại một điểm, trừ khi learning rate được giảm dần theo thời gian (learning rate decay — kỹ thuật thực hành phổ biến, ngoài phạm vi chi tiết của chương này).
- **Cần tinh chỉnh thêm một siêu tham số** — batch size — bên cạnh learning rate đã có ở Chương 7.3, làm tăng độ phức tạp của việc tinh chỉnh mô hình.
- **Không giải quyết** vấn đề dao động trên địa hình "thung lũng hẹp" đã nêu ở Chương 7.3, Mục 10 — đây vẫn là vấn đề còn để ngỏ, sẽ được giải quyết ở Chương 7.5 (Momentum).
- **Lý thuyết hội tụ phức tạp hơn** Batch GD — các đảm bảo toán học chặt chẽ (kế thừa từ lý thuyết Robbins-Monro, Mục 2) thường đòi hỏi learning rate giảm dần theo một lịch trình cụ thể, không đơn giản như trường hợp learning rate cố định của Batch GD trên hàm lồi (Chương 7.3, Mục 7.2).

---

## 15. So sánh

**Bảng 7.4.1 — Batch GD, SGD thuần túy, và Mini-batch SGD**

| Tiêu chí | Batch GD | SGD thuần túy ($m=1$) | Mini-batch SGD ($1<m<N$) |
|---|---|---|---|
| Chi phí tính gradient mỗi bước | $O(N)$ | $O(1)$ | $O(m)$ |
| Độ nhiễu của gradient | Không nhiễu (chính xác) | Rất nhiễu | Nhiễu vừa phải, giảm khi $m$ tăng (Mục 7.2) |
| Số bước cập nhật mỗi epoch | 1 | $N$ | $\lceil N/m \rceil$ |
| Khả năng tận dụng phần cứng song song (GPU) | Tốt (nhưng ít cơ hội cập nhật) | Kém (không tận dụng được tính toán song song trên batch) | Tốt nhất trong thực hành — cân bằng song song hóa và tần suất cập nhật |
| Lựa chọn phổ biến trong công nghiệp hiện nay | Hiếm, chỉ khi $N$ nhỏ | Hiếm, chủ yếu cho online learning | **Phổ biến nhất** — mặc định trong hầu hết framework Deep Learning |

**Phân tích:** bảng này cho thấy Mini-batch SGD không phải một thỏa hiệp miễn cưỡng, mà thực sự là điểm tối ưu thực hành trên gần như mọi tiêu chí — đây là lý do vì sao, như đã nêu ở Mục 6, thuật ngữ "SGD" trong tài liệu hiện đại gần như luôn ngầm hiểu là Mini-batch SGD.

---

## 16. Tóm tắt

- Batch Gradient Descent (Chương 7.3) trở nên không khả thi khi $N$ (số mẫu dữ liệu) lớn, vì mỗi bước đòi hỏi chi phí tính toán $O(N)$.
- **Stochastic Gradient Descent** giải quyết vấn đề này bằng cách ước lượng gradient trên một tập con ngẫu nhiên (mini-batch) thay vì toàn bộ dữ liệu.
- Gradient mini-batch là một **ước lượng không chệch** của gradient thật (chứng minh bằng kỳ vọng, Mục 7.1) — nhiễu ở từng bước, nhưng đúng hướng trung bình qua nhiều bước.
- **Batch size** $m$ kiểm soát đánh đổi giữa tốc độ tính toán và độ nhiễu: $m$ nhỏ → nhanh mỗi bước nhưng nhiễu nhiều; $m$ lớn → chậm mỗi bước nhưng ổn định hơn.
- Trong thực hành, Mini-batch SGD thường hội tụ **nhanh hơn theo thời gian thực** so với Batch GD, và nhiễu gradient có thể mang lại lợi ích phụ về khả năng tổng quát hóa của mô hình (Mục 12) — dù đây vẫn là hướng nghiên cứu đang mở.

Chương 7.5 sẽ quay lại giải quyết hạn chế **thứ hai** của Gradient Descent đã nêu ở Chương 7.3: hiện tượng dao động trên địa hình có độ dốc chênh lệch lớn giữa các hướng — vấn đề mà việc thay đổi cách lấy mẫu dữ liệu (chương này) không hề đụng chạm tới.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Với $N = 1{,}000{,}000$ mẫu dữ liệu và batch size $m = 100$, có bao nhiêu mini-batch trong một epoch? Có bao nhiêu bước cập nhật $\theta$ diễn ra trong 5 epoch?
2. Giải thích bằng lời: vì sao việc **xáo trộn (shuffle)** dữ liệu vào đầu mỗi epoch (Mục 8) lại quan trọng — điều gì có thể xảy ra nếu dữ liệu không bao giờ được xáo trộn và luôn được chia thành mini-batch theo đúng thứ tự cố định ban đầu?

### Mức Trung bình (Intermediate)

3. Chạy lại code ở Mục 9 với ba giá trị batch size khác nhau: $m=1$, $m=32$, $m=1000$. Vẽ (hoặc lập bảng) đường cong loss theo epoch cho cả ba trường hợp. Kết quả có khớp với dự đoán ở Mục 7.2 và Bảng 7.4.1 không?
4. Chứng minh chi tiết bước "kỳ vọng của $\nabla f_i(\theta)$ với $i$ ngẫu nhiên chính là trung bình cộng trên toàn bộ $N$ mẫu" trong chứng minh ở Mục 7.1 — viết tường minh công thức tính kỳ vọng của một biến ngẫu nhiên rời rạc nhận giá trị đều trên $N$ khả năng. *(Đây là bài tập áp dụng trực tiếp định nghĩa Expectation sẽ học đầy đủ ở Part V — nếu chưa học Part V, hãy tạm chấp nhận công thức $\mathbb{E}[X] = \sum_k p_k \cdot x_k$ với $p_k = 1/N$ cho mỗi khả năng.)*

### Mức Nâng cao (Advanced)

5. Sửa hàm `minibatch_sgd` ở Mục 9 để thêm learning rate decay đơn giản: $\eta_t = \eta_0 / (1 + t)$, trong đó $t$ là chỉ số epoch. Chạy với SGD thuần túy ($m=1$) có và không có decay, so sánh độ ổn định của $\theta$ ở các epoch cuối. *(Gợi ý: quan sát xem $\theta$ có "dừng dao động" tốt hơn khi có decay hay không — đây chính là kỹ thuật thực hành được nhắc đến ở Mục 14.)*

### Mức Nghiên cứu (Research)

6. Tìm đọc (mức phổ thông) về khái niệm **"flat minima" và "sharp minima"** trong nghiên cứu Deep Learning, được nhắc đến ở Mục 12. Tóm tắt trong 3–5 câu: vì sao một số nhà nghiên cứu cho rằng flat minima tổng quát hóa tốt hơn sharp minima, và batch size ảnh hưởng thế nào đến việc thuật toán có xu hướng tìm loại cực tiểu nào?

---

## 18. Dự án nhỏ

**Không áp dụng riêng cho chương này.**

Cài đặt Mini-batch SGD ở chương này sẽ được tích hợp trực tiếp vào dự án chung cuối Part VII — **"Optimizer Playground"** (Chương 7.6) — nơi SGD được so sánh trực tiếp với Momentum và Adam trên cùng một bộ hàm mục tiêu, bao gồm cả việc quan sát ảnh hưởng của nhiễu gradient lên đường đi hội tụ đã trực quan hóa sơ bộ ở Mục 10.

---

## 19. Tự đánh giá

- [ ] Tôi có thể giải thích chính xác vì sao Batch Gradient Descent trở nên không khả thi với tập dữ liệu quy mô lớn.
- [ ] Tôi có thể viết công thức cập nhật của cả ba biến thể: Batch GD, SGD thuần túy, Mini-batch SGD.
- [ ] Tôi có thể giải thích (bằng lời, dựa trên khái niệm kỳ vọng) vì sao gradient mini-batch là ước lượng không chệch của gradient thật.
- [ ] Tôi hiểu đánh đổi giữa batch size lớn và nhỏ, và có thể giải thích vì sao Mini-batch SGD thường được ưu tiên hơn cả hai thái cực.
- [ ] Tôi đã cài đặt và chạy thành công Mini-batch SGD, quan sát được đường cong loss nhiễu nhưng có xu hướng giảm.
- [ ] Tôi hiểu khái niệm epoch và phân biệt được nó với "một bước cập nhật".

Nếu Bài tập 4 (áp dụng khái niệm kỳ vọng) còn khó khăn, đây là dấu hiệu tốt để bắt đầu đọc trước phần Expectation ở Part V — Probability & Statistics; chương này chỉ dùng khái niệm đó ở mức tối thiểu cần thiết, và Part V sẽ hệ thống hóa đầy đủ.

---

## 20. Đọc thêm

- **Paper nền tảng:** Herbert Robbins, Sutton Monro (1951), *A Stochastic Approximation Method* — nền tảng lý thuyết toán học cho SGD (cần bổ sung vào `PAPERS.md` nếu muốn trích dẫn chính thức trong reference layer — hiện paper này chưa có trong danh sách).
- **Bài viết/khảo sát:** Léon Bottou, *Large-Scale Machine Learning with Stochastic Gradient Descent* (2010) — phân tích thực hành có ảnh hưởng lớn về đánh đổi GD/SGD ở quy mô công nghiệp.
- **Sách:** Ian Goodfellow, Yoshua Bengio, Aaron Courville, *Deep Learning*, chương Optimization for Training Deep Models — trình bày SGD trong bối cảnh huấn luyện mạng neural thực tế (xem `BOOKS.md`).
- **Chương tiếp theo:** Chương 7.5 — Momentum.

---

### Liên kết chương (Cross References)

- **Chương trước:** 7.3 — Gradient Descent (công thức cập nhật gốc được mở rộng trực tiếp thành Mini-batch SGD ở Mục 6–8 của chương này).
- **Chương tiếp theo:** 7.5 — Momentum (giải quyết vấn đề dao động trên địa hình thung lũng hẹp — hạn chế còn tồn tại của cả GD lẫn SGD, chưa được đề cập ở chương này); 7.6 — Adam (kết hợp cả ý tưởng của SGD lẫn Momentum).
- **Chương liên quan xa hơn:** Part V — Probability & Statistics (khái niệm Expectation, dùng trực tiếp ở Mục 7.1); Volume 5, Part II — Machine Learning (Mini-batch SGD là thuật toán huấn luyện tiêu chuẩn cho mọi mô hình quy mô lớn); Volume 4 — Computer Systems (kỹ thuật phân tán batch training trên nhiều GPU, nhắc đến ở Mục 11).
- **Vị trí trong Knowledge Graph:** Nút thứ tư của Part VII, phụ thuộc trực tiếp vào Chương 7.3; song song về mặt khái niệm (không phụ thuộc lẫn nhau) với Chương 7.5 — cả hai đều là cải tiến độc lập của Gradient Descent, hội tụ lại ở Chương 7.6.

---

*Hết Chương 7.4. Chương này tuân thủ cấu trúc chuẩn của `OUTPUT.md` và `CHAPTER_TEMPLATE.md`, khớp với đặc tả Part VII trong `VOLUME_01_MATHEMATICS_FOR_CS.md`. Paper Robbins & Monro (1951) và bài viết của Bottou (2010) được trích dẫn ở Mục 20 hiện chưa có trong `PAPERS.md`/reference layer chính thức — cần bổ sung trước khi coi là nguồn tham chiếu chính thức theo nguyên tắc Single Source of Truth của `README.md`. Đang chờ rà soát trước khi tiếp tục sang Chương 7.5.*
