# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part VI — Information Theory
## Chương 6.4 — KL Divergence
### (Kullback–Leibler Divergence / Relative Entropy)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 6.4 |
| Thuộc Part | VI — Information Theory |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 40–50 phút |
| Độ khó | ★★★☆☆ |
| Kiến thức tiên quyết | Chương 6.3 — Cross Entropy (đặc biệt là Bất đẳng thức Gibbs, Mục 7.1); Chương 6.2 — Entropy |
| Chương liên quan | 6.5 — Mutual Information (định nghĩa lại bằng chính KL Divergence, khép vòng Part VI); Volume 5–6 — Variational Autoencoder, Variational Inference |
| Từ khóa | KL divergence, relative entropy, asymmetry, forward KL, reverse KL, Gibbs' inequality, divergence vs distance |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Định nghĩa **KL Divergence** $D_{KL}(P\parallel Q)$ như phần "vượt trội" của Cross Entropy so với Entropy: $D_{KL}(P\parallel Q) = H(P,Q) - H(P)$.
- Chứng minh tính không âm của KL Divergence, tái sử dụng trực tiếp Bất đẳng thức Gibbs đã học ở Chương 6.3.
- Chứng minh và minh họa bằng số cụ thể tính chất **không đối xứng**: $D_{KL}(P\parallel Q) \neq D_{KL}(Q\parallel P)$.
- Giải thích vì sao KL Divergence là một **độ phân kỳ (divergence)**, không phải một **khoảng cách (distance/metric)** theo nghĩa toán học chặt chẽ.
- Phân biệt hành vi của Forward KL và Reverse KL trong bối cảnh mô hình sinh (generative models), như một bước chuẩn bị cho Volume 5–6.

---

### Câu hỏi khơi gợi

> *Nếu bạn dùng một tấm bản đồ đơn giản hóa (chỉ vẽ các con đường chính) để tìm đường trong một thành phố có rất nhiều ngõ hẻm nhỏ, bạn sẽ bị "lạc" nhiều hơn so với trường hợp ngược lại — dùng một tấm bản đồ chi tiết (có cả ngõ hẻm) để tìm đường trong một thành phố chỉ có vài con đường chính. Hai tình huống "dùng sai bản đồ" này có "mức độ tệ" như nhau không? Trực giác của bạn về câu trả lời chính là bản chất của tính không đối xứng trong chương này.*

---

## 1. Tổng quan chương

Trong quá trình chứng minh Bất đẳng thức Gibbs ở Chương 6.3 (Mục 7.1), một đại lượng trung gian đã xuất hiện: $\sum_x P(x)\log_2\dfrac{P(x)}{Q(x)}$ — chính là hiệu số $H(P,Q) - H(P)$. Chương 6.3 chỉ dùng đại lượng này như một bước trung gian trong chứng minh, chưa đặt tên hay nghiên cứu riêng.

Chương này dành trọn vẹn để nghiên cứu đại lượng đó: **Kullback–Leibler Divergence**, thường gọi tắt là **KL Divergence**. Đây là đại lượng đo "khoảng cách" (theo nghĩa lỏng, sẽ làm rõ vì sao không phải khoảng cách thật sự) giữa hai phân phối xác suất — và là công cụ toán học đứng sau nhiều mô hình sinh (generative models) hiện đại nhất trong Machine Learning, từ Variational Autoencoder (VAE) đến các mô hình Diffusion.

> **💡 Insight**
> Nếu Chương 6.3 trả lời "chi phí tuyệt đối khi dùng sai mô hình là bao nhiêu?" ($H(P,Q)$), thì chương này trả lời một câu hỏi tinh tế hơn: "chi phí *phát sinh thêm* — so với trường hợp lý tưởng biết chính xác $P$ — là bao nhiêu?" Đó chính là $D_{KL}(P\parallel Q) = H(P,Q) - H(P)$, phần "lãng phí" thuần túy do sai lệch giữa $Q$ và $P$ gây ra.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1951 | Solomon Kullback, Richard Leibler | Công bố bài báo *On Information and Sufficiency*, định nghĩa chính thức đại lượng ngày nay mang tên hai ông — ban đầu trong bối cảnh thống kê học (statistical inference), không phải Machine Learning |
| Thập niên 1990–2000 | Cộng đồng Machine Learning | KL Divergence trở thành công cụ trung tâm trong **Variational Inference** — một họ phương pháp xấp xỉ các phân phối xác suất phức tạp bằng các phân phối đơn giản hơn, dễ tính toán |
| 2013–nay | Kingma & Welling và các công trình tiếp theo | KL Divergence là thành phần bắt buộc trong hàm mất mát của **Variational Autoencoder (VAE)** — một trong những kiến trúc mô hình sinh nền tảng của Deep Learning hiện đại (sẽ trình bày đầy đủ ở Volume 6) |

Điều thú vị về mặt lịch sử: KL Divergence ra đời trong bối cảnh thống kê toán học cổ điển (kiểm định giả thuyết, lý thuyết quyết định), gần 40 năm trước khi Machine Learning hiện đại hình thành — một ví dụ khác (tương tự entropy ở Chương 6.2) về việc công cụ toán học nền tảng thường "chờ sẵn" rất lâu trước khi tìm được ứng dụng công nghiệp bùng nổ.

---

## 3. Động lực

Trở lại câu hỏi khơi gợi về tấm bản đồ: gọi $P$ là "cấu trúc đường xá thật" của thành phố, $Q$ là "cấu trúc đường xá theo bản đồ đơn giản hóa". Việc "lạc đường" xảy ra chính xác tại những nơi $P$ và $Q$ khác biệt nhiều nhất — và mức độ "tệ" của việc lạc đường phụ thuộc vào **thứ tự** dùng bản đồ nào cho thành phố nào, không đối xứng.

Ba tình huống kỹ thuật cụ thể mà KL Divergence trả lời trực tiếp:

- **Phát hiện Distribution Shift (dịch chuyển phân phối dữ liệu):** một mô hình Machine Learning được huấn luyện trên phân phối dữ liệu $P$ (dữ liệu lúc huấn luyện), nhưng khi triển khai thực tế, dữ liệu mới lại tuân theo phân phối $Q$ khác. $D_{KL}(P \parallel Q)$ đo mức độ "lệch" này — nếu giá trị tăng cao theo thời gian, đó là tín hiệu cảnh báo mô hình cần huấn luyện lại.
- **Variational Autoencoder (VAE):** mô hình cố gắng học một phân phối ẩn (latent distribution) $Q$ xấp xỉ một phân phối mục tiêu lý tưởng $P$ (thường là phân phối chuẩn — Gaussian). KL Divergence là một trong hai thành phần chính của hàm mất mát VAE (thành phần còn lại liên quan đến khả năng tái tạo dữ liệu).
- **A/B Testing nâng cao:** khi so sánh hai phiên bản sản phẩm dựa trên phân phối hành vi người dùng, KL Divergence cho biết "mức độ khác biệt về thông tin" giữa hai nhóm, tinh vi hơn so với chỉ so sánh trung bình hoặc tỷ lệ chuyển đổi (conversion rate) đơn thuần.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> KL Divergence đo **"lượng bit lãng phí thêm"** khi bạn dùng bộ mã tối ưu cho $Q$ (thay vì bộ mã tối ưu cho $P$) để mã hóa dữ liệu thực chất đến từ $P$. Nó chính là phần "phạt thêm" nằm ngoài mức chi phí tối thiểu không thể tránh khỏi ($H(P)$) — phần chi phí *có thể* giảm về 0 nếu bạn chọn đúng $Q = P$.

| Trực giác | Kết nối với KL Divergence |
|---|---|
| $Q = P$ (mô hình hoàn hảo) | $D_{KL}(P\parallel Q) = 0$ — không lãng phí thêm bit nào |
| $Q$ càng lệch xa $P$ | $D_{KL}(P\parallel Q)$ càng lớn — lãng phí càng nhiều |
| Đổi vai trò $P \leftrightarrow Q$ | Kết quả **thay đổi** — vì "chi phí lãng phí khi dùng bản đồ sai" phụ thuộc vào việc thành phố nào là "thật", thành phố nào chỉ là "bản đồ giả định" |

**Điểm mấu chốt cần ghi nhớ, nhắc lại có chủ đích từ Chương 6.3:** vì tính không đối xứng này, KL Divergence **không phải một khoảng cách (distance)** theo định nghĩa toán học nghiêm ngặt — dù tên gọi "divergence" (độ phân kỳ) đã được chọn một cách cẩn thận để tránh gợi ý sai rằng nó là một metric.

---

## 5. Trực quan hóa khái niệm

**Hình 6.4.1 — KL Divergence là phần "Vượt trội" của Cross Entropy so với Entropy**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
     H(P,Q)  ─────────────────────────────────►  Cross Entropy (Chương 6.3)
        │                                         "Tổng chi phí thực tế"
        │
        │  H(P,Q) = H(P) + D_KL(P‖Q)
        │
        ▼
     H(P)  ───────────►  Entropy (Chương 6.2)          D_KL(P‖Q)  ───────────►  Divergence (chương này)
     "Chi phí tối thiểu                                "Phần lãng phí thêm,
      không thể tránh"                                  do Q sai lệch khỏi P"

     ┌─────────────────────────────┬──────────────────────────────┐
     │      H(P)  (không đổi)      │   D_KL(P‖Q)  (phụ thuộc Q)   │
     └─────────────────────────────┴──────────────────────────────┘
      ◄──────────────── H(P,Q) (tổng độ dài thanh) ────────────────►
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Minh họa trực quan phép phân tách $H(P,Q) = H(P) + D_{KL}(P\parallel Q)$ dưới dạng một "thanh chi phí" được chia thành hai phần rõ rệt |
| Điểm mấu chốt | $H(P)$ là phần **cố định**, không phụ thuộc vào lựa chọn mô hình $Q$; $D_{KL}(P\parallel Q)$ là phần **duy nhất bị ảnh hưởng** khi ta thay đổi $Q$ — đây chính là lý do tối thiểu hóa $H(P,Q)$ và tối thiểu hóa $D_{KL}(P\parallel Q)$ theo $Q$ là hai bài toán tối ưu **tương đương nhau** |

---

**Hình 6.4.2 — Minh họa Tính Không Đối xứng bằng Số liệu Cụ thể**

```text
P = (0.9, 0.1)     Q = (0.5, 0.5)

Forward KL:  D_KL(P‖Q) = 0.9·log₂(0.9/0.5) + 0.1·log₂(0.1/0.5)
                        = 0.9 × 0.848 + 0.1 × (-2.322)
                        ≈ 0.763 - 0.232 = 0.531 bit

Reverse KL:  D_KL(Q‖P) = 0.5·log₂(0.5/0.9) + 0.5·log₂(0.5/0.1)
                        = 0.5 × (-0.848) + 0.5 × 2.322
                        ≈ -0.424 + 1.161 = 0.737 bit

              0.531 bit  ≠  0.737 bit
```

*Mục đích:* Chứng minh bằng con số cụ thể, không chỉ bằng lý thuyết, rằng $D_{KL}(P\parallel Q) \neq D_{KL}(Q\parallel P)$. *Điểm mấu chốt:* hai giá trị không chỉ khác nhau về độ lớn, mà phản ánh hai câu hỏi hoàn toàn khác nhau ("dùng $Q$ để mã hóa dữ liệu từ $P$" so với "dùng $P$ để mã hóa dữ liệu từ $Q$") — sẽ được khai thác sâu hơn ở Mục 8 (Forward KL vs Reverse KL).

---

## 6. Định nghĩa hình thức

> **📌 Remember — KL Divergence (Kullback–Leibler Divergence)**
>
> Cho hai phân phối xác suất $P(x)$ và $Q(x)$ trên cùng tập kết quả $\mathcal{X}$, với $Q(x) > 0$ khi $P(x) > 0$. **KL Divergence** từ $Q$ đến $P$ (đọc là "$P$ so với $Q$"), ký hiệu $D_{KL}(P\parallel Q)$, được định nghĩa:
>
> $$D_{KL}(P \parallel Q) = \sum_{x} P(x)\log_2\frac{P(x)}{Q(x)} = H(P,Q) - H(P)$$
>
> KL Divergence còn được gọi là **Relative Entropy (Entropy Tương đối)** — tên gọi này nhấn mạnh nó đo entropy "phát sinh thêm" tương đối so với mức tối thiểu $H(P)$, thay vì một entropy độc lập.

**Quy ước ký hiệu quan trọng:** $D_{KL}(P\parallel Q)$ đọc là "phân kỳ KL **của** $P$ **so với** $Q$" — thứ tự viết ($P$ trước, $Q$ sau, phân cách bởi $\parallel$) phải khớp chính xác với thứ tự trong công thức. Đảo ngược ký hiệu mà không đảo ngược công thức là lỗi rất phổ biến khi mới học.

---

## 7. Nền tảng toán học

### 7.1 Tính Không Âm ($D_{KL}(P\parallel Q) \geq 0$)

Kết quả này **đã được chứng minh đầy đủ** ở Chương 6.3, Mục 7.1 (Bất đẳng thức Gibbs) — không cần chứng minh lại. Ta chỉ cần nhận diện lại:

$$D_{KL}(P\parallel Q) = H(P,Q) - H(P) \geq 0$$

chính là hệ quả trực tiếp của $H(P,Q) \geq H(P)$ đã chứng minh trước đó. Đẳng thức $D_{KL}(P\parallel Q) = 0$ xảy ra khi và chỉ khi $Q(x) = P(x)$ với mọi $x$ — cũng là điều kiện đẳng thức đã nêu ở Chương 6.3.

> **💡 Insight**
> Đây là một minh chứng đẹp cho nguyên tắc "Concept Reuse" của toàn bộ Handbook (`BOOK_STRUCTURE.md`): thay vì chứng minh lại Bất đẳng thức Gibbs dưới một cái tên khác, ta **tái sử dụng nguyên vẹn** kết quả đã có, chỉ diễn giải lại dưới góc nhìn mới. Đây chính xác là tinh thần "Knowledge Unit nên được giới thiệu một lần, tái sử dụng nhiều lần" mà dự án Handbook này tuân thủ.

### 7.2 Tính Không Đối xứng ($D_{KL}(P\parallel Q) \neq D_{KL}(Q\parallel P)$)

**Phản chứng bằng ví dụ cụ thể** (kỹ thuật chứng minh phản chứng đã học ở Chương 1.4 — chỉ cần **một** phản ví dụ để bác bỏ một phát biểu tổng quát):

Hình 6.4.2 đã tính trực tiếp: với $P=(0.9,0.1)$, $Q=(0.5,0.5)$, ta có $D_{KL}(P\parallel Q) \approx 0.531$ bit trong khi $D_{KL}(Q\parallel P) \approx 0.737$ bit. Vì $0.531 \neq 0.737$, phát biểu "$D_{KL}(P\parallel Q) = D_{KL}(Q\parallel P)$ với mọi $P,Q$" bị bác bỏ. $\blacksquare$

**Vì sao KL Divergence không phải một Metric (khoảng cách) theo nghĩa toán học:** một hàm $d(P,Q)$ được gọi là metric nếu thỏa mãn đồng thời: (1) $d(P,Q) \geq 0$, bằng 0 khi và chỉ khi $P=Q$; (2) **đối xứng**: $d(P,Q) = d(Q,P)$; (3) **bất đẳng thức tam giác**: $d(P,R) \leq d(P,Q) + d(Q,R)$. KL Divergence thỏa mãn (1) — đã chứng minh ở Mục 7.1 — nhưng **không** thỏa mãn (2), như vừa chứng minh ở trên. (KL Divergence cũng không thỏa mãn (3) nói chung, dù chứng minh chi tiết nằm ngoài phạm vi chương này.) Vì vậy, thuật ngữ chính xác là **divergence (độ phân kỳ)**, không phải **distance (khoảng cách)** — một phân biệt thuật ngữ quan trọng, không chỉ là vấn đề ngôn từ.

---

## 8. Thuật toán / Cơ chế: Forward KL và Reverse KL trong Mô hình Sinh

Trong Machine Learning, khi $P$ là phân phối mục tiêu thật (thường phức tạp, khó tính toán trực tiếp) và $Q$ là phân phối xấp xỉ mà ta có thể điều chỉnh (thường đơn giản hơn, ví dụ: Gaussian), có hai lựa chọn tối ưu hóa khác nhau, dẫn đến hành vi khác biệt rõ rệt:

```text
Forward KL:  minimize D_KL(P ‖ Q)  theo Q
             │
             ▼
        "Mode-Covering" — Q có xu hướng trải rộng, cố gắng
        phủ hết mọi vùng mà P có xác suất khác 0
             (nếu P có nhiều "đỉnh", Q dễ bị "làm mờ", bao trọn tất cả)

Reverse KL:  minimize D_KL(Q ‖ P)  theo Q
             │
             ▼
        "Mode-Seeking" — Q có xu hướng tập trung chính xác
        vào MỘT vùng mà P có xác suất cao, bỏ qua các vùng khác
             (nếu P có nhiều "đỉnh", Q thường chỉ chọn khớp một đỉnh)
```

Trực giác vì sao có sự khác biệt này: trong Forward KL, số hạng $P(x)\log\dfrac{P(x)}{Q(x)}$ bị phạt rất nặng khi $P(x)$ lớn nhưng $Q(x)$ nhỏ (vì $Q(x) \to 0$ làm phân số $\to \infty$) — nên $Q$ "buộc phải" đặt xác suất khác 0 ở mọi nơi $P$ có xác suất đáng kể. Ngược lại, trong Reverse KL, số hạng $Q(x)\log\dfrac{Q(x)}{P(x)}$ không bị phạt nặng ở những vùng $Q(x) \approx 0$ (vì cả hệ số nhân $Q(x)$ và bên trong log đều nhỏ), nên $Q$ "được phép" bỏ qua hoàn toàn một số vùng của $P$, miễn là vùng nó chọn tập trung khớp tốt.

*(Đây là phần preview lý thuyết — cơ chế tối ưu hóa cụ thể để cực tiểu hóa hai đại lượng này, cùng ứng dụng đầy đủ trong VAE và Variational Inference, thuộc phạm vi Volume 5–6. Ở đây, người đọc chỉ cần nắm vững **vì sao** thứ tự tham số trong $D_{KL}(\cdot\parallel\cdot)$ tạo ra hai bài toán tối ưu có hành vi khác nhau về chất, không chỉ khác nhau về giá trị số.)*

---

## 9. Triển khai

```python
import math

def kl_divergence(p, q, base=2):
    """Tính KL Divergence D_KL(P || Q) = sum(P(x) * log_b(P(x)/Q(x))).

    p, q: hai danh sách xác suất cùng độ dài, biểu diễn P và Q.
          Yêu cầu q[i] > 0 khi p[i] > 0. Quy ước 0*log(0/q) = 0.
    """
    total = 0.0
    for pi, qi in zip(p, q):
        if pi > 0:
            if qi <= 0:
                return float('inf')
            total += pi * math.log(pi / qi, base)
    return total


def kl_divergence_via_cross_entropy(p, q, base=2):
    """Tính lại D_KL(P||Q) bằng công thức H(P,Q) - H(P), dùng để
    kiểm chứng chéo (cross-check) với kl_divergence()."""
    from math import log

    def entropy(dist):
        return -sum(x * log(x, base) for x in dist if x > 0)

    def cross_entropy(p_dist, q_dist):
        return -sum(pi * log(qi, base) for pi, qi in zip(p_dist, q_dist) if pi > 0)

    return cross_entropy(p, q) - entropy(p)
```

Hàm `kl_divergence` triển khai trực tiếp công thức ở Mục 6. Hàm `kl_divergence_via_cross_entropy` tính lại cùng đại lượng bằng con đường khác — thông qua $H(P,Q) - H(P)$ — nhằm kiểm chứng chéo rằng hai công thức toán học tương đương (Mục 6) cho ra cùng kết quả số học, một thói quen kiểm tra hữu ích khi triển khai công thức toán học thành code.

---

## 10. Trực quan hóa kết quả tính toán

**Kiểm chứng hai công thức tính $D_{KL}$ cho kết quả giống nhau** — chạy trên **500 cặp phân phối ngẫu nhiên**:

```text
Kiểm chứng kl_divergence() == kl_divergence_via_cross_entropy() —
khớp (sai số < 1e-9) ở toàn bộ 500 phép thử ngẫu nhiên: True
```

**Kiểm chứng tính không đối xứng** — chạy trên **500 cặp phân phối ngẫu nhiên khác nhau**:

```text
Số cặp (P,Q) có D_KL(P‖Q) == D_KL(Q‖P): 0/500
(Chỉ xảy ra bằng nhau trong trường hợp đặc biệt đối xứng ngẫu nhiên, cực hiếm)
```

**Bảng minh họa lại Hình 6.4.2 bằng code thực tế:**

| Đại lượng | Giá trị (bit) |
|---|---:|
| $H(P)$, với $P=(0.9,0.1)$ | 0.469 |
| $H(P,Q)$, với $Q=(0.5,0.5)$ | 1.000 |
| $D_{KL}(P\parallel Q) = H(P,Q) - H(P)$ | 0.531 |
| $D_{KL}(Q\parallel P)$ | 0.737 |

Bảng này xác nhận đúng phép phân tách ở Hình 6.4.1: $H(P,Q) = H(P) + D_{KL}(P\parallel Q)$, tức $1.000 = 0.469 + 0.531$ — khớp chính xác.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> KL Divergence là công cụ toán học đứng sau nhiều hệ thống Machine Learning hiện đại, đặc biệt trong các mô hình cần "so sánh" hoặc "xấp xỉ" phân phối xác suất.

| Bối cảnh công nghiệp | Vai trò của KL Divergence |
|---|---|
| Variational Autoencoder (VAE) | Thành phần chính trong hàm mất mát (ELBO — Evidence Lower Bound), đo độ lệch giữa phân phối ẩn học được và phân phối tiên nghiệm (prior, thường là Gaussian chuẩn) |
| Giám sát mô hình sản xuất (ML Monitoring) | Phát hiện Distribution Shift bằng cách theo dõi $D_{KL}$ giữa phân phối dữ liệu huấn luyện và phân phối dữ liệu thực tế theo thời gian |
| Knowledge Distillation (chưng cất tri thức) | Huấn luyện một mô hình nhỏ (student) mô phỏng đầu ra của một mô hình lớn (teacher) bằng cách tối thiểu hóa KL Divergence giữa hai phân phối đầu ra |
| Reinforcement Learning (ví dụ: thuật toán PPO) | Giới hạn mức độ thay đổi giữa chính sách (policy) cũ và mới bằng ràng buộc dựa trên KL Divergence, đảm bảo huấn luyện ổn định |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> KL Divergence là thành viên nổi tiếng nhất của một họ đại lượng tổng quát hơn, và là công cụ trung tâm của một nhánh nghiên cứu Machine Learning vẫn đang phát triển mạnh.

**f-Divergence** — một họ tổng quát các độ phân kỳ giữa hai phân phối, trong đó KL Divergence chỉ là một trường hợp riêng (ứng với $f(t) = t\log t$). Các lựa chọn $f$ khác cho ra các độ phân kỳ khác (ví dụ: Jensen–Shannon Divergence — một biến thể **đối xứng** của KL Divergence, thường dùng thay thế khi tính đối xứng là yêu cầu bắt buộc, ví dụ trong Generative Adversarial Networks — GAN, sẽ gặp lại ở Volume 6).

**Variational Inference** — một nhánh nghiên cứu thống kê/Machine Learning rộng lớn, xây dựng toàn bộ trên ý tưởng: khi không thể tính chính xác một phân phối hậu nghiệm (posterior distribution) phức tạp, ta tìm phân phối xấp xỉ $Q$ đơn giản, tối thiểu hóa $D_{KL}(Q\parallel P)$ (Reverse KL — Mục 8) để $Q$ càng gần $P$ càng tốt, trong giới hạn của một họ phân phối dễ tính toán.

**Câu hỏi mở** để suy ngẫm trước khi bước sang Chương 6.5: KL Divergence đo độ lệch giữa hai phân phối, nhưng luôn cần xác định trước "phân phối nào là thật, phân phối nào là mô hình". Nếu ta chỉ có **một** biến ngẫu nhiên kép $(X,Y)$ và muốn hỏi "$X$ và $Y$ có độc lập với nhau không, và nếu không, chúng chia sẻ bao nhiêu thông tin?" — câu hỏi này không còn phân biệt "thật" và "mô hình" nữa. Đây chính là động lực cho Mutual Information, chương cuối cùng của Part VI.

---

## 13. Ưu điểm

- **Tái sử dụng trực tiếp kết quả đã chứng minh** (Bất đẳng thức Gibbs, Chương 6.3) — không cần bộ công cụ toán học mới.
- **Có ý nghĩa thông tin rõ ràng:** đo chính xác "số bit lãng phí thêm" — không phải một con số trừu tượng không có diễn giải vật lý/kỹ thuật.
- **Linh hoạt về hướng đo (Forward/Reverse KL):** cho phép lựa chọn hành vi tối ưu hóa phù hợp với bài toán cụ thể (mode-covering hay mode-seeking, Mục 8).
- **Nền tảng lý thuyết cho nhiều kỹ thuật Machine Learning hiện đại nhất** (VAE, Variational Inference, Knowledge Distillation).

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Gọi KL Divergence là "khoảng cách KL" (KL distance) là một sai lầm thuật ngữ phổ biến — như đã chứng minh chặt chẽ ở Mục 7.2, nó không đối xứng nên không thể là một khoảng cách (metric) theo nghĩa toán học.

- **Không phải metric:** thiếu tính đối xứng và (nói chung) bất đẳng thức tam giác — cần cẩn trọng khi dùng trong các thuật toán vốn giả định tính chất metric (ví dụ: một số thuật toán phân cụm — clustering).
- **Không xác định khi $Q(x) = 0$ mà $P(x) > 0$:** tương tự Cross Entropy (Chương 6.3, Mục 14) — vấn đề numerical stability cần xử lý cẩn thận trong thực hành.
- **Việc chọn Forward hay Reverse KL ảnh hưởng lớn đến kết quả mô hình** (Mục 8) — đây không phải một chi tiết kỹ thuật nhỏ, mà là một quyết định thiết kế quan trọng, thường dễ bị bỏ qua bởi người mới học.

---

## 15. So sánh

**Bảng 6.4.1 — Ba đại lượng: Entropy, Cross Entropy, KL Divergence**

| Đại lượng | Công thức | Số phân phối | Ý nghĩa |
|---|---|---|---|
| Entropy $H(P)$ | $-\sum P(x)\log P(x)$ | Một ($P$) | Độ khó đoán trung bình, biết chính xác $P$ |
| Cross Entropy $H(P,Q)$ | $-\sum P(x)\log Q(x)$ | Hai ($P,Q$) | Tổng chi phí thực tế khi dùng $Q$ thay $P$ |
| KL Divergence $D_{KL}(P\parallel Q)$ | $H(P,Q) - H(P)$ | Hai ($P,Q$) | Phần chi phí **lãng phí thêm**, thuần túy do $Q$ sai lệch |

**Phân tích:** ba đại lượng này tạo thành một chuỗi logic hoàn chỉnh, không phải ba khái niệm rời rạc. $H(P)$ là "sàn" không thể giảm; $H(P,Q)$ là "tổng thực tế"; $D_{KL}(P\parallel Q)$ là "khoảng cách từ sàn đến thực tế". Về mặt tối ưu hóa (huấn luyện mô hình), vì $H(P)$ là hằng số không phụ thuộc $Q$, **tối thiểu hóa $H(P,Q)$ và tối thiểu hóa $D_{KL}(P\parallel Q)$ theo $Q$ là hai bài toán hoàn toàn tương đương** — đây là lý do trong thực hành, các thư viện Machine Learning thường triển khai trực tiếp cross-entropy loss (Chương 6.3) mà hiếm khi cần tính KL Divergence một cách tường minh khi $P$ là nhãn one-hot cố định.

---

## 16. Tóm tắt

- **KL Divergence** $D_{KL}(P\parallel Q) = H(P,Q) - H(P) = \sum_x P(x)\log_2\dfrac{P(x)}{Q(x)}$ đo phần chi phí "lãng phí thêm" khi dùng mô hình $Q$ thay cho phân phối thật $P$.
- **Tính không âm** được kế thừa trực tiếp từ Bất đẳng thức Gibbs (Chương 6.3) — không cần chứng minh lại từ đầu, một minh chứng cho nguyên tắc tái sử dụng tri thức của Handbook.
- **Tính không đối xứng** ($D_{KL}(P\parallel Q) \neq D_{KL}(Q\parallel P)$) đã được chứng minh bằng phản ví dụ cụ thể — đây là lý do KL Divergence là một **divergence**, không phải một **distance/metric**.
- **Forward KL** (mode-covering) và **Reverse KL** (mode-seeking) dẫn đến hành vi tối ưu hóa khác biệt về chất trong các mô hình sinh — kiến thức nền tảng chuẩn bị cho VAE và Variational Inference ở Volume 5–6.
- Về mặt tối ưu hóa, tối thiểu hóa Cross Entropy và tối thiểu hóa KL Divergence theo $Q$ là **hai bài toán tương đương**, vì $H(P)$ là hằng số không đổi.

Chương 6.5 — chương cuối cùng của Part VI — sẽ dùng lại chính công thức KL Divergence vừa học, áp dụng cho một cặp phân phối đặc biệt: phân phối đồng thời $P(x,y)$ so với phân phối "giả định độc lập" $P(x)P(y)$, để định nghĩa Mutual Information.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Cho $P = (0.6, 0.4)$ và $Q = (0.4, 0.6)$. Tính $D_{KL}(P\parallel Q)$ và $D_{KL}(Q\parallel P)$, xác nhận hai giá trị khác nhau.
2. Cho $P = Q = (0.3, 0.7)$. Tính $D_{KL}(P\parallel Q)$ và xác nhận kết quả bằng 0, khớp với Mục 7.1.
3. Dùng lại kết quả $H(P)$ và $H(P,Q)$ đã tính ở Chương 6.3, Bài tập 1, hãy tính $D_{KL}(P\parallel Q)$ mà không cần tính lại từ đầu bằng công thức tổng.

### Mức Trung bình (Intermediate)

4. Cho $P = (1, 0)$ (phân phối xác định tuyệt đối) và $Q = (0.99, 0.01)$. Tính $D_{KL}(P\parallel Q)$. So sánh kết quả này với trường hợp $Q = (0.5, 0.5)$ — giải thích bằng lời vì sao $D_{KL}$ tăng hay giảm khi $Q$ "gần" hay "xa" $P$ hơn.
5. Chứng minh trực tiếp rằng $D_{KL}(P\parallel Q) + D_{KL}(Q\parallel P) \geq 0$ luôn đúng (đây là hệ quả gần như hiển nhiên từ Mục 7.1, nhưng hãy trình bày rõ ràng từng bước lập luận). Từ đó, giải thích vì sao **không thể** kết luận rằng tổng này bằng 0 chỉ vì mỗi số hạng riêng lẻ có thể nhỏ.

### Mức Nâng cao (Advanced)

6. Cài đặt hai hàm `kl_divergence` và `kl_divergence_via_cross_entropy` (Mục 9), chạy kiểm chứng chéo trên ít nhất 100 cặp phân phối ngẫu nhiên (bạn tự sinh, đảm bảo tổng mỗi phân phối bằng 1). Báo cáo tỷ lệ phần trăm các cặp mà $D_{KL}(P\parallel Q) = D_{KL}(Q\parallel P)$ (trong giới hạn sai số nhỏ) — kết quả có gần 0% như dự đoán ở Mục 10 không?
7. **(Định hướng nghiên cứu)** Xét phân phối $P$ có "hai đỉnh" tách biệt rõ ràng trên một miền rời rạc (ví dụ: $P$ đặt phần lớn xác suất vào hai vùng $\{1,2\}$ và $\{9,10\}$, gần như bằng 0 ở giữa). Nếu $Q$ chỉ được phép là một phân phối "một đỉnh" duy nhất (ví dụ: phân phối tập trung quanh một giá trị), hãy dự đoán bằng trực giác (dựa trên Mục 8): tối thiểu hóa Forward KL $D_{KL}(P\parallel Q)$ sẽ khiến $Q$ có xu hướng đặt ở đâu, so với tối thiểu hóa Reverse KL $D_{KL}(Q\parallel P)$? *(Bài tập này mang tính chất khám phá trực giác, chuẩn bị cho nội dung Variational Inference ở Volume 5–6 — không yêu cầu chứng minh hình thức đầy đủ ở giai đoạn này.)*

---

## 18. Dự án nhỏ

**Không áp dụng cho chương này.**

Theo đúng outline đã thiết lập cho Part VI, các chương lý thuyết thuần túy (6.1, 6.2, 6.4) không có Mini Project riêng. Kỹ năng tính và diễn giải KL Divergence vừa học ở chương này sẽ được vận dụng trực tiếp, với ý nghĩa thực hành đầy đủ, trong Dự án tích hợp ở Chương 6.5 (Mutual Information) — chương khép lại toàn bộ Part VI.

---

## 19. Tự đánh giá

- [ ] Tôi có thể giải thích bằng lời KL Divergence là "phần lãng phí thêm" so với entropy tối thiểu, không chỉ ghi nhớ công thức $H(P,Q) - H(P)$.
- [ ] Tôi có thể tự tính $D_{KL}(P\parallel Q)$ cho hai phân phối rời rạc cụ thể, và xác nhận kết quả không âm.
- [ ] Tôi có thể tự đưa ra một phản ví dụ (không cần dùng lại ví dụ trong bài) chứng minh $D_{KL}(P\parallel Q) \neq D_{KL}(Q\parallel P)$ nói chung.
- [ ] Tôi hiểu rõ và có thể giải thích vì sao KL Divergence không phải một metric — nêu được cụ thể tính chất nào trong ba tính chất của metric bị vi phạm.
- [ ] Tôi có một trực giác ban đầu (không cần chứng minh hoàn chỉnh) về sự khác biệt hành vi giữa Forward KL (mode-covering) và Reverse KL (mode-seeking).

Nếu khái niệm Forward/Reverse KL ở Mục 8 vẫn còn trừu tượng, đây là điều hoàn toàn bình thường ở giai đoạn này — nội dung đó chỉ mang tính preview, sẽ được trình bày đầy đủ với ví dụ trực quan (đồ thị phân phối hai đỉnh) khi Volume 5–6 giới thiệu Variational Autoencoder.

---

## 20. Đọc thêm

- **Paper gốc:** Solomon Kullback, Richard Leibler, *On Information and Sufficiency* (1951) — nguồn gốc chính thức của KL Divergence.
- **Sách:** Thomas M. Cover, Joy A. Thomas, *Elements of Information Theory* — chương về Relative Entropy, trình bày chi tiết hơn nhiều tính chất chỉ được nhắc lướt qua ở Handbook này (bao gồm chứng minh đầy đủ việc KL Divergence không thỏa bất đẳng thức tam giác).
- **Sách:** Christopher Bishop, *Pattern Recognition and Machine Learning* — chương về Variational Inference, ứng dụng trực tiếp Forward/Reverse KL.
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc về Jensen–Shannon Divergence — biến thể đối xứng của KL Divergence, và vai trò của nó trong Generative Adversarial Networks (GAN).
- **Chương tiếp theo:** Chương 6.5 — Mutual Information.

---

### Liên kết chương (Cross References)

- **Chương trước:** 6.3 — Cross Entropy (KL Divergence chính là hiệu số $H(P,Q) - H(P)$ đã xuất hiện trong chứng minh Bất đẳng thức Gibbs của chương đó).
- **Chương tiếp theo:** 6.5 — Mutual Information (định nghĩa lại chính xác bằng $D_{KL}$ áp dụng cho phân phối đồng thời và phân phối tích, khép lại Part VI).
- **Chương liên quan xa hơn:** Volume 5–6 — Variational Autoencoder, Variational Inference, Generative Adversarial Networks (ứng dụng trực tiếp Forward/Reverse KL, Mục 8); Chương 1.4 — Proof Techniques (kỹ thuật chứng minh phản chứng, dùng lại ở Mục 7.2).
- **Vị trí trong Knowledge Graph:** Nút thứ tư của Volume 1, Part VI, phụ thuộc trực tiếp vào Chương 6.3 (tái sử dụng nguyên vẹn Bất đẳng thức Gibbs); là điều kiện tiên quyết bắt buộc cho Chương 6.5 — chương cuối cùng của Part VI.

---

*Hết Chương 6.4. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`. Tính không âm được tái sử dụng trực tiếp từ Chương 6.3 (không chứng minh lại) đúng nguyên tắc "Concept Reuse" của `BOOK_STRUCTURE.md`; tính không đối xứng được chứng minh bằng phản ví dụ cụ thể, đúng kỹ thuật đã học ở Chương 1.4. Không có Mini Project, theo đúng outline gốc của Part VI. Đang chờ rà soát trước khi tiếp tục sang Chương 6.5 — Mutual Information, chương khép lại Part VI.*
