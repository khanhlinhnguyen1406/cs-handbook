# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part VI — Information Theory
## Chương 6.3 — Cross Entropy và Ứng dụng trong Machine Learning
### (Cross Entropy)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 6.3 |
| Thuộc Part | VI — Information Theory |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 45–55 phút |
| Độ khó | ★★★☆☆ |
| Kiến thức tiên quyết | Chương 6.2 — Entropy; Chương 6.1 — Foundations of Information |
| Chương liên quan | 6.4 — KL Divergence (Cross Entropy được phân tách chính xác thành $H(P) + D_{KL}(P\parallel Q)$); Volume 5, Part II — Machine Learning (Cross-Entropy Loss là hàm mất mát phổ biến nhất cho bài toán phân loại) |
| Từ khóa | cross entropy, estimated distribution, cross-entropy loss, binary classification, maximum likelihood, log loss |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Giải thích động lực của Cross Entropy: đo "chi phí" phải trả khi dùng một phân phối ước lượng $Q$ thay cho phân phối thật $P$.
- Định nghĩa và tính toán $H(P, Q) = -\sum_x P(x)\log Q(x)$ cho các phân phối rời rạc cụ thể.
- Chứng minh bất đẳng thức nền tảng $H(P,Q) \geq H(P)$, với đẳng thức xảy ra khi và chỉ khi $Q = P$.
- Cài đặt **Binary Cross-Entropy Loss** — hàm mất mát trung tâm của bài toán phân loại nhị phân trong Machine Learning.
- Kết nối việc tối thiểu hóa cross entropy với nguyên lý **Maximum Likelihood Estimation (Ước lượng Hợp lý Cực đại)**.

---

### Câu hỏi khơi gợi

> *Hai người dự báo thời tiết cùng đưa ra dự báo cho 100 ngày liên tiếp. Người A luôn nói "70% khả năng có mưa" mỗi ngày, bất kể thời tiết thực tế thế nào. Người B cố gắng dự báo chính xác theo từng ngày, nhưng đôi khi rất tự tin sai — có ngày nói "99% chắc chắn không mưa" nhưng trời vẫn mưa to. Nếu bạn phải chấm điểm độ tin cậy của hai người dự báo này bằng một con số duy nhất, con số đó nên trừng phạt kiểu sai lầm nào nặng hơn: sai lầm "an toàn, không chắc chắn" hay sai lầm "tự tin nhưng sai"?*

---

## 1. Tổng quan chương

Chương 6.2 định nghĩa entropy $H(X)$ với một giả định ngầm quan trọng: ta *biết chính xác* phân phối xác suất thật $P(x)$. Nhưng trong hầu hết các bài toán kỹ thuật thực tế — đặc biệt là Machine Learning — giả định đó gần như không bao giờ đúng. Ta không biết phân phối thật của dữ liệu; ta chỉ có một **mô hình ước lượng** $Q(x)$, được xây dựng từ dữ liệu quan sát hoặc học được qua huấn luyện.

Chương này trả lời câu hỏi tự nhiên tiếp theo: nếu ta dùng $Q$ để "mã hóa" hoặc "dự đoán" các sự kiện thực chất tuân theo $P$, ta phải trả giá bao nhiêu cho sự sai lệch đó? Đại lượng trả lời chính xác câu hỏi này gọi là **Cross Entropy** — và nó không phải một khái niệm xa lạ, mà là hàm mất mát (loss function) bạn sẽ gặp lại ở hầu như mọi mô hình phân loại trong Volume 5.

> **💡 Insight**
> Nếu Chương 6.2 đo "độ khó đoán trung bình của một nguồn tin, khi biết chính xác nguồn đó", thì chương này đo "cái giá phải trả trung bình khi bạn *đoán sai* về nguồn tin đó". Đây chính là bản chất của việc huấn luyện một mô hình Machine Learning: liên tục điều chỉnh $Q$ để cái giá đó càng nhỏ càng tốt.

---

## 2. Bối cảnh lịch sử

Cross Entropy không gắn với một khoảnh khắc phát minh đơn lẻ như self-information hay entropy — nó là một hệ quả toán học tự nhiên xuất hiện ngay trong khuôn khổ lý thuyết mà Shannon xây dựng năm 1948, khi các nhà nghiên cứu lý thuyết mã hóa bắt đầu hỏi: "điều gì xảy ra nếu bộ mã hóa (codebook) được thiết kế cho một phân phối, nhưng dữ liệu thực tế lại tuân theo phân phối khác?"

| Thời điểm | Bối cảnh | Ý nghĩa |
|---|---|---|
| Cuối thập niên 1940–1950 | Lý thuyết mã hóa (Coding Theory) | Các nhà nghiên cứu hình thức hóa "chi phí" của việc dùng sai bộ mã hóa — chính là cross entropy, dù thuật ngữ này được dùng phổ biến hơn về sau |
| 1986 | Rumelhart, Hinton, Williams — *Learning Representations by Back-Propagating Errors* | Công trình phổ biến hóa việc huấn luyện mạng neural bằng lan truyền ngược (backpropagation) — cross-entropy loss dần trở thành lựa chọn tiêu chuẩn cho các bài toán phân loại trong những thập kỷ sau đó (đã có trong `PAPERS.md`, sẽ trình bày đầy đủ ở Volume 5) |
| Hiện nay | Machine Learning hiện đại | Cross-Entropy Loss là hàm mất mát mặc định cho hầu hết bài toán phân loại — từ Logistic Regression đơn giản đến các mô hình Deep Learning phức tạp nhất |

Điểm đáng chú ý: cross entropy là một trong số ít khái niệm toán học "chờ sẵn" từ những năm 1940, nhưng chỉ thực sự trở thành công cụ phổ biến hàng ngày của hàng triệu kỹ sư sau khi Machine Learning bùng nổ — một minh chứng cho việc lý thuyết toán học nền tảng thường đi trước ứng dụng công nghiệp hàng chục năm.

---

## 3. Động lực

Quay lại câu hỏi khơi gợi: hãy hình thức hóa tình huống hai người dự báo thời tiết.

Gọi $P$ là phân phối thật (thời tiết thực tế xảy ra), và $Q$ là phân phối dự báo (mô hình ước lượng). Với mỗi ngày, "chi phí" ta phải trả khi tin vào dự báo $Q(x)$ trong khi kết quả thật là $x$ (xảy ra với xác suất $P(x)$) chính là self-information *tính theo* $Q$, tức $-\log_2 Q(x)$ — nhưng được lấy trung bình theo phân phối *thật* $P$.

Ba tình huống kỹ thuật cụ thể mà cross entropy trả lời trực tiếp:

- **Bài toán phân loại nhị phân (binary classification):** một mô hình dự đoán $Q(\text{spam}) = 0.95$ cho một email thực chất *không* phải spam ($P(\text{not spam}) = 1$). Cross entropy đo chính xác "mức phạt" cho dự đoán tự tin-nhưng-sai này — và mức phạt đó, như sẽ thấy ở Mục 7, tăng rất nhanh khi mô hình càng tự tin mà càng sai.
- **Đánh giá mô hình ngôn ngữ (language model):** một mô hình ngôn ngữ tốt nên gán xác suất cao cho các câu tự nhiên, hợp lý — cross entropy giữa phân phối văn bản thật và phân phối mô hình dự đoán chính là cơ sở của độ đo **Perplexity**, dùng để đánh giá chất lượng mô hình ngôn ngữ (sẽ gặp lại ở Volume 5–6).
- **Nén dữ liệu với bộ mã sai:** nếu ta thiết kế bộ mã Huffman (Chương 6.2, Mục 8) dựa trên ước lượng tần suất ký tự $Q$, nhưng văn bản thực tế tuân theo tần suất $P$ khác, độ dài mã trung bình thực tế sẽ là $H(P,Q)$, không phải $H(P)$ — luôn **tệ hơn hoặc bằng** trường hợp biết chính xác $P$ (Mục 7).

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Cross Entropy giống như việc bạn **thiết kế một hệ thống câu hỏi Có/Không tối ưu cho một thế giới giả định $Q$** (ví dụ: bạn nghĩ hầu hết mọi người thích chó), rồi áp dụng hệ thống câu hỏi đó vào **thế giới thật $P$** (thực tế: hầu hết mọi người thích mèo). Vì chiến lược câu hỏi của bạn được tối ưu cho sai đối tượng, số câu hỏi trung bình cần dùng trong thế giới thật sẽ **nhiều hơn** so với nếu bạn thiết kế đúng ngay từ đầu.

| Yếu tố | Vai trò |
|---|---|
| $P(x)$ | Phân phối **thật** — "thế giới thực tế xảy ra" |
| $Q(x)$ | Phân phối **ước lượng/mô hình** — "chiến lược ta dùng để mã hóa/dự đoán" |
| $H(P,Q)$ | Chi phí trung bình thực tế phải trả, khi thế giới tuân theo $P$ nhưng ta hành động theo $Q$ |

**Điểm mấu chốt cần ghi nhớ:** Cross Entropy **không đối xứng** — $H(P,Q) \neq H(Q,P)$ nói chung. Vai trò của $P$ (lấy trung bình theo) và $Q$ (dùng để "mã hóa") hoàn toàn khác nhau, không thể hoán đổi. Đây là điểm rất dễ nhầm lẫn, sẽ được nhấn mạnh lại ở Mục 14.

---

## 5. Trực quan hóa khái niệm

**Hình 6.3.1 — Cross Entropy như phần mở rộng của Entropy**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
                    Chương 6.2: Entropy H(X)
                    "Biết chính xác P, đo độ khó đoán trung bình"

                    H(X) = -Σ P(x)·log₂ P(x)
                                  │
                                  │  Thay P(x) thứ hai bằng Q(x)
                                  │  (mô hình ước lượng, có thể sai)
                                  ▼
                    Chương 6.3: Cross Entropy H(P,Q)
                    "Trung bình theo P thật, nhưng tính theo Q ước lượng"

                    H(P,Q) = -Σ P(x)·log₂ Q(x)
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Cho thấy Cross Entropy chỉ khác Entropy ở **đúng một vị trí duy nhất** trong công thức — thay $\log_2 P(x)$ bằng $\log_2 Q(x)$, trong khi trọng số lấy trung bình vẫn giữ nguyên là $P(x)$ |
| Điểm mấu chốt | Khi $Q = P$ (mô hình ước lượng hoàn hảo), $H(P,Q)$ thu gọn đúng về $H(P)$ — Mục 7 sẽ chứng minh đây chính là trường hợp **cực tiểu** của $H(P,Q)$ |

---

**Hình 6.3.2 — So sánh Hai Người Dự báo Thời tiết (từ Câu hỏi khơi gợi)**

```text
Ngày thực tế: KHÔNG MƯA  (P(không mưa) = 1, P(mưa) = 0)

Người dự báo A: Q_A(mưa) = 0.3,  Q_A(không mưa) = 0.7
  → Chi phí ngày này = -log₂ Q_A(không mưa) = -log₂ 0.7 ≈ 0.51 bit

Người dự báo B: Q_B(mưa) = 0.99, Q_B(không mưa) = 0.01
  → Chi phí ngày này = -log₂ Q_B(không mưa) = -log₂ 0.01 ≈ 6.64 bit
                                                              ↑
                                            Phạt RẤT nặng vì "tự tin nhưng sai"
```

*Mục đích:* Trả lời trực tiếp câu hỏi khơi gợi bằng số liệu cụ thể. *Điểm mấu chốt:* cross entropy phạt nặng hơn rất nhiều đối với dự đoán **tự tin nhưng sai** (Người B) so với dự đoán **không chắc chắn nhưng an toàn** (Người A) — đây là lý do cross entropy được ưa chuộng làm hàm mất mát: nó tự động khuyến khích mô hình "biết mình không biết" thay vì tự tin mù quáng.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Cross Entropy**
>
> Cho hai phân phối xác suất $P(x)$ (phân phối **thật**) và $Q(x)$ (phân phối **ước lượng**) trên cùng một tập kết quả $\mathcal{X}$, với điều kiện $Q(x) > 0$ với mọi $x$ mà $P(x) > 0$ (nếu không, $H(P,Q)$ có thể tiến tới $+\infty$ — xem Mục 14). **Cross Entropy** giữa $P$ và $Q$ được định nghĩa:
>
> $$H(P, Q) = -\sum_{x \in \mathcal{X}} P(x)\log_2 Q(x)$$
>
> Lưu ý thứ tự tham số quan trọng: $P$ luôn là phân phối lấy trung bình theo (đóng vai trò "trọng số"), $Q$ luôn là phân phối bên trong logarit (đóng vai trò "mô hình dùng để mã hóa/dự đoán"). Đảo ngược thứ tự cho ra một đại lượng khác, nói chung không bằng giá trị ban đầu.

---

## 7. Nền tảng toán học

### 7.1 Bất đẳng thức Nền tảng: $H(P,Q) \geq H(P)$

Đây là kết quả quan trọng nhất của chương này — nó chứng minh trực giác ở Mục 3–5: **dùng sai mô hình không bao giờ tốt hơn dùng đúng mô hình**, và bằng nhau chỉ khi mô hình đúng tuyệt đối.

> **📦 Formula Box — Bất đẳng thức Gibbs (Gibbs' Inequality)**
>
> $$H(P, Q) \geq H(P), \quad \text{với đẳng thức xảy ra khi và chỉ khi } Q(x) = P(x)\ \forall x$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | Diễn giải | Chi phí trung bình khi dùng mô hình ước lượng $Q$ **không bao giờ nhỏ hơn** chi phí khi biết chính xác $P$ |
> | **Ứng dụng thường gặp** | Nền tảng lý thuyết trực tiếp giải thích vì sao việc **tối thiểu hóa cross entropy** trong huấn luyện Machine Learning là một mục tiêu hợp lý: nó đẩy $Q$ tiến gần về $P$ |

**Chứng minh** (dùng bất đẳng thức cơ bản $\ln t \leq t - 1$ với mọi $t > 0$, đẳng thức khi $t=1$ — một kết quả đã quen thuộc từ Calculus, Part IV):

Xét hiệu $H(P,Q) - H(P)$:

$$H(P,Q) - H(P) = -\sum_x P(x)\log_2 Q(x) + \sum_x P(x)\log_2 P(x) = \sum_x P(x)\log_2\frac{P(x)}{Q(x)}$$

Đổi sang logarit tự nhiên (dùng $\log_2 t = \dfrac{\ln t}{\ln 2}$) và áp dụng bất đẳng thức $\ln t \leq t-1$ với $t = \dfrac{Q(x)}{P(x)}$:

$$-\ln\frac{P(x)}{Q(x)} = \ln\frac{Q(x)}{P(x)} \leq \frac{Q(x)}{P(x)} - 1$$

Suy ra $\log_2 \dfrac{P(x)}{Q(x)} \geq \dfrac{1}{\ln 2}\left(1 - \dfrac{Q(x)}{P(x)}\right)$. Nhân hai vế với $P(x) \geq 0$ rồi lấy tổng theo $x$:

$$\sum_x P(x)\log_2\frac{P(x)}{Q(x)} \geq \frac{1}{\ln 2}\sum_x \big(P(x) - Q(x)\big) = \frac{1}{\ln 2}\left(\sum_x P(x) - \sum_x Q(x)\right) = \frac{1}{\ln 2}(1 - 1) = 0$$

(vì cả $P$ và $Q$ đều là phân phối xác suất hợp lệ, nên $\sum_x P(x) = \sum_x Q(x) = 1$). Vậy $H(P,Q) - H(P) \geq 0$, tức $H(P,Q) \geq H(P)$. $\blacksquare$

> **💡 Insight**
> Đại lượng $\sum_x P(x)\log_2\dfrac{P(x)}{Q(x)}$ xuất hiện ngay giữa chứng minh này chính xác là **KL Divergence** — chủ đề trọn vẹn của Chương 6.4. Nói cách khác: $H(P,Q) = H(P) + D_{KL}(P \parallel Q)$. Chứng minh ở đây thực chất đã chứng minh luôn tính không âm của KL Divergence — Chương 6.4 sẽ khai thác kết quả này sâu hơn thay vì chứng minh lại từ đầu.

### 7.2 Cross Entropy và Maximum Likelihood Estimation

Một kết nối lý thuyết quan trọng, giải thích *vì sao* cross entropy được chọn làm hàm mất mát thay vì một độ đo "khoảng cách" nào khác: **tối thiểu hóa cross entropy tương đương về mặt toán học với Maximum Likelihood Estimation (MLE)** — nguyên lý ước lượng tham số bằng cách chọn mô hình khiến dữ liệu quan sát được có xác suất xảy ra cao nhất. Đây là một kết quả sẽ được trình bày đầy đủ khi Volume 5 giới thiệu MLE một cách hình thức; ở đây, người đọc chỉ cần ghi nhận: **cross entropy không phải một lựa chọn tùy ý**, mà có nền tảng xác suất thống kê vững chắc.

---

## 8. Thuật toán / Cơ chế: Cross-Entropy Loss trong Phân loại

Trong bài toán phân loại nhị phân (binary classification), gọi $y \in \{0, 1\}$ là nhãn thật (ví dụ: $1$ = spam, $0$ = không spam), và $\hat{y} \in (0,1)$ là xác suất mô hình dự đoán cho nhãn $1$. Phân phối thật $P$ chỉ đặt toàn bộ xác suất vào giá trị $y$ (tương tự $P(x_0)=1$ ở Chương 6.2, Mục 7.2), nên công thức tổng quát $H(P,Q)$ thu gọn thành:

> **📦 Formula Box — Binary Cross-Entropy Loss**
>
> $$\mathcal{L}(y, \hat{y}) = -\big[y\log_2 \hat{y} + (1-y)\log_2(1-\hat{y})\big]$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $y$ | Nhãn thật (0 hoặc 1) |
> | $\hat{y}$ | Xác suất mô hình dự đoán cho nhãn 1 |
> | **Diễn giải kỹ thuật** | Khi $y=1$, chỉ số hạng $-\log_2\hat{y}$ còn hiệu lực — mô hình bị phạt theo mức độ nó "không tự tin" vào đáp án đúng; tương tự khi $y=0$ |
> | **Lưu ý thực hành** | Trong tài liệu Machine Learning, hàm này thường dùng logarit tự nhiên ($\ln$, đơn vị nat) thay vì $\log_2$, vì đạo hàm của $\ln$ đơn giản hơn khi tính gradient cho backpropagation (Volume 5) |

Với bài toán phân loại đa lớp (multi-class classification, $K$ lớp), nhãn thật thường được biểu diễn dưới dạng **one-hot encoding** ($P$ đặt xác suất 1 vào đúng một lớp), và công thức tổng quát ở Mục 6 áp dụng trực tiếp:

$$\mathcal{L} = -\sum_{k=1}^{K} y_k \log \hat{y}_k$$

trong đó chỉ số hạng ứng với lớp đúng ($y_k=1$) còn đóng góp khác 0 — một hệ quả trực tiếp của việc $P$ là phân phối one-hot, tương tự cách $H(P,Q)$ thu gọn ở công thức binary phía trên.

---

## 9. Triển khai

```python
import math

def cross_entropy(p, q, base=2):
    """Tính Cross Entropy H(P,Q) = -sum(P(x) * log_b Q(x)).

    p, q: hai danh sách xác suất cùng độ dài, biểu diễn P và Q
          trên cùng tập kết quả x. Yêu cầu q[i] > 0 khi p[i] > 0.
    """
    total = 0.0
    for pi, qi in zip(p, q):
        if pi > 0:
            if qi <= 0:
                return float('inf')
            total -= pi * math.log(qi, base)
    return total


def binary_cross_entropy_loss(y, y_hat, eps=1e-12):
    """Binary Cross-Entropy Loss cho một mẫu dữ liệu, dùng log tự nhiên
    (đơn vị nat), theo chuẩn phổ biến trong Machine Learning.

    y: nhãn thật, 0 hoặc 1
    y_hat: xác suất dự đoán cho nhãn 1, trong khoảng (0, 1)
    eps: hằng số nhỏ để tránh log(0) khi y_hat quá gần 0 hoặc 1
    """
    y_hat = min(max(y_hat, eps), 1 - eps)  # giới hạn tránh log(0)
    return -(y * math.log(y_hat) + (1 - y) * math.log(1 - y_hat))
```

Hàm `cross_entropy` triển khai trực tiếp công thức tổng quát ở Mục 6. Hàm `binary_cross_entropy_loss` triển khai Formula Box ở Mục 8, đồng thời xử lý vấn đề thực hành quan trọng: khi $\hat{y}$ quá gần 0 hoặc 1, `log(0)` gây lỗi toán học — tham số `eps` (epsilon) là kỹ thuật phổ biến để "chặn" giá trị dự đoán trong một khoảng an toàn.

---

## 10. Trực quan hóa kết quả tính toán

**Kiểm chứng Bất đẳng thức Gibbs ($H(P,Q) \geq H(P)$)** — chạy trên **1000 cặp phân phối ngẫu nhiên** $(P, Q)$ khác nhau:

```text
Kiểm chứng H(P,Q) >= H(P) — đúng ở toàn bộ 1000 phép thử ngẫu nhiên: True
Số trường hợp đẳng thức xảy ra (Q trùng P): 0/1000 (như dự đoán, vì P, Q sinh ngẫu nhiên độc lập)
```

**Binary Cross-Entropy Loss cho các mức độ tự tin khác nhau**, với nhãn thật $y=1$ (đơn vị nat):

| $\hat{y}$ (dự đoán) | Loss | Diễn giải |
|---:|---:|---|
| 0.99 | 0.010 | Rất tự tin và **đúng** — loss gần 0 |
| 0.70 | 0.357 | Tự tin vừa phải, đúng hướng |
| 0.50 | 0.693 | Hoàn toàn không chắc chắn |
| 0.30 | 1.204 | Tự tin sai hướng vừa phải |
| 0.01 | 4.605 | Rất tự tin nhưng **sai hoàn toàn** — loss rất lớn |

Bảng này minh họa lại chính xác trực giác ở Hình 6.3.2: cross-entropy loss tăng **phi tuyến** (không tuyến tính) khi mô hình càng tự tin sai — một tính chất mong muốn cho việc huấn luyện, vì nó tạo ra gradient lớn để "sửa sai" mạnh mẽ cho những dự đoán tồi tệ nhất.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Cross Entropy không phải một công cụ lý thuyết thuần túy — nó là hàm mất mát đứng sau việc huấn luyện phần lớn các mô hình phân loại đang chạy trong sản xuất (production) ngày nay.

| Bối cảnh công nghiệp | Vai trò của Cross Entropy |
|---|---|
| Logistic Regression, Neural Network (bài toán phân loại) | Binary/Categorical Cross-Entropy Loss là lựa chọn mặc định để huấn luyện, tối ưu bằng Gradient Descent (Part VII) |
| Bộ lọc thư rác (spam filter), phát hiện gian lận (fraud detection) | Mô hình được huấn luyện để tối thiểu hóa cross entropy giữa nhãn thật (spam/không spam) và xác suất dự đoán |
| Đánh giá mô hình ngôn ngữ (Language Model) | **Perplexity**, độ đo tiêu chuẩn để so sánh chất lượng mô hình ngôn ngữ, được tính trực tiếp từ cross entropy trung bình trên tập dữ liệu kiểm thử |
| Hệ thống gợi ý (Recommendation System) | Bài toán dự đoán "người dùng có click hay không" thường được mô hình hóa như phân loại nhị phân, huấn luyện bằng cross-entropy loss |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Cross Entropy là điểm khởi đầu cho nhiều kỹ thuật huấn luyện tinh vi hơn trong Machine Learning hiện đại.

**Label Smoothing** — một kỹ thuật regularization phổ biến (sẽ gặp lại ở Volume 5–6): thay vì dùng nhãn one-hot "cứng" (xác suất 1 cho lớp đúng, 0 cho các lớp khác) làm phân phối $P$, ta "làm mềm" $P$ bằng cách phân bổ một phần xác suất nhỏ cho các lớp sai. Kỹ thuật này trực tiếp giải quyết vấn đề "tự tin quá mức" (overconfidence) đã thấy ở Mục 10 — vì $H(P,Q)$ chỉ đạt cực tiểu khi $Q=P$ (Mục 7.1), nếu $P$ không còn là one-hot tuyệt đối, mô hình cũng không còn bị khuyến khích dự đoán xác suất cực đoan gần 0 hoặc 1.

**Vấn đề Numerical Stability** — một chủ đề nghiên cứu ứng dụng quan trọng: khi $\hat{y}$ tiến gần 0 hoặc 1 (Mục 9, tham số `eps`), việc tính `log` trực tiếp có thể gây lỗi số học nghiêm trọng trong huấn luyện mô hình lớn. Các thư viện Deep Learning hiện đại (PyTorch, TensorFlow) thường kết hợp bước tính xác suất (thường dùng hàm Sigmoid hoặc Softmax) và bước tính cross-entropy loss thành **một phép toán duy nhất** (ví dụ: `BCEWithLogitsLoss` trong PyTorch) chính vì lý do ổn định số học này.

**Kết nối lý thuyết sâu hơn:** như đã nêu ở Mục 7.1, $H(P,Q) = H(P) + D_{KL}(P\parallel Q)$. Vì $H(P)$ không phụ thuộc vào mô hình $Q$ (nó chỉ phụ thuộc dữ liệu thật), việc **tối thiểu hóa cross entropy theo $Q$ hoàn toàn tương đương với việc tối thiểu hóa KL Divergence theo $Q$** — một góc nhìn sẽ được khai thác đầy đủ ở Chương 6.4.

---

## 13. Ưu điểm

- **Có nền tảng lý thuyết vững chắc:** tương đương với Maximum Likelihood Estimation (Mục 7.2), không phải một lựa chọn "cảm tính" giữa nhiều hàm mất mát khả dĩ.
- **Phạt phi tuyến, ưu tiên đúng mục tiêu huấn luyện:** phạt nặng các dự đoán tự tin-nhưng-sai (Mục 10), tạo gradient hữu ích cho quá trình tối ưu hóa (Part VII).
- **Tổng quát hóa tự nhiên** từ phân loại nhị phân sang đa lớp mà không cần thay đổi công thức gốc (Mục 8).
- **Luôn không nhỏ hơn entropy** (Mục 7.1) — đảm bảo có một "sàn" lý thuyết rõ ràng để đánh giá mức độ mô hình còn có thể cải thiện.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Nhầm lẫn phổ biến nhất: coi $H(P,Q)$ và $H(Q,P)$ là như nhau. Cross Entropy **không đối xứng** — vai trò của $P$ (trọng số trung bình) và $Q$ (bên trong logarit) hoàn toàn khác biệt.

- **Không đối xứng:** $H(P,Q) \neq H(Q,P)$ nói chung — khác với các độ đo "khoảng cách" quen thuộc (như khoảng cách Euclid), cross entropy không phải một metric theo nghĩa toán học chặt chẽ.
- **Không xác định (tiến tới $\infty$) khi $Q(x) = 0$ mà $P(x) > 0$:** về mặt thực hành, đây là lý do bắt buộc phải xử lý cẩn thận các giá trị xác suất cực nhỏ (tham số `eps` ở Mục 9).
- **Nhạy cảm với chất lượng ước lượng $P$:** nếu nhãn thật $P$ (ví dụ: nhãn dữ liệu do con người gán) bị lỗi hoặc nhiễu, cross entropy sẽ "dạy" mô hình học theo cả những lỗi đó — vấn đề dữ liệu nhiễu (noisy labels) là một thách thức thực hành lớn trong Machine Learning.

---

## 15. So sánh

**Bảng 6.3.1 — Entropy (6.2) và Cross Entropy (chương này)**

| Đặc điểm | Entropy $H(X)$ | Cross Entropy $H(P,Q)$ |
|---|---|---|
| Số phân phối liên quan | Một: $P$ | Hai: $P$ (thật) và $Q$ (ước lượng) |
| Công thức | $-\sum_x P(x)\log P(x)$ | $-\sum_x P(x)\log Q(x)$ |
| Tính đối xứng | Không áp dụng (chỉ một phân phối) | **Không đối xứng**: $H(P,Q)\neq H(Q,P)$ |
| Giá trị cực tiểu (theo $Q$) | Không áp dụng | Đạt tại $Q = P$, giá trị cực tiểu bằng $H(P)$ |
| Vai trò trong Machine Learning | Đo độ khó đoán của nhãn thật (ví dụ: Information Gain) | Trực tiếp là hàm mất mát (Cross-Entropy Loss) được tối ưu hóa khi huấn luyện |

**Phân tích:** bảng này cho thấy rõ cross entropy **tổng quát hóa** entropy theo đúng một hướng: cho phép "mô hình dùng để đo" ($Q$) khác với "nguồn dữ liệu thật" ($P$). Khi hai phân phối trùng nhau, hai đại lượng trùng nhau — đúng như đã chứng minh ở Mục 7.1. Đây là bước chuẩn bị trực tiếp cho Chương 6.4, nơi *hiệu số* $H(P,Q) - H(P)$ (đã xuất hiện trong chứng minh Mục 7.1) sẽ được đặt tên chính thức và nghiên cứu độc lập.

---

## 16. Tóm tắt

- **Cross Entropy** $H(P,Q) = -\sum_x P(x)\log_2 Q(x)$ đo chi phí trung bình khi dùng phân phối ước lượng $Q$ để "mã hóa" hoặc dự đoán các sự kiện thực chất tuân theo phân phối thật $P$.
- **Bất đẳng thức Gibbs**: $H(P,Q) \geq H(P)$, đẳng thức xảy ra khi và chỉ khi $Q = P$ — chứng minh bằng bất đẳng thức $\ln t \leq t-1$, một kỹ thuật sẽ tái sử dụng ở Chương 6.4.
- Trong Machine Learning, cross entropy thu gọn thành **Binary/Categorical Cross-Entropy Loss** — hàm mất mát tiêu chuẩn cho bài toán phân loại, phạt nặng các dự đoán tự tin-nhưng-sai.
- Tối thiểu hóa cross entropy theo $Q$ **tương đương về mặt toán học** với Maximum Likelihood Estimation — không phải một lựa chọn tùy tiện, mà có cơ sở lý thuyết xác suất vững chắc.
- Cross entropy **không đối xứng** — một điểm khác biệt căn bản so với khái niệm "khoảng cách" thông thường, sẽ được phân tích sâu hơn ở Chương 6.4.

Chương 6.4 sẽ tách riêng và đặt tên cho phần "vượt trội" $H(P,Q) - H(P)$ đã xuất hiện xuyên suốt chứng minh của chương này — đó chính là KL Divergence.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Cho $P = (0.5, 0.5)$ và $Q = (0.9, 0.1)$ trên hai kết quả. Tính $H(P,Q)$ và so sánh với $H(P)$ (đã tính ở Chương 6.2, Bài tập 2).
2. Tính Binary Cross-Entropy Loss (đơn vị nat) cho ba trường hợp: (a) $y=1, \hat{y}=0.9$; (b) $y=1, \hat{y}=0.5$; (c) $y=1, \hat{y}=0.1$. Sắp xếp theo thứ tự tăng dần và giải thích xu hướng.
3. Tính $H(Q,P)$ với $P, Q$ như Bài 1, và xác nhận $H(P,Q) \neq H(Q,P)$.

### Mức Trung bình (Intermediate)

4. Chứng minh trực tiếp rằng khi $Q = P$, công thức $H(P,Q)$ thu gọn chính xác về $H(P)$. *(Đây là bước khởi động đơn giản cho Bài tập 5.)*
5. Dùng lại chứng minh ở Mục 7.1 (không cần chép lại toàn bộ, chỉ cần áp dụng), hãy chứng minh rằng nếu $Q$ khác $P$ chỉ tại **một** giá trị $x_0$ duy nhất (các giá trị khác giữ nguyên, phân phối vẫn hợp lệ), thì $H(P,Q) > H(P)$ nghiêm ngặt (không chỉ $\geq$).

### Mức Nâng cao (Advanced)

6. Cài đặt hàm `binary_cross_entropy_loss` (Mục 9) và dùng nó để so sánh hai mô hình phân loại giả định trên cùng 5 mẫu dữ liệu: Mô hình A luôn dự đoán $\hat{y}=0.6$ cho nhãn dương và $\hat{y}=0.4$ cho nhãn âm (thận trọng); Mô hình B dự đoán rất tự tin ($\hat{y}=0.95$ hoặc $0.05$) nhưng sai ở đúng 1 trong 5 mẫu. Tính tổng loss của cả hai mô hình trên 5 mẫu và giải thích mô hình nào bị phạt nặng hơn — kết quả có luôn đúng với "trực giác" rằng ít lỗi hơn thì loss thấp hơn không?
7. **(Định hướng nghiên cứu)** Đọc phần mô tả về Label Smoothing ở Mục 12. Thử tự xây dựng công thức cho $P$ "làm mềm" trong bài toán 3 lớp (thay vì one-hot $(1,0,0)$, dùng $(1-\epsilon, \epsilon/2, \epsilon/2)$ với $\epsilon$ nhỏ), rồi tính lại $H(P,Q)$ cho một $Q$ cụ thể. So sánh với trường hợp $P$ one-hot thuần túy — mức phạt khi mô hình dự đoán cực đoan có giảm đi không?

---

## 18. Dự án nhỏ

### Xây dựng Bộ phân loại Nhị phân Tối giản bằng Cross-Entropy Loss

**Mục tiêu:** Củng cố hiểu biết về Cross Entropy bằng cách tự cài đặt toàn bộ vòng lặp tính loss cho một bài toán phân loại nhị phân đơn giản, không dùng thư viện Machine Learning có sẵn.

**Yêu cầu:**

1. Tạo một bộ dữ liệu giả lập gồm 20 mẫu, mỗi mẫu có một đặc trưng số (feature) duy nhất $x$ và nhãn nhị phân $y \in \{0,1\}$ (ví dụ: $y=1$ nếu $x > 5$, có thêm nhiễu ngẫu nhiên).
2. Dùng một mô hình cực kỳ đơn giản: $\hat{y} = \sigma(wx + b)$, với $\sigma$ là hàm Sigmoid ($\sigma(z) = \dfrac{1}{1+e^{-z}}$), và $w, b$ là hai tham số bạn tự chọn thử nghiệm bằng tay (chưa cần huấn luyện tối ưu — Gradient Descent sẽ học ở Part VII).
3. Tính **Binary Cross-Entropy Loss trung bình** trên toàn bộ 20 mẫu, dùng hàm `binary_cross_entropy_loss` đã cài đặt ở Mục 9.
4. Thử ít nhất 3 bộ giá trị $(w, b)$ khác nhau, so sánh loss trung bình, và xác định (bằng thử-sai thủ công) bộ giá trị nào cho loss thấp nhất.

**Công nghệ gợi ý:** Python thuần (`math`), không cần NumPy hay bất kỳ thư viện Machine Learning nào — mục tiêu là hiểu cơ chế, không phải hiệu năng.

**Kết quả kỳ vọng:** Một bảng so sánh loss trung bình giữa các bộ $(w,b)$ khác nhau, kèm nhận xét ngắn về mối liên hệ giữa "mô hình khớp dữ liệu tốt hơn" và "cross-entropy loss thấp hơn" — chuẩn bị trực tiếp cho khái niệm Gradient Descent ở Part VII, nơi bạn sẽ học cách **tự động** tìm $(w,b)$ tối ưu thay vì thử-sai thủ công.

**Hướng mở rộng (tùy chọn):** thử vẽ đồ thị loss trung bình theo từng giá trị $w$ (cố định $b$) để quan sát trực quan hình dạng "lòng chảo" (bowl-shaped) của hàm loss — hình dạng này chính là lý do Gradient Descent hoạt động hiệu quả trên hàm cross-entropy loss.

---

## 19. Tự đánh giá

- [ ] Tôi có thể giải thích bằng lời sự khác biệt giữa $P$ (thật) và $Q$ (ước lượng) trong công thức Cross Entropy, không nhầm lẫn vai trò của hai phân phối.
- [ ] Tôi có thể tính $H(P,Q)$ cho hai phân phối rời rạc cụ thể, và xác nhận $H(P,Q) \geq H(P)$.
- [ ] Tôi hiểu và có thể trình bày lại (không cần thuộc lòng từng bước) ý tưởng chính của chứng minh Bất đẳng thức Gibbs ở Mục 7.1.
- [ ] Tôi có thể tự tính Binary Cross-Entropy Loss cho một cặp $(y, \hat{y})$ cụ thể, và giải thích vì sao dự đoán tự tin-sai bị phạt nặng hơn nhiều so với dự đoán không chắc chắn.
- [ ] Tôi hoàn thành được Dự án nhỏ ở Mục 18, và quan sát được mối liên hệ giữa việc chọn $(w,b)$ tốt hơn và loss trung bình thấp hơn.

Nếu khái niệm "không đối xứng" ở Mục 14 vẫn còn mơ hồ, hãy thử tự tính cả $H(P,Q)$ và $H(Q,P)$ cho cùng một cặp phân phối (Bài tập 3) trước khi chuyển sang Chương 6.4 — KL Divergence sẽ định nghĩa chính xác "mức độ bất đối xứng" này bằng một công thức riêng.

---

## 20. Đọc thêm

- **Sách:** Christopher Bishop, *Pattern Recognition and Machine Learning* — chương về Maximum Likelihood Estimation và mối liên hệ với hàm mất mát cross-entropy. *(Xem BOOKS.md — Volume 5.)*
- **Sách:** Ian Goodfellow, Yoshua Bengio, Aaron Courville, *Deep Learning* — chương về hàm mất mát (loss functions) trong huấn luyện mạng neural. *(Xem BOOKS.md — Volume 5.)*
- **Paper liên quan:** Rumelhart, Hinton, Williams, *Learning Representations by Back-Propagating Errors* (1986) — đã dẫn ở Mục 2, ngữ cảnh lịch sử phổ biến hóa cross-entropy loss trong huấn luyện mạng neural.
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc về Label Smoothing (Mục 12) và các biến thể hiện đại của cross-entropy loss dùng trong các mô hình phân loại ảnh và ngôn ngữ lớn.
- **Chương tiếp theo:** Chương 6.4 — KL Divergence.

---

### Liên kết chương (Cross References)

- **Chương trước:** 6.2 — Entropy (Cross Entropy là phần mở rộng trực tiếp, thay $P$ thứ hai trong công thức bằng $Q$).
- **Chương tiếp theo:** 6.4 — KL Divergence (đặt tên chính thức và nghiên cứu sâu hiệu số $H(P,Q) - H(P)$ đã xuất hiện xuyên suốt chương này).
- **Chương liên quan xa hơn:** Volume 1, Part VII — Optimization for Artificial Intelligence (Gradient Descent, công cụ dùng để tối thiểu hóa cross-entropy loss trong thực hành, sẽ áp dụng trực tiếp lên Dự án nhỏ Mục 18); Volume 5, Part II — Machine Learning (Logistic Regression, Neural Network, và Maximum Likelihood Estimation trình bày đầy đủ).
- **Vị trí trong Knowledge Graph:** Nút thứ ba của Volume 1, Part VI, phụ thuộc trực tiếp vào Chương 6.2; là điều kiện tiên quyết bắt buộc cho Chương 6.4, và là chương đầu tiên của Part VI có ứng dụng thực hành trực tiếp dưới dạng Dự án nhỏ.

---

*Hết Chương 6.3. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`. Khác với Chương 6.1–6.2, chương này có Mini Project (Mục 18) vì đã đạt đến điểm có ứng dụng thực hành rõ ràng trong Machine Learning, đúng như đã ghi chú tại Mục 18 của hai chương trước. Bất đẳng thức Gibbs được chứng minh đầy đủ bằng công cụ đã có sẵn từ Part IV (Calculus), không vay mượn trước kết quả của Chương 6.4. Đang chờ rà soát trước khi tiếp tục sang Chương 6.4 — KL Divergence.*
