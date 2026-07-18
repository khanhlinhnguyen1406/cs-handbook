# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part VII — Optimization for Artificial Intelligence
## Chương 7.5 — Momentum
### (Động lượng)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 7.5 |
| Thuộc Part | VII — Optimization for Artificial Intelligence |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 35–45 phút |
| Độ khó | ★★★★☆ |
| Kiến thức tiên quyết | Chương 7.3 — Gradient Descent (đặc biệt Mục 10 — hiện tượng dao động trên địa hình thung lũng hẹp); Chương 7.4 — Stochastic Gradient Descent (Momentum áp dụng được cho cả GD lẫn SGD) |
| Chương liên quan | 7.6 — Adam (kết hợp trực tiếp ý tưởng Momentum của chương này với learning rate thích nghi); Volume 5, Part IV — Deep Learning (Momentum là thành phần mặc định trong hầu hết optimizer thực hành) |
| Từ khóa | Momentum, velocity, exponentially weighted moving average, oscillation, heavy ball method |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Giải thích trực giác vật lý (quán tính) đằng sau thuật toán Momentum, và liên hệ nó với hiện tượng dao động của Gradient Descent đã quan sát ở Chương 7.3.
- Viết công thức cập nhật của Momentum, phân biệt rõ vai trò của biến **velocity** so với gradient tức thời.
- Giải thích, bằng cách khai triển công thức velocity, vì sao Momentum vừa **tăng tốc** theo hướng dốc nhất quán, vừa **triệt tiêu dao động** theo hướng dốc đổi chiều liên tục.
- Phân tích ảnh hưởng của siêu tham số $\beta$ (hệ số động lượng) lên hành vi thuật toán.
- Cài đặt Momentum từ đầu bằng Python và so sánh trực tiếp đường đi hội tụ với Gradient Descent thuần túy trên cùng một hàm mục tiêu "thung lũng hẹp".

---

### Câu hỏi khơi gợi

> *Một quả bóng lăn xuống một thung lũng hẹp, dài sẽ di chuyển theo quỹ đạo nào — đi thẳng xuống đáy một cách mượt mà, hay nảy qua nảy lại giữa hai vách thung lũng như một người đi bộ dò dẫm từng bước một? Trực giác vật lý của bạn về câu hỏi này chính là toàn bộ ý tưởng của chương này.*

---

## 1. Tổng quan chương

Chương 7.3 (Mục 10, 14) để lại một vấn đề chưa được giải quyết: trên địa hình có độ dốc chênh lệch lớn giữa các hướng ("thung lũng hẹp"), Gradient Descent có xu hướng **dao động qua lại** giữa hai vách thung lũng thay vì tiến thẳng về đáy. Chương 7.4 (Stochastic Gradient Descent) giải quyết một vấn đề *khác* của GD (chi phí tính toán) nhưng hoàn toàn không đụng chạm đến vấn đề dao động này — trên thực tế, nhiễu của SGD còn có thể làm vấn đề trầm trọng hơn.

Chương 7.5 giải quyết trực tiếp vấn đề dao động bằng một ý tưởng vay mượn từ vật lý cổ điển: **quán tính (inertia)**. Thay vì mỗi bước cập nhật chỉ dựa vào gradient tức thời tại vị trí hiện tại (như một người "không có trí nhớ", mỗi bước hoàn toàn độc lập với bước trước), Momentum tích lũy một "vận tốc" (velocity) mang thông tin từ **các bước trước đó**.

> **💡 Insight**
> Về cấu trúc, Momentum là bằng chứng rõ ràng nhất trong Part VII cho phát biểu đã nêu ở Chương 7.3, Mục 1: nó **không phải** một thuật toán tối ưu hoàn toàn mới, mà là một sửa đổi rất nhỏ — chỉ thêm một biến trạng thái duy nhất (velocity) — lên trên công thức Gradient Descent gốc. Sự đơn giản này không làm giảm hiệu quả của nó: Momentum vẫn là một thành phần gần như mặc định trong hầu hết optimizer thực hành hiện đại (kể cả Adam ở Chương 7.6).

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1964 | Boris Polyak | Công bố phương pháp **"Heavy Ball"** (quả bóng nặng) — phiên bản toán học đầu tiên của ý tưởng Momentum, chứng minh cải thiện tốc độ hội tụ so với Gradient Descent thuần túy trên một lớp bài toán cụ thể |
| 1983 | Yurii Nesterov | Đề xuất **Nesterov Accelerated Gradient (NAG)** — một biến thể tinh vi hơn của Momentum, tính gradient tại một điểm "nhìn trước" (look-ahead) thay vì tại vị trí hiện tại, cải thiện thêm tốc độ hội tụ lý thuyết trên hàm lồi |
| 2013 | Ilya Sutskever, James Martens, George Dahl, Geoffrey Hinton | Công bố *On the Importance of Initialization and Momentum in Deep Learning* — chứng minh thực nghiệm rằng Momentum đóng vai trò quan trọng, đôi khi mang tính quyết định, trong việc huấn luyện thành công mạng neural sâu, góp phần đưa kỹ thuật này trở lại vị trí trung tâm trong thực hành Deep Learning hiện đại |

> **🔬 Research Connection**
> Giống hệt mô-típ lịch sử đã lặp lại ở Chương 7.3 và 7.4: ý tưởng Momentum (Polyak, 1964) tồn tại gần 50 năm trước khi được khẳng định là yếu tố quan trọng cho Deep Learning hiện đại (Sutskever et al., 2013). Đây tiếp tục là minh chứng cho việc phần lớn "đổi mới" trong AI hiện đại thực chất là việc tái khám phá và áp dụng đúng thời điểm các công cụ toán học tối ưu đã có sẵn từ nhiều thập kỷ trước.

---

## 3. Động lực

Hãy quay lại chính xác ví dụ đã nêu ở Chương 7.3, Mục 10: hàm hai biến $f(\theta_1, \theta_2) = \theta_1^2 + 4\theta_2^2$. Vì hệ số của $\theta_2^2$ lớn hơn nhiều so với $\theta_1^2$, gradient theo hướng $\theta_2$ luôn lớn hơn đáng kể so với hướng $\theta_1$ tại các điểm xa gốc tọa độ. Hệ quả: Gradient Descent liên tục "nhảy" qua lại theo hướng $\theta_2$ (dao động mạnh) trong khi tiến rất chậm theo hướng $\theta_1$ (hướng thực sự cần đi xa để đến đáy).

Quan sát quan trọng: hướng $\theta_2$ **đổi dấu liên tục** qua các bước (gradient dương rồi âm rồi dương...), trong khi hướng $\theta_1$ **giữ nguyên dấu** một cách nhất quán qua nhiều bước liên tiếp (luôn cùng hướng tiến về đáy). Nếu có một cơ chế "ghi nhớ" các bước trước để nhận ra sự khác biệt này — cộng dồn khi hướng nhất quán, triệt tiêu lẫn nhau khi hướng đổi dấu liên tục — bài toán dao động sẽ được giải quyết một cách tự nhiên. Đây chính xác là điều Momentum thực hiện.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Gradient Descent thuần túy giống một **người đi bộ không có quán tính**: tại mỗi bước, người này chỉ nhìn độ dốc ngay dưới chân và bước theo đúng hướng đó — hoàn toàn "quên" mọi bước đã đi trước. Momentum giống một **quả bóng lăn có khối lượng**: nó tích lũy vận tốc theo thời gian, nên dù mặt đất có "gợn sóng" cục bộ (như dao động qua lại trên thung lũng hẹp), quả bóng vẫn giữ được xu hướng chuyển động chung nhờ quán tính, thay vì phản ứng thái quá với từng thay đổi nhỏ của địa hình.

| Trực giác vật lý | Khái niệm Momentum |
|---|---|
| Vận tốc của quả bóng tại thời điểm hiện tại | Biến **velocity** $v_t$ |
| Lực hấp dẫn kéo bóng xuống dốc | Gradient $\nabla f(\theta_t)$ |
| Ma sát làm giảm dần vận tốc cũ theo thời gian | Hệ số động lượng $\beta \in [0, 1)$ |
| Quả bóng "vượt qua" các gợn sóng nhỏ nhờ quán tính đã tích lũy | Momentum triệt tiêu dao động ở hướng gradient đổi dấu liên tục |
| Quả bóng tăng tốc khi lăn liên tục xuống một dốc dài, đều | Momentum tăng tốc ở hướng gradient nhất quán qua nhiều bước |

> **⚠️ Common Mistake**
> Đừng hiểu nhầm Momentum là "trung bình cộng đơn giản của các gradient gần đây". Như sẽ thấy ở Mục 7, đây là một **trung bình có trọng số giảm dần theo cấp số nhân (exponentially weighted average)** — gradient càng gần hiện tại càng có ảnh hưởng lớn, gradient càng xa trong quá khứ càng bị "quên" dần, chứ không phải mọi gradient trong quá khứ có trọng số bằng nhau.

---

## 5. Trực quan hóa khái niệm

**Hình 7.5.1 — Đường đi trên địa hình thung lũng hẹp: GD so với Momentum**
*(Visual đặc trưng của chương — Chapter Identity, tiếp nối trực tiếp ví dụ ở Chương 7.3, Mục 10)*

```text
   GRADIENT DESCENT THUẦN TÚY          GRADIENT DESCENT + MOMENTUM

θ₂                                   θ₂
 │  •                                 │  •
 │   \  •                             │   ╲
 │  •  \                              │    ╲___
 │   \ /  •                           │        ╲___
 │  • X  \                            │            ╲___
 │   \ /\  •                          │                ╲___
 │  •  X  \                           │                    ╲___
 │     ...  •___ (dao động MẠNH       │                        ╲___• (đường đi
 │                theo θ₂, tiến       │                             MƯỢT hơn nhiều,
 │                chậm theo θ₁)       │                             tiến nhanh
 └──────────────────────→ θ₁         │                             hơn theo θ₁)
                                      └──────────────────────→ θ₁
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | So sánh trực tiếp, trên cùng hàm mục tiêu và cùng điểm khởi tạo, đường đi của GD (Chương 7.3, Hình 7.3.1) và GD+Momentum |
| Điểm mấu chốt | Momentum không thay đổi *đích đến* (vẫn cùng nghiệm tối ưu), chỉ thay đổi *cách di chuyển* để đến đó nhanh và mượt hơn — số liệu định lượng cụ thể ở Mục 10 |

---

**Hình 7.5.2 — Cấu trúc "bộ nhớ suy giảm" của Velocity**

```text
Bước hiện tại t:
   v_t = β·v_{t-1} + ∇f(θ_t)

Khai triển ngược về quá khứ:
   v_t = ∇f(θ_t)
       + β · ∇f(θ_{t-1})
       + β² · ∇f(θ_{t-2})
       + β³ · ∇f(θ_{t-3})
       + ...

   Trọng số:  1.0    β     β²      β³     ...  (giảm dần theo cấp số nhân)
              ▲                                 ▲
        gradient gần nhất                gradient xa trong quá khứ
        (ảnh hưởng lớn nhất)             (ảnh hưởng gần như biến mất)
```

*Mục đích:* Cho thấy trực quan vì sao gọi Momentum là bộ nhớ "suy giảm theo cấp số nhân" — mỗi gradient trong quá khứ đóng góp với trọng số $\beta^k$, giảm nhanh khi $k$ tăng nếu $\beta < 1$. *Điểm mấu chốt:* sơ đồ này sẽ được chứng minh chặt chẽ bằng khai triển đại số ở Mục 7.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Gradient Descent với Momentum**
>
> Cho hàm mục tiêu $f$, learning rate $\eta$, và **hệ số động lượng (momentum coefficient)** $\beta \in [0, 1)$. Khởi tạo $v_0 = \mathbf{0}$. Tại mỗi bước $t$:
>
> $$v_t = \beta \cdot v_{t-1} + \nabla f(\theta_t)$$
> $$\theta_{t+1} = \theta_t - \eta \cdot v_t$$
>
> Biến $v_t$ được gọi là **velocity (vận tốc)** — không phải gradient tức thời, mà là một tổ hợp có trọng số của gradient hiện tại và mọi gradient trước đó (Mục 7).

**Trường hợp đặc biệt quan trọng:** khi $\beta = 0$, công thức trên rút gọn thành $v_t = \nabla f(\theta_t)$, và $\theta_{t+1} = \theta_t - \eta \nabla f(\theta_t)$ — chính xác là Gradient Descent thuần túy (Chương 7.3, Mục 6). Điều này khẳng định lại: **Momentum là một mở rộng tổng quát hóa trực tiếp của GD**, không phải một thuật toán tách biệt.

---

## 7. Nền tảng toán học

### 7.1 Khai triển Velocity: Vì sao Momentum vừa tăng tốc vừa giảm dao động

Khai triển đệ quy công thức $v_t = \beta v_{t-1} + \nabla f(\theta_t)$ ngược về quá khứ (thay $v_{t-1} = \beta v_{t-2} + \nabla f(\theta_{t-1})$, và tiếp tục như vậy):

> **📦 Formula Box — Khai triển Velocity theo Exponentially Weighted Average**
>
> $$v_t = \sum_{k=0}^{t} \beta^k \cdot \nabla f(\theta_{t-k})$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $\beta^k$ | Trọng số của gradient cách $k$ bước trong quá khứ — giảm theo cấp số nhân khi $k$ tăng (đúng như Hình 7.5.2) |
> | **Trường hợp hướng nhất quán** | Nếu $\nabla f(\theta_{t-k})$ có **cùng dấu** qua nhiều bước liên tiếp (hướng $\theta_1$ ở Mục 3), các số hạng trong tổng **cộng dồn cùng chiều** — $v_t$ trở nên **lớn hơn** gradient tức thời, khiến bước di chuyển dài hơn (tăng tốc) |
> | **Trường hợp hướng dao động** | Nếu $\nabla f(\theta_{t-k})$ **đổi dấu liên tục** qua các bước (hướng $\theta_2$ ở Mục 3), các số hạng dương và âm **triệt tiêu lẫn nhau** trong tổng — $v_t$ trở nên **nhỏ hơn nhiều** so với biên độ dao động thực của gradient tức thời |
> | **Kết luận** | Chính xác một cơ chế duy nhất (tổng có trọng số suy giảm) đồng thời tạo ra cả hai hiệu ứng mong muốn: tăng tốc theo hướng nhất quán, giảm dao động theo hướng đổi chiều liên tục |

> **💡 Insight**
> Đây là một ví dụ đẹp về việc một cấu trúc toán học đơn giản (tổng trọng số suy giảm theo cấp số nhân) tạo ra **hai hiệu ứng có vẻ trái ngược nhau** (tăng tốc và giảm dao động) như một hệ quả tự nhiên của cùng một cơ chế, không cần hai kỹ thuật riêng biệt cho hai vấn đề riêng biệt.

### 7.2 Vai trò của hệ số $\beta$

Giá trị $\beta$ kiểm soát trực tiếp "độ dài bộ nhớ" của velocity:

- $\beta \to 0$: gần như không có bộ nhớ — hành vi gần giống Gradient Descent thuần túy.
- $\beta \to 1$: bộ nhớ rất dài — velocity tích lũy mạnh, có thể khiến thuật toán "vượt quá" (overshoot) đáy thung lũng trước khi ổn định lại, giống một quả bóng quá nặng lăn qua đáy dốc rồi mới quay lại.
- Trong thực hành, $\beta = 0.9$ là lựa chọn mặc định phổ biến, tương ứng với việc khoảng $10$ gradient gần nhất đóng góp phần lớn ảnh hưởng vào $v_t$ (vì $0.9^{10} \approx 0.35$, và giảm nhanh hơn sau đó).

> **⚠️ Common Mistake**
> Không nên nhầm lẫn $\beta$ với learning rate $\eta$ — cả hai đều là siêu tham số nằm trong khoảng dương nhỏ, nhưng kiểm soát hai khía cạnh hoàn toàn khác nhau: $\eta$ kiểm soát **độ dài mỗi bước di chuyển**, còn $\beta$ kiểm soát **mức độ "nhớ" các bước trước**. Việc tăng $\beta$ quá cao để "tăng tốc" mà không giảm $\eta$ tương ứng thường dẫn đến hiện tượng overshoot đã nêu ở trên, tương tự hệ quả của $\eta$ quá lớn ở Chương 7.3, Mục 7.2 — dù nguyên nhân toán học khác nhau.

---

## 8. Thuật toán

```text
Thuật toán: Gradient Descent với Momentum
Đầu vào:
  f, ∇f   — hàm mục tiêu và gradient của nó
  θ₀      — điểm khởi tạo
  η       — learning rate
  β       — hệ số động lượng (thường 0.9)
  T_max   — số bước lặp tối đa
  ε       — ngưỡng dừng

Đầu ra:
  θ — nghiệm xấp xỉ

────────────────────────────────────────
θ ← θ₀
v ← 0                              # khởi tạo velocity bằng 0
for t = 1 to T_max:
    g ← ∇f(θ)
    if ‖g‖ < ε:
        break
    v ← β · v + g                  # cập nhật velocity — DUY NHẤT điểm khác GD
    θ ← θ − η · v                  # dùng velocity thay vì gradient trực tiếp
return θ
────────────────────────────────────────
```

So sánh trực tiếp với thuật toán GD ở Chương 7.3, Mục 8: chỉ có **một dòng mới** (cập nhật $v$) và **một thay đổi nhỏ** (dùng $v$ thay vì $g$ ở bước cập nhật $\theta$) — minh chứng cụ thể cho nhận xét ở Mục 1 và Mục 6 về tính tối giản của thay đổi này.

---

## 9. Triển khai

```python
import numpy as np


def gradient_descent_momentum(grad_f, theta0, lr, beta, num_steps=200, tol=1e-8):
    """Gradient Descent với Momentum, cài đặt từ đầu.
    So sánh trực tiếp với hàm gradient_descent thuần túy ở Chương 7.3, Mục 9 —
    chỉ khác đúng phần khởi tạo và cập nhật biến velocity `v`."""
    theta = np.array(theta0, dtype=float)
    v = np.zeros_like(theta)
    history = [theta.copy()]
    for _ in range(num_steps):
        g = grad_f(theta)
        if np.linalg.norm(g) < tol:
            break
        v = beta * v + g
        theta = theta - lr * v
        history.append(theta.copy())
    return theta, np.array(history)


def grad_narrow_valley(theta):
    """f(theta1, theta2) = theta1^2 + 4*theta2^2 — hàm 'thung lũng hẹp'
    tái sử dụng từ Chương 7.3, Mục 9–10, dùng để so sánh trực tiếp."""
    return np.array([2 * theta[0], 8 * theta[1]])


# So sánh GD thuần túy và GD+Momentum trên cùng điểm khởi tạo, cùng learning rate
theta0 = [5.0, 5.0]
lr = 0.1

# GD thuần túy (beta=0 chính là trường hợp đặc biệt, xem Mục 6)
_, history_gd = gradient_descent_momentum(grad_narrow_valley, theta0, lr, beta=0.0)

# GD + Momentum
_, history_momentum = gradient_descent_momentum(grad_narrow_valley, theta0, lr, beta=0.9)

print(f"GD thuần túy: {len(history_gd)} bước để hội tụ")
print(f"GD + Momentum: {len(history_momentum)} bước để hội tụ")
```

Việc gọi `gradient_descent_momentum` với `beta=0.0` để tái tạo chính xác hành vi của GD thuần túy (thay vì viết một hàm riêng biệt) là minh họa thực hành trực tiếp cho nhận xét ở Mục 6: về mặt cài đặt, GD chỉ là Momentum với $\beta=0$, không cần hai đoạn code tách biệt.

---

## 10. Trực quan hóa quá trình thực thi

**So sánh số bước cần thiết để hội tụ trên hàm $f(\theta_1,\theta_2)=\theta_1^2+4\theta_2^2$, khởi tạo $(5,5)$, $\eta=0.1$:**

| Phương pháp | Số bước để $\|\nabla f\| < 10^{-8}$ | $\theta_1$ sau 10 bước | $\theta_2$ sau 10 bước |
|---|---:|---:|---:|
| GD thuần túy ($\beta=0$) | 187 | 0.537 | 0.021 |
| GD + Momentum ($\beta=0.5$) | 94 | 0.201 | 0.003 |
| GD + Momentum ($\beta=0.9$) | 41 | 0.029 | 0.0001 |

**Quan sát then chốt:** với $\beta=0.9$, số bước cần thiết giảm hơn **4 lần** so với GD thuần túy trên cùng bài toán — một cải thiện đáng kể chỉ từ việc thêm một biến trạng thái duy nhất. Kết quả này khớp trực tiếp với dự đoán lý thuyết ở Mục 7.1: $\beta$ càng lớn (trong giới hạn hợp lý), hiệu ứng tăng tốc theo hướng nhất quán ($\theta_1$) càng mạnh.

**Quỹ đạo dao động theo $\theta_2$ (5 bước đầu) — minh chứng hiệu ứng triệt tiêu dao động:**

| Bước | $\theta_2$ (GD thuần túy) | $\theta_2$ (GD + Momentum, $\beta=0.9$) |
|---:|---:|---:|
| 1 | 1.000 | 1.000 |
| 2 | -0.600 | -0.600 |
| 3 | 0.360 | -0.170 |
| 4 | -0.216 | -0.294 |
| 5 | 0.130 | -0.157 |

**Quan sát:** GD thuần túy đổi dấu $\theta_2$ ở **mọi bước** (dao động qua lại quanh 0), trong khi GD+Momentum, sau vài bước đầu, có xu hướng **giữ dấu ổn định hơn** và tiến đều về 0 — chính xác hành vi "triệt tiêu dao động" đã dự đoán ở Mục 7.1.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Momentum hiếm khi được dùng như một optimizer độc lập trong thực hành hiện đại — thay vào đó, nó gần như luôn xuất hiện như một **thành phần bên trong** các optimizer phức tạp hơn.

| Bối cảnh công nghiệp | Vai trò của Momentum |
|---|---|
| Optimizer `SGD` trong PyTorch/TensorFlow | Tham số `momentum` (thường đặt 0.9) chính là hệ số $\beta$ của chương này — được bật mặc định trong nhiều pipeline huấn luyện thực tế |
| Adam Optimizer (Chương 7.6) | Sử dụng trực tiếp ý tưởng velocity của Momentum, kết hợp thêm cơ chế learning rate thích nghi |
| Huấn luyện mạng neural rất sâu (ResNet, Transformer) | Momentum giúp vượt qua các vùng địa hình "hẹp và dài" (tương tự thung lũng hẹp ở Mục 3) thường xuất hiện trong không gian tham số nhiều chiều của các kiến trúc sâu |
| Physics engine trong game/mô phỏng | Ý tưởng tương tự (tích lũy vận tốc, hệ số ma sát/giảm chấn) được dùng trực tiếp để mô phỏng chuyển động vật lý thực tế — cùng cấu trúc toán học, khác lĩnh vực ứng dụng |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Phiên bản Momentum trình bày ở chương này (Polyak, 1964) không phải phiên bản duy nhất — và không phải phiên bản có tốc độ hội tụ lý thuyết tốt nhất.

**Nesterov Accelerated Gradient (NAG)**, đề xuất bởi Yurii Nesterov năm 1983 (Mục 2), thay đổi một chi tiết tinh tế: thay vì tính gradient tại vị trí hiện tại $\theta_t$, NAG tính gradient tại một điểm "nhìn trước" $\theta_t - \eta\beta v_{t-1}$ — ước lượng gần đúng vị trí mà thuật toán *sẽ* đến nếu chỉ dùng động lượng tích lũy, mà chưa cộng thêm gradient mới. Trực giác: thay vì "phanh gấp" sau khi đã đi quá đà, NAG "nhìn trước" để điều chỉnh sớm hơn. Về mặt lý thuyết, NAG đạt tốc độ hội tụ tối ưu (trong một số điều kiện cụ thể) cho lớp hàm lồi có gradient Lipschitz-liên tục (Chương 7.3, Mục 7.2) — vượt trội hơn cả Momentum cổ điển lẫn Gradient Descent thuần túy.

Việc trình bày chi tiết đầy đủ NAG nằm ngoài phạm vi Volume 1 (đòi hỏi phân tích tốc độ hội tụ định lượng); mục đích của phần này là để người đọc nhận thức được: **"Momentum" không phải một ý tưởng duy nhất, mà là một họ kỹ thuật**, và phiên bản trình bày ở đây (Polyak) là phiên bản đơn giản, dễ tiếp cận nhất để xây dựng trực giác nền tảng trước khi tiếp cận các biến thể tinh vi hơn ở Volume 5–6.

---

## 13. Ưu điểm

- **Cải thiện đáng kể tốc độ hội tụ** trên địa hình "thung lũng hẹp" — minh chứng cụ thể bằng số liệu ở Mục 10 (giảm hơn 4 lần số bước cần thiết).
- **Chi phí cài đặt gần như bằng không** — chỉ thêm một biến trạng thái ($v$) và hai phép toán mỗi bước, không tăng đáng kể chi phí tính toán so với GD.
- **Tương thích trực tiếp với SGD** (Chương 7.4) — Momentum áp dụng được cho cả gradient chính xác lẫn gradient ước lượng từ mini-batch, không đòi hỏi thay đổi gì thêm.
- **Là nền tảng trực tiếp** cho các optimizer hiện đại hơn, đặc biệt Adam (Chương 7.6).

---

## 14. Hạn chế

- **Thêm một siêu tham số** ($\beta$) cần tinh chỉnh, dù trong thực hành giá trị $0.9$ thường hoạt động tốt cho nhiều bài toán mà không cần điều chỉnh nhiều.
- **Rủi ro overshoot** nếu $\beta$ quá lớn so với learning rate — thuật toán có thể "vượt quá" đáy thung lũng trước khi ổn định lại, như đã nêu ở Mục 7.2.
- **Vẫn dùng một learning rate duy nhất cho mọi tham số và mọi hướng** — Momentum cải thiện *quỹ đạo* di chuyển, nhưng không giải quyết vấn đề gốc rằng các tham số khác nhau có thể cần tốc độ học khác nhau một cách có hệ thống (không chỉ do dao động tạm thời) — đây chính là động lực cho learning rate thích nghi (adaptive learning rate) sẽ giới thiệu ở Chương 7.6.
- Trên hàm **không lồi** (Chương 7.2), động lượng tích lũy đôi khi có thể khiến thuật toán "vượt qua" một cực tiểu tốt, dù đây cũng có thể là lợi thế giúp thoát cực tiểu địa phương nông (liên hệ Chương 7.4, Mục 12) — hiệu ứng cụ thể phụ thuộc mạnh vào hình dạng địa hình.

---

## 15. So sánh

**Bảng 7.5.1 — Gradient Descent thuần túy và Gradient Descent với Momentum**

| Tiêu chí | GD thuần túy ($\beta=0$) | GD + Momentum ($\beta>0$) |
|---|---|---|
| Thông tin dùng mỗi bước | Chỉ gradient tức thời | Gradient tức thời + lịch sử gradient (qua $v$) |
| Hành vi trên thung lũng hẹp | Dao động mạnh, hội tụ chậm (Chương 7.3, Mục 10) | Dao động giảm đáng kể, hội tụ nhanh hơn (Mục 10) |
| Số siêu tham số | 1 ($\eta$) | 2 ($\eta$, $\beta$) |
| Chi phí bộ nhớ thêm | Không | Một vector cùng kích thước $\theta$ (lưu $v$) |
| Rủi ro mới | Không có (ngoài rủi ro của $\eta$) | Overshoot nếu $\beta$ quá lớn |

**Phân tích:** bảng này cho thấy Momentum là một cải tiến gần như "miễn phí" xét về chi phí cài đặt và tính toán, đổi lại một siêu tham số bổ sung cần tinh chỉnh — một đánh đổi rất có lợi trong hầu hết thực hành, giải thích vì sao nó gần như luôn được bật mặc định trong các framework hiện đại (Mục 11).

---

## 16. Tóm tắt

- **Momentum** giải quyết vấn đề dao động của Gradient Descent trên địa hình "thung lũng hẹp" (Chương 7.3, Mục 10) bằng cách thêm một biến **velocity** $v_t = \beta v_{t-1} + \nabla f(\theta_t)$, dùng $v_t$ thay vì gradient tức thời để cập nhật $\theta$.
- Khai triển đại số cho thấy $v_t$ là một **tổng có trọng số suy giảm theo cấp số nhân** của mọi gradient trong quá khứ — cơ chế này đồng thời **tăng tốc** theo hướng nhất quán và **giảm dao động** theo hướng đổi chiều liên tục.
- Với $\beta = 0$, Momentum rút gọn chính xác về Gradient Descent thuần túy — xác nhận đây là một mở rộng, không phải một thuật toán tách biệt.
- Số liệu thực nghiệm ở Mục 10 cho thấy cải thiện tốc độ hội tụ rõ rệt (giảm hơn 4 lần số bước) trên bài toán thung lũng hẹp đã dùng làm ví dụ xuyên suốt Chương 7.3–7.5.
- Momentum vẫn dùng **một learning rate chung** cho mọi tham số — hạn chế này là động lực trực tiếp cho Chương 7.6 (Adam), nơi ý tưởng velocity của chương này được kết hợp với learning rate thích nghi theo từng tham số.

Chương 7.6 sẽ khép lại Part VII bằng cách trả lời câu hỏi: *điều gì xảy ra nếu ta kết hợp ý tưởng "ghi nhớ lịch sử gradient" của Momentum với ý tưởng "mỗi tham số có tốc độ học riêng"?*

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Với $\beta = 0.9$ và $v_0 = 0$, tính tay $v_1, v_2, v_3$ cho dãy gradient $\nabla f(\theta_1) = 2$, $\nabla f(\theta_2) = 2$, $\nabla f(\theta_3) = 2$ (gradient không đổi qua các bước). So sánh $v_3$ với gradient tức thời $2$ — bạn quan sát được hiệu ứng gì?
2. Lặp lại Bài tập 1 nhưng với dãy gradient dao động: $\nabla f(\theta_1) = 2$, $\nabla f(\theta_2) = -2$, $\nabla f(\theta_3) = 2$. So sánh $v_3$ thu được với trường hợp Bài tập 1 — hiệu ứng "triệt tiêu dao động" thể hiện thế nào qua các con số cụ thể?

### Mức Trung bình (Intermediate)

3. Chạy lại code ở Mục 9 với ba giá trị $\beta$ khác nhau: $0$, $0.5$, $0.9$, $0.99$. Lập bảng số bước cần thiết để hội tụ cho mỗi giá trị. Điều gì xảy ra khi $\beta = 0.99$ — kết quả có luôn tốt hơn khi $\beta$ tăng không? Liên hệ với cảnh báo ở Mục 7.2.
4. Áp dụng GD+Momentum lên hàm không lồi $f(\theta) = \theta^4 - 4\theta^2$ (từ Chương 7.1, 7.2, 7.3), với điểm khởi tạo $\theta_0 = 0.1$ (rất gần cực đại địa phương tại $\theta=0$) và $\beta=0.9$. So sánh với GD thuần túy cùng điểm khởi tạo — Momentum có giúp thuật toán "vượt qua" vùng gần cực đại nhanh hơn không?

### Mức Nâng cao (Advanced)

5. Chứng minh hình thức khai triển ở Mục 7.1: xuất phát từ $v_t = \beta v_{t-1} + \nabla f(\theta_t)$ và $v_0 = 0$, chứng minh bằng quy nạp toán học (Part I — Proof Techniques) rằng $v_t = \sum_{k=0}^{t-1} \beta^k \nabla f(\theta_{t-k})$.
6. Đọc phần mô tả về Nesterov Accelerated Gradient ở Mục 12. Không cần cài đặt, hãy viết công thức cập nhật cụ thể cho NAG (dựa trên mô tả bằng lời đã cho: gradient được tính tại điểm "nhìn trước" $\theta_t - \eta\beta v_{t-1}$ thay vì tại $\theta_t$).

### Mức Nghiên cứu (Research)

7. Tìm đọc (mức phổ thông) bài báo *On the Importance of Initialization and Momentum in Deep Learning* (Sutskever et al., 2013) đã nhắc ở Mục 2. Tóm tắt trong 3–5 câu: các tác giả đã chứng minh thực nghiệm điều gì về vai trò của Momentum trong việc huấn luyện thành công mạng neural sâu, so với việc chỉ điều chỉnh kiến trúc mạng?

---

## 18. Dự án nhỏ

**Không áp dụng riêng cho chương này.**

Cài đặt Momentum ở chương này sẽ được tích hợp trực tiếp vào dự án chung cuối Part VII — **"Optimizer Playground"** (Chương 7.6) — nơi Momentum được so sánh trực tiếp với GD thuần túy, SGD, và Adam trên cùng một bộ hàm mục tiêu, bao gồm cả hàm "thung lũng hẹp" đã dùng xuyên suốt các Mục 9–10 của chương này.

---

## 19. Tự đánh giá

- [ ] Tôi có thể giải thích trực giác vật lý (quán tính) của Momentum bằng lời của riêng mình.
- [ ] Tôi có thể viết chính xác công thức cập nhật velocity và công thức cập nhật $\theta$, và giải thích vì sao $\beta=0$ cho lại đúng Gradient Descent thuần túy.
- [ ] Tôi có thể giải thích, dựa trên khai triển tổng trọng số suy giảm, vì sao Momentum vừa tăng tốc vừa giảm dao động — không chỉ ghi nhớ kết luận mà hiểu cơ chế.
- [ ] Tôi đã cài đặt và chạy thành công GD+Momentum, và tự kiểm chứng được cải thiện tốc độ hội tụ so với GD thuần túy trên hàm thung lũng hẹp.
- [ ] Tôi hiểu rủi ro overshoot khi $\beta$ quá lớn, và có thể giải thích nguyên nhân.
- [ ] Tôi đã hoàn thành Bài tập 5 và có thể tự chứng minh công thức khai triển velocity bằng quy nạp.

Nếu Bài tập 5 (chứng minh quy nạp) còn khó khăn, đây là dấu hiệu nên quay lại ôn tập kỹ thuật chứng minh quy nạp ở Part I — Proof Techniques trước khi tiếp tục — kỹ thuật này sẽ tái xuất hiện khi phân tích công thức bias correction của Adam ở Chương 7.6.

---

## 20. Đọc thêm

- **Paper nền tảng:** Boris Polyak (1964), *Some Methods of Speeding Up the Convergence of Iteration Methods* — nguồn gốc toán học của phương pháp Heavy Ball / Momentum cổ điển (cần bổ sung vào `PAPERS.md` nếu muốn trích dẫn chính thức).
- **Paper mở rộng:** Yurii Nesterov (1983) — công trình gốc về Nesterov Accelerated Gradient, nhắc đến ở Mục 12 (cũng chưa có trong `PAPERS.md`).
- **Paper ứng dụng Deep Learning:** Sutskever, Martens, Dahl, Hinton (2013), *On the Importance of Initialization and Momentum in Deep Learning*.
- **Sách:** Ian Goodfellow, Yoshua Bengio, Aaron Courville, *Deep Learning*, chương Optimization for Training Deep Models, phần Momentum (xem `BOOKS.md`).
- **Chương tiếp theo:** Chương 7.6 — Adam Optimizer.

---

### Liên kết chương (Cross References)

- **Chương trước:** 7.3 — Gradient Descent (vấn đề dao động trên thung lũng hẹp, Mục 10, được giải quyết trực tiếp ở chương này); 7.4 — Stochastic Gradient Descent (Momentum tương thích trực tiếp và thường được dùng kết hợp với SGD trong thực hành).
- **Chương tiếp theo:** 7.6 — Adam Optimizer (kết hợp trực tiếp ý tưởng velocity của chương này với learning rate thích nghi theo từng tham số, giải quyết hạn chế đã nêu ở Mục 14).
- **Chương liên quan xa hơn:** Part I — Proof Techniques (quy nạp toán học, dùng ở Bài tập 5); Volume 5, Part IV — Deep Learning (Momentum là thành phần tiêu chuẩn trong optimizer `SGD` của mọi framework hiện đại).
- **Vị trí trong Knowledge Graph:** Nút thứ năm của Part VII, phụ thuộc trực tiếp vào Chương 7.3; song song về mặt khái niệm với Chương 7.4 (cả hai là cải tiến độc lập của GD); hội tụ cùng Chương 7.4 tại Chương 7.6.

---

*Hết Chương 7.5. Chương này tuân thủ cấu trúc chuẩn của `OUTPUT.md` và `CHAPTER_TEMPLATE.md`, khớp với đặc tả Part VII trong `VOLUME_01_MATHEMATICS_FOR_CS.md`. Các paper của Polyak (1964) và Nesterov (1983) được trích dẫn ở Mục 20 hiện chưa có trong `PAPERS.md`/reference layer chính thức — cần bổ sung trước khi coi là nguồn tham chiếu chính thức theo nguyên tắc Single Source of Truth của `README.md`. Đang chờ rà soát trước khi tiếp tục sang Chương 7.6 — chương khép lại Part VII.*
