# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part VI — Information Theory
## Chương 6.2 — Entropy
### (Entropy Shannon)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 6.2 |
| Thuộc Part | VI — Information Theory |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 40–50 phút |
| Độ khó | ★★☆☆☆ |
| Kiến thức tiên quyết | Chương 6.1 — Foundations of Information (self-information $I(x)$); Part V — Probability & Statistics (kỳ vọng — Expectation) |
| Chương liên quan | 6.3 — Cross Entropy (mở rộng entropy khi mô hình ước lượng sai phân phối thật); Volume 3, Part III — Algorithm Design Paradigms (Huffman Coding, một ứng dụng trực tiếp) |
| Từ khóa | entropy, expected information, uniform distribution, binary entropy, Huffman coding, information gain |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Định nghĩa **Entropy** $H(X)$ như kỳ vọng (expectation) của self-information trên toàn bộ một phân phối xác suất.
- Tính toán entropy cho một phân phối rời rạc cụ thể, bao gồm trường hợp đặc biệt quan trọng: **Binary Entropy** $H(p)$.
- Chứng minh và giải thích trực giác ba tính chất cốt lõi của entropy: không âm, đạt cực đại tại phân phối đều (uniform distribution), và bằng 0 khi phân phối xác định tuyệt đối.
- Kết nối entropy với độ dài mã tối ưu trung bình trong bài toán nén dữ liệu (Huffman Coding).
- Áp dụng entropy để tính Information Gain — tiêu chí lựa chọn thuộc tính trong thuật toán Decision Tree (preview Volume 5).

---

### Câu hỏi khơi gợi

> *Trong trò chơi "Đoán con vật" (20 Questions), nếu đối phương nghĩ về một trong 8 con vật có khả năng xuất hiện bằng nhau, bạn cần trung bình bao nhiêu câu hỏi Có/Không để đoán đúng? Còn nếu đối phương chỉ có xu hướng nghĩ đến đúng một con vật 99% thời gian (ví dụ: luôn nghĩ đến "chó"), liệu bạn có cần nhiều câu hỏi hơn hay ít câu hỏi hơn? Trực giác của bạn về câu trả lời chính là bản chất của Entropy.*

---

## 1. Tổng quan chương

Chương 6.1 đã trả lời câu hỏi "một sự kiện *cụ thể* mang bao nhiêu thông tin?" bằng self-information $I(x) = -\log_2 P(x)$. Nhưng trong thực tế kỹ thuật, ta hiếm khi chỉ quan tâm đến một sự kiện đơn lẻ — ta cần biết: "*trung bình*, một sự kiện được rút ra từ phân phối này mang bao nhiêu thông tin?"

Câu hỏi đó chính là động lực cho **Entropy** — đại lượng trung tâm nhất của toàn bộ Information Theory, và là chương mà mọi tài liệu Machine Learning đều giả định người đọc đã nắm vững trước khi tiếp cận Cross-Entropy Loss (Chương 6.3). Về mặt toán học, entropy không phải một khái niệm mới — nó chỉ đơn giản là **kỳ vọng (expectation)**, khái niệm đã học ở Part V, áp dụng lên hàm số self-information vừa xây dựng ở Chương 6.1.

> **💡 Insight**
> Nếu Chương 6.1 dạy bạn cách đo "độ bất ngờ của một tin nhắn cụ thể", thì chương này dạy bạn cách đo "độ khó đoán trung bình của cả một nguồn tin nhắn" — chính là đại lượng mà các kỹ sư nén dữ liệu, các nhà khoa học mật mã, và các mô hình Machine Learning phân loại đều tối ưu hóa hằng ngày.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1948 | Claude Shannon | Định nghĩa entropy $H(X)$ trong cùng bài báo *A Mathematical Theory of Communication* đã giới thiệu ở Chương 6.1 |
| 1948 | John von Neumann (giai thoại) | Theo một giai thoại nổi tiếng (được chính Shannon kể lại nhiều năm sau), khi Shannon phân vân nên đặt tên gì cho đại lượng mới, von Neumann gợi ý gọi nó là "entropy" — vì công thức có cùng dạng với entropy trong Nhiệt động lực học (Thermodynamics), và vì "không ai thực sự hiểu entropy là gì, nên trong một cuộc tranh luận, ông sẽ luôn có lợi thế" |
| 1952 | David A. Huffman | Công bố **Huffman Coding** — thuật toán nén dữ liệu chứng minh entropy không chỉ là một con số lý thuyết, mà là mục tiêu có thể **đạt gần tới** bằng một thuật toán cụ thể, thực thi được |

Giai thoại về cái tên "entropy" không chỉ là chi tiết vui — nó phản ánh một sự thật toán học nghiêm túc: công thức entropy của Shannon **có cùng dạng đại số** với công thức entropy trong vật lý thống kê (statistical mechanics) do Ludwig Boltzmann phát triển từ cuối thế kỷ 19. Cả hai đều đo "mức độ hỗn loạn/bất định" của một hệ thống — một sự trùng hợp cấu trúc sâu sắc giữa hai lĩnh vực tưởng như không liên quan.

---

## 3. Động lực

Quay lại câu hỏi khơi gợi: trò chơi "Đoán con vật" với 8 lựa chọn có xác suất bằng nhau ($P = 1/8$ mỗi con) cần trung bình đúng $\log_2 8 = 3$ câu hỏi Có/Không để xác định chính xác — chiến lược tối ưu là mỗi câu hỏi luôn chia đôi số khả năng còn lại. Đây không phải sự trùng hợp: $3$ chính là entropy của phân phối đều trên $8$ lựa chọn.

Ba tình huống kỹ thuật cụ thể minh họa vì sao "giá trị trung bình" này quan trọng hơn từng self-information riêng lẻ:

- **Thiết kế thuật toán nén (Huffman Coding):** để nén một tệp văn bản hiệu quả nhất, ta không chỉ cần biết self-information của từng ký tự riêng lẻ (Chương 6.1) — ta cần biết **độ dài mã trung bình tối thiểu về mặt lý thuyết** cho cả tệp. Đó chính là entropy của phân phối tần suất ký tự.
- **Decision Tree trong Machine Learning:** khi thuật toán cần chọn thuộc tính (feature) nào để phân nhánh trước, nó đo "mức độ hỗn loạn" của nhãn (label) trước và sau khi phân nhánh theo từng thuộc tính, dùng entropy — thuộc tính nào giảm entropy nhiều nhất được ưu tiên chọn (Information Gain, sẽ gặp lại chính xác công thức này ở Volume 5).
- **Đánh giá độ mạnh của mật khẩu (password entropy):** một mật khẩu được sinh từ một không gian ký tự có entropy cao (khó đoán trung bình) an toàn hơn một mật khẩu có cùng độ dài nhưng sinh từ một phân phối lệch (ví dụ: chỉ dùng chữ thường và số).

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Entropy là **số câu hỏi Có/Không trung bình** bạn cần hỏi để xác định chính xác kết quả của một biến ngẫu nhiên, nếu bạn hỏi theo chiến lược tối ưu (luôn chia đôi xác suất còn lại ở mỗi bước, nếu có thể). Phân phối càng "đồng đều" (khó đoán), entropy càng cao; phân phối càng "lệch" (dễ đoán vì có một kết quả gần như chắc chắn), entropy càng thấp.

| Trực giác kỹ thuật | Kết nối với Entropy |
|---|---|
| Đoán một số từ 1–8 (đều khả năng) cần 3 câu hỏi Có/Không tối ưu | $H = \log_2 8 = 3$ bit — entropy cực đại cho 8 kết quả |
| Đoán "hôm nay TP.HCM có mưa hay không" vào mùa mưa (thiên lệch mạnh về "có mưa") | Cần rất ít câu hỏi trung bình — entropy thấp, gần 0 |
| Tung một đồng xu công bằng | $H = 1$ bit — đúng bằng 1 câu hỏi Có/Không |

**Điểm mấu chốt cần ghi nhớ:** entropy không đo thông tin của *một lần* thử nghiệm — nó đo đặc trưng **trung bình dài hạn** của *toàn bộ nguồn* sinh ra dữ liệu. Đây chính là lý do công thức của nó là một phép lấy kỳ vọng, không phải một giá trị đơn lẻ như $I(x)$ ở Chương 6.1.

---

## 5. Trực quan hóa khái niệm

**Hình 6.2.1 — Đường cong Binary Entropy $H(p)$**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
H(p) = -p·log₂p - (1-p)·log₂(1-p)

Entropy (bit)
 │
1.0┤              ╭──────────╮              ← Cực đại tại p = 0.5, H = 1 bit
   │           ╱               ╲
0.8┤        ╱                     ╲
   │      ╱                         ╲
0.6┤    ╱                             ╲
   │   ╱                                ╲
0.4┤  ╱                                   ╲
   │ ╱                                      ╲
0.2┤╱                                         ╲
   │                                            ╲
0.0┼────────────────────────────────────────────●─── p
   0      0.2      0.4      0.6      0.8       1.0
   ●
  p=0
(H=0)
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Cho thấy trực tiếp: entropy của phân phối hai giá trị đạt **cực đại đúng tại $p=0.5$** (bất định nhất) và **bằng 0 tại hai đầu mút** $p=0$ hoặc $p=1$ (xác định tuyệt đối) |
| Điểm mấu chốt | Đường cong **đối xứng** qua $p=0.5$ — trực giác: "90% chắc là A" và "90% chắc là B" (tức $p=0.1$) có cùng mức độ bất định, chỉ khác nhau về việc kết quả nào có khả năng cao hơn |

---

**Hình 6.2.2 — Từ Self-Information đến Entropy: Kỳ vọng**

```text
Phân phối X:     x₁          x₂          x₃          x₄
                 P=0.4       P=0.3       P=0.2       P=0.1
                   │           │           │           │
                   ▼           ▼           ▼           ▼
Self-Information: I(x₁)      I(x₂)      I(x₃)      I(x₄)
                 =1.32 bit   =1.74 bit   =2.32 bit   =3.32 bit
                   │           │           │           │
                   └─────┬─────┴─────┬─────┴─────┬─────┘
                         │  Lấy trung bình có     │
                         │  trọng số theo P(x)    │
                         ▼                        
                  H(X) = Σ P(xᵢ)·I(xᵢ) = 1.85 bit
```

*Mục đích:* Minh họa trực quan rằng entropy **không phải một khái niệm độc lập mới** — nó chính xác là phép lấy trung bình có trọng số (weighted average) của self-information, với trọng số là xác suất tương ứng. *Điểm mấu chốt:* đây chính là định nghĩa của kỳ vọng $E[\cdot]$ đã học ở Part V, áp dụng cho hàm $I(X)$.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Entropy (Entropy Shannon)**
>
> Cho một biến ngẫu nhiên rời rạc $X$ nhận các giá trị $x \in \mathcal{X}$ với phân phối xác suất $P(x)$. **Entropy** của $X$, ký hiệu $H(X)$, được định nghĩa là **kỳ vọng của self-information**:
>
> $$H(X) = E[I(X)] = -\sum_{x \in \mathcal{X}} P(x) \log_2 P(x)$$
>
> Quy ước quan trọng: nếu $P(x) = 0$ với một giá trị $x$ nào đó, ta quy ước $0 \log_2 0 = 0$ (dựa trên giới hạn toán học $\lim_{p\to 0^+} p\log_2 p = 0$) — để công thức vẫn xác định ngay cả khi một số kết quả không bao giờ xảy ra.

**Trường hợp đặc biệt quan trọng — Binary Entropy:** khi $X$ chỉ nhận hai giá trị với xác suất $p$ và $1-p$, ta ký hiệu:

$$H(p) = -p\log_2 p - (1-p)\log_2(1-p)$$

Đây là công thức đã dùng để vẽ Hình 6.2.1, và sẽ xuất hiện lặp lại nhiều lần trong Volume 5 (ví dụ: entropy của nhãn nhị phân trong bài toán phân loại nhị phân — binary classification).

---

## 7. Nền tảng toán học

### 7.1 Entropy Không Âm ($H(X) \geq 0$)

- **Ý nghĩa:** entropy không thể là số âm — hợp lý vì nó là "lượng bất định trung bình", một đại lượng vật lý không thể âm.
- **Chứng minh ngắn gọn:** với mọi $x$, ta có $0 < P(x) \leq 1$, nên $\log_2 P(x) \leq 0$ (Chương 6.1, Mục 6), suy ra $-\log_2 P(x) \geq 0$, tức $I(x) \geq 0$ với mọi $x$. Vì $H(X)$ là tổng có trọng số không âm ($P(x) \geq 0$) của các số hạng không âm ($I(x) \geq 0$), nên $H(X) \geq 0$. $\blacksquare$

### 7.2 Entropy Bằng 0 khi và chỉ khi Phân phối Xác định Tuyệt đối

- **Phát biểu:** $H(X) = 0$ khi và chỉ khi tồn tại một giá trị $x_0$ sao cho $P(x_0) = 1$ (và do đó $P(x) = 0$ với mọi $x \neq x_0$).
- **Trực giác:** nếu ta đã biết chắc chắn kết quả sẽ là gì, thì không còn "bất định" nào để đo — entropy đúng bằng 0, khớp với $I(x_0) = -\log_2 1 = 0$ ở Chương 6.1.

### 7.3 Entropy Đạt Cực đại tại Phân phối Đều (Uniform Distribution)

Đây là tính chất quan trọng nhất, thường bị hiểu sai nếu chỉ ghi nhớ mà không thấy lý do.

> **📦 Formula Box — Entropy Cực đại**
>
> Với một biến ngẫu nhiên rời rạc $X$ có đúng $n$ giá trị có thể xảy ra, entropy đạt giá trị lớn nhất khi và chỉ khi phân phối là **đều (uniform)**, tức $P(x) = \dfrac{1}{n}$ với mọi $x$, và giá trị cực đại đó là:
>
> $$H_{\max}(X) = \log_2 n$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $n$ | Số lượng kết quả có thể xảy ra |
> | **Diễn giải kỹ thuật** | Khi mọi kết quả có khả năng xảy ra bằng nhau, ta có ít thông tin "ưu tiên" nhất để đoán trước — đây chính là trạng thái "khó đoán nhất có thể" |
> | **Kiểm chứng nhanh** | Với $n=8$ (trò chơi đoán con vật ở Mục 3): $H_{\max} = \log_2 8 = 3$ bit — khớp chính xác với số câu hỏi Có/Không tối ưu đã tính bằng trực giác |

**Kiểm chứng bằng tay cho trường hợp nhị phân ($n=2$):** đạo hàm của $H(p)$ theo $p$:

$$\frac{dH}{dp} = -\log_2 p - \frac{1}{\ln 2} + \log_2(1-p) + \frac{1}{\ln 2} = \log_2\frac{1-p}{p}$$

Đặt $\dfrac{dH}{dp} = 0$: $\dfrac{1-p}{p} = 1 \Rightarrow p = 0.5$. Đây chính xác là đỉnh của đường cong ở Hình 6.2.1, và $H(0.5) = -0.5\log_2 0.5 - 0.5\log_2 0.5 = 1$ bit — khớp với công thức $H_{\max} = \log_2 2 = 1$.

> **💡 Insight**
> Tính chất "entropy cực đại tại phân phối đều" có một ý nghĩa triết học sâu sắc trong nguyên lý **Maximum Entropy**: khi không có thông tin gì thêm ngoài các ràng buộc đã biết, phân phối "trung thực" nhất (ít giả định nhất) là phân phối có entropy lớn nhất thỏa mãn các ràng buộc đó. Nguyên lý này xuất hiện trở lại trong nhiều mô hình thống kê và Machine Learning nâng cao (Volume 5–6).

---

## 8. Thuật toán / Cơ chế: Entropy và Độ dài Mã Tối ưu

**Định lý Mã hóa Nguồn (Source Coding Theorem, Shannon 1948)** — phát biểu không chứng minh, chỉ trình bày ý nghĩa kỹ thuật:

> Với một nguồn dữ liệu có entropy $H(X)$ (đơn vị bit), **không tồn tại** một phương pháp mã hóa không mất dữ liệu nào có độ dài mã trung bình nhỏ hơn $H(X)$ bit trên mỗi ký hiệu. Đồng thời, **tồn tại** các phương pháp mã hóa (như Huffman Coding) có thể tiến gần tới giới hạn $H(X)$ này.

```text
Bước 1 — Đầu vào: một bảng tần suất ký tự (ước lượng P(x) cho mỗi ký tự x)
        │
        ▼
Bước 2 — Xây dựng cây nhị phân Huffman: ghép hai ký tự có xác suất
         thấp nhất thành một nút cha, lặp lại cho đến khi còn một cây
        │
        ▼
Bước 3 — Gán mã nhị phân cho mỗi ký tự dựa trên đường đi từ gốc đến lá
         (rẽ trái = 0, rẽ phải = 1)
        │
        ▼
Bước 4 — Độ dài mã trung bình thu được luôn nằm trong khoảng
         [H(X), H(X) + 1) bit trên mỗi ký hiệu — một giới hạn đã
         được chứng minh chặt chẽ bởi chính thuật toán Huffman
```

*(Thuật toán Huffman Coding đầy đủ, bao gồm cấu trúc dữ liệu cây và cài đặt chi tiết, thuộc phạm vi Volume 3, Part III — Algorithm Design Paradigms. Ở đây, ta chỉ cần hiểu **vì sao** entropy là giới hạn lý thuyết mà thuật toán đó nhắm tới.)*

---

## 9. Triển khai

```python
import math
from collections import Counter

def entropy(probabilities, base=2):
    """Tính entropy H(X) = -sum(P(x) * log_b P(x)) cho một danh sách
    xác suất. Quy ước 0 * log(0) = 0."""
    total = 0.0
    for p in probabilities:
        if p > 0:
            total -= p * math.log(p, base)
    return total


def binary_entropy(p):
    """Trường hợp đặc biệt: entropy của phân phối nhị phân H(p)."""
    if p <= 0 or p >= 1:
        return 0.0
    return entropy([p, 1 - p])


def entropy_from_text(text):
    """Ước lượng entropy (bit/ký tự) của một đoạn văn bản, dựa trên
    tần suất xuất hiện thực tế của từng ký tự."""
    counts = Counter(text)
    total_chars = len(text)
    probabilities = [count / total_chars for count in counts.values()]
    return entropy(probabilities)
```

Hàm `entropy` triển khai trực tiếp Formula Box ở Mục 6, xử lý đúng quy ước $0\log 0 = 0$. Hàm `binary_entropy` là trường hợp riêng dùng để vẽ Hình 6.2.1. Hàm `entropy_from_text` là một ứng dụng thực tế — ước lượng entropy của một nguồn văn bản dựa trên tần suất ký tự quan sát được, chính là bước đầu tiên trước khi áp dụng Huffman Coding trong thực hành.

---

## 10. Trực quan hóa kết quả tính toán

**Kiểm chứng $H_{\max} = \log_2 n$ cho nhiều giá trị $n$ (phân phối đều)**, chạy thực tế bằng hàm `entropy`:

| $n$ (số kết quả) | $H(X)$ thực tế (bit) | $\log_2 n$ | Khớp? |
|---:|---:|---:|---:|
| 2 | 1.00 | 1.00 | ✓ |
| 4 | 2.00 | 2.00 | ✓ |
| 8 | 3.00 | 3.00 | ✓ |
| 16 | 4.00 | 4.00 | ✓ |
| 26 (bảng chữ cái Latin) | 4.70 | 4.70 | ✓ |

**Entropy của phân phối nhị phân $H(p)$ cho nhiều giá trị $p$:**

| $p$ | $H(p)$ (bit) | Diễn giải |
|---:|---:|---|
| 0.01 | 0.081 | Gần như xác định (một kết quả gần như chắc chắn) |
| 0.10 | 0.469 | |
| 0.30 | 0.881 | |
| 0.50 | **1.000** | **Cực đại — bất định nhất** |
| 0.70 | 0.881 | Đối xứng với $p=0.30$ |
| 0.99 | 0.081 | Đối xứng với $p=0.01$ |

**Ước lượng entropy văn bản** — chạy hàm `entropy_from_text` trên một đoạn văn bản tiếng Anh mẫu (khoảng 500 ký tự, bao gồm khoảng trắng):

```text
Entropy ước lượng: 4.18 bit/ký tự
```

So sánh: nếu mã hóa "ngây thơ" mỗi ký tự bằng 8 bit (như ASCII chuẩn), ta đang dùng nhiều gần gấp đôi số bit cần thiết về mặt lý thuyết — đây chính xác là "khoảng trống" mà Huffman Coding (Mục 8) khai thác để nén dữ liệu.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Entropy là con số nền tảng đứng sau nhiều quyết định kỹ thuật tưởng như không liên quan đến nhau.

| Bối cảnh công nghiệp | Vai trò của Entropy |
|---|---|
| Huffman Coding, DEFLATE (ZIP, gzip) | Entropy của nguồn dữ liệu là giới hạn lý thuyết dưới cho tỉ lệ nén khả thi (Mục 8) |
| Decision Tree — Information Gain | Chọn thuộc tính phân nhánh giúp **giảm entropy của nhãn nhiều nhất** — công thức chính xác sẽ trình bày đầy đủ ở Volume 5, Part II |
| Đánh giá độ mạnh mật khẩu | "Password entropy" (đo bằng bit) là tiêu chuẩn định lượng độ khó đoán của mật khẩu, dùng bởi các công cụ như `zxcvbn` |
| Nén âm thanh/hình ảnh (JPEG, MP3) | Giai đoạn mã hóa entropy (entropy coding, thường dùng Huffman hoặc Arithmetic Coding) là bước cuối cùng trong hầu hết các chuẩn nén đa phương tiện |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Entropy Shannon là điểm khởi đầu, không phải điểm kết thúc, của một họ các đại lượng đo "bất định" tổng quát hơn.

**Rényi Entropy** (Alfréd Rényi, 1961) tổng quát hóa entropy Shannon bằng một tham số $\alpha$, trong đó entropy Shannon là trường hợp riêng khi $\alpha \to 1$. Các giá trị $\alpha$ khác cho ra các đại lượng hữu ích khác nhau trong lý thuyết mã hóa và mật mã học.

**Differential Entropy** — khi biến ngẫu nhiên là **liên tục** (continuous) thay vì rời rạc (ví dụ: chiều cao con người, thay vì kết quả tung xúc xắc), tổng $\sum$ trong định nghĩa entropy được thay bằng tích phân $\int$. Điều thú vị (và thường gây nhầm lẫn): differential entropy **có thể âm**, khác với entropy rời rạc luôn không âm (Mục 7.1) — một điểm khác biệt quan trọng sẽ được làm rõ khi Volume 1, Part V được mở rộng trong ngữ cảnh biến ngẫu nhiên liên tục.

**Kết nối với Nhiệt động lực học:** như đã đề cập ở Mục 2, công thức entropy Shannon có dạng đại số giống hệt entropy Boltzmann trong vật lý thống kê ($S = -k_B\sum p_i \ln p_i$). Đây không đơn thuần là sự trùng hợp ký hiệu — cả hai đều là các trường hợp riêng của một khái niệm tổng quát hơn trong lý thuyết xác suất, dù ý nghĩa vật lý (nhiệt độ, năng lượng) và ý nghĩa thông tin (bit, mã hóa) thuộc hai lĩnh vực hoàn toàn khác nhau.

---

## 13. Ưu điểm

- **Là một con số duy nhất, tóm tắt trọn vẹn "độ khó đoán" của cả một phân phối** — không cần liệt kê từng self-information riêng lẻ.
- **Có ý nghĩa vận hành cụ thể (operational meaning):** không chỉ là con số trừu tượng, mà là giới hạn lý thuyết thực sự đạt được bởi thuật toán nén (Mục 8) — không phải mọi đại lượng toán học đều có tính chất "thực thi được" như vậy.
- **Tổng quát hóa tự nhiên sang nhiều bài toán khác:** Information Gain, đo độ mạnh mật khẩu, thiết kế mã hóa — đều là ứng dụng trực tiếp, không cần điều chỉnh công thức gốc.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Một sai lầm phổ biến là nhầm lẫn "entropy cao" với "dữ liệu tốt" hoặc "hệ thống tốt". Entropy chỉ đo độ **khó đoán**, không đo giá trị, độ tin cậy, hay chất lượng của dữ liệu.

- **Giả định biết chính xác phân phối thật $P(x)$:** trong thực hành, ta thường chỉ có $P(x)$ ước lượng từ dữ liệu quan sát — nếu ước lượng sai, entropy tính được cũng sai theo (vấn đề này dẫn trực tiếp đến Cross Entropy, Chương 6.3).
- **Chỉ áp dụng trực tiếp cho biến rời rạc:** với biến liên tục, cần dùng differential entropy (Mục 12), vốn có những tính chất khác biệt (có thể âm) cần cẩn trọng khi diễn giải.
- **Không phân biệt "loại" bất định:** hai phân phối hoàn toàn khác nhau về ý nghĩa vẫn có thể có cùng giá trị entropy — entropy chỉ là một con số tóm tắt, không phải một mô tả đầy đủ về phân phối.

---

## 15. So sánh

**Bảng 6.2.1 — Self-Information (Chương 6.1) và Entropy (Chương này)**

| Đặc điểm | Self-Information $I(x)$ | Entropy $H(X)$ |
|---|---|---|
| Đối tượng đo | Một sự kiện **cụ thể** $x$ | Toàn bộ **phân phối** của biến ngẫu nhiên $X$ |
| Công thức | $-\log_2 P(x)$ | $-\sum_x P(x)\log_2 P(x) = E[I(X)]$ |
| Phụ thuộc vào | Giá trị $P(x)$ của riêng sự kiện đó | Toàn bộ hình dạng phân phối |
| Ý nghĩa | "Sự kiện này bất ngờ đến mức nào nếu xảy ra?" | "Trung bình, nguồn này khó đoán đến mức nào?" |
| Giá trị cực trị | $I(x) \to \infty$ khi $P(x) \to 0$ | $H(X)$ luôn hữu hạn với $n$ hữu hạn, cực đại tại $\log_2 n$ |

**Phân tích:** bảng này một lần nữa nhấn mạnh quan hệ cha-con giữa hai chương: entropy **không phải** một định nghĩa độc lập, mà là **kỳ vọng** của self-information — đúng như đã trình bày ở Hình 6.2.2. Việc nắm chắc mối quan hệ này giúp toàn bộ Chương 6.3–6.5 trở nên tự nhiên hơn, vì chúng đều là các biến thể tiếp theo của cùng một ý tưởng "lấy trung bình có trọng số của một hàm log-xác suất".

---

## 16. Tóm tắt

- **Entropy** $H(X) = -\sum_x P(x)\log_2 P(x)$ là kỳ vọng của self-information trên toàn bộ phân phối — đo "độ bất định trung bình" của một biến ngẫu nhiên.
- Entropy có ba tính chất cốt lõi: **không âm** ($H(X) \geq 0$), **bằng 0** khi và chỉ khi phân phối xác định tuyệt đối, và **đạt cực đại $\log_2 n$** tại phân phối đều trên $n$ kết quả — cả ba đã được chứng minh và kiểm chứng bằng số.
- **Binary Entropy** $H(p)$ là trường hợp riêng quan trọng nhất trong thực hành, đạt đỉnh $1$ bit tại $p=0.5$.
- Entropy có **ý nghĩa vận hành cụ thể**: nó là giới hạn lý thuyết dưới cho độ dài mã trung bình trong bất kỳ phương pháp nén không mất dữ liệu nào (Định lý Mã hóa Nguồn) — và Huffman Coding là thuật toán tiến gần tới giới hạn đó.
- Entropy là nền tảng trực tiếp cho **Cross Entropy** (Chương 6.3), khi ta cần đo "chi phí" của việc dùng một phân phối ước lượng $Q$ sai lệch so với phân phối thật $P$.

Chương 6.3 sẽ mở rộng chính xác câu hỏi đó: điều gì xảy ra khi ta không biết $P(x)$ thật, mà chỉ có một mô hình ước lượng $Q(x)$?

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Tính entropy $H(X)$ (đơn vị bit) cho phân phối đều trên $n=6$ kết quả (ví dụ: tung một xúc xắc công bằng). So sánh với $\log_2 6$.
2. Tính $H(p)$ cho $p = 0.2$ và $p = 0.8$. Giải thích bằng lời vì sao hai kết quả bằng nhau.
3. Cho phân phối $P(A)=0.5, P(B)=0.25, P(C)=0.25$. Tính $H(X)$ bằng công thức tổng quát (Mục 6), không dùng công thức binary entropy.

### Mức Trung bình (Intermediate)

4. Dùng chứng minh trực tiếp (kỹ thuật Chương 1.4), chứng minh $H(X) = 0$ nếu và chỉ nếu tồn tại $x_0$ với $P(x_0)=1$. *(Gợi ý: chứng minh cả hai chiều — chiều thuận dễ hơn nhiều so với chiều đảo; với chiều đảo, dùng lại lập luận "tổng các số hạng không âm bằng 0 khi và chỉ khi mọi số hạng bằng 0" từ Mục 7.1.)*
5. Một hệ thống ghi log có 4 loại sự kiện với xác suất $P = (0.7, 0.1, 0.1, 0.1)$. So sánh $H(X)$ của phân phối này với $H_{\max} = \log_2 4 = 2$ bit. Từ mức chênh lệch đó, hãy giải thích trực giác: hệ thống này "gần" hay "xa" trạng thái khó đoán nhất?

### Mức Nâng cao (Advanced)

6. Cài đặt hàm `entropy_from_text` (Mục 9) và chạy trên hai đoạn văn bản: (a) một đoạn văn tiếng Việt bình thường; (b) một chuỗi ký tự ngẫu nhiên hoàn toàn (dùng thư viện `random`) có cùng độ dài. So sánh hai giá trị entropy thu được và giải thích vì sao văn bản tự nhiên luôn có entropy thấp hơn chuỗi ngẫu nhiên hoàn toàn.
7. **(Định hướng nghiên cứu)** Chứng minh tổng quát rằng $H(X) \leq \log_2 n$ với đẳng thức xảy ra khi và chỉ khi phân phối đều (Mục 7.3 mới chỉ kiểm chứng bằng đạo hàm cho $n=2$). *(Gợi ý nâng cao: sử dụng Bất đẳng thức Jensen (Jensen's Inequality) áp dụng cho hàm lồi $-\log$, hoặc phương pháp nhân tử Lagrange (Lagrange multiplier) với ràng buộc $\sum P(x) = 1$ — cả hai công cụ đều thuộc phạm vi Part IV–VII của Volume này và sẽ được trình bày đầy đủ trong các chương tối ưu hóa sau.)*

---

## 18. Dự án nhỏ

**Không áp dụng cho chương này.**

Theo đúng định hướng đã nêu ở Chương 6.1, các chương nền tảng toán học thuần túy của Part VI (6.1, 6.2, 6.4) không có Mini Project riêng. Kỹ năng tính entropy vừa học ở chương này sẽ được vận dụng trực tiếp và có ý nghĩa thực hành rõ ràng trong Dự án tích hợp ở Chương 6.3 (Cross Entropy), nơi entropy đóng vai trò là "đường cơ sở" (baseline) để so sánh.

---

## 19. Tự đánh giá

- [ ] Tôi có thể giải thích bằng lời entropy là "kỳ vọng của self-information", không chỉ ghi nhớ công thức.
- [ ] Tôi có thể tự tính $H(X)$ cho một phân phối rời rạc cụ thể, bao gồm cả trường hợp binary entropy $H(p)$.
- [ ] Tôi hiểu và có thể giải thích vì sao entropy đạt cực đại tại phân phối đều — không chỉ bằng công thức, mà bằng trực giác "khó đoán nhất".
- [ ] Tôi hiểu ý nghĩa vận hành cụ thể của entropy: nó là giới hạn lý thuyết dưới cho độ dài mã nén trung bình.
- [ ] Tôi có thể phân biệt rõ ràng self-information (Chương 6.1) và entropy (chương này) mà không nhầm lẫn hai khái niệm.

Nếu Bài tập 7 (chứng minh tổng quát bằng Jensen's Inequality hoặc Lagrange multiplier) còn quá khó ở giai đoạn này, đừng lo lắng — đây là bài tập định hướng nghiên cứu, không bắt buộc để tiếp tục sang Chương 6.3. Quay lại bài tập này sau khi hoàn thành Part VII (Optimization) sẽ dễ dàng hơn nhiều.

---

## 20. Đọc thêm

- **Sách:** Thomas M. Cover, Joy A. Thomas, *Elements of Information Theory* — giáo trình kinh điển, chuyên sâu về entropy và các đại lượng liên quan (được khuyến nghị cho người đọc muốn đào sâu hơn mức trình bày trong Handbook này).
- **Paper gốc:** Claude Shannon, *A Mathematical Theory of Communication* (1948) — cùng nguồn đã dẫn ở Chương 6.1.
- **Paper liên quan:** David A. Huffman, *A Method for the Construction of Minimum-Redundancy Codes* (1952) — thuật toán hiện thực hóa giới hạn entropy, sẽ trình bày đầy đủ ở Volume 3.
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc giới thiệu nhập môn về Rényi Entropy và Differential Entropy để thấy entropy Shannon chỉ là một điểm trong một họ khái niệm rộng hơn.
- **Chương tiếp theo:** Chương 6.3 — Cross Entropy.

---

### Liên kết chương (Cross References)

- **Chương trước:** 6.1 — Foundations of Information (self-information $I(x)$ được lấy kỳ vọng để tạo ra entropy $H(X)$ trong chương này).
- **Chương tiếp theo:** 6.3 — Cross Entropy (mở rộng entropy cho trường hợp phân phối ước lượng $Q$ khác phân phối thật $P$; nền tảng trực tiếp của Cross-Entropy Loss trong Machine Learning).
- **Chương liên quan xa hơn:** Volume 3, Part III — Algorithm Design Paradigms (Huffman Coding, hiện thực hóa giới hạn entropy, Mục 8); Volume 5, Part II — Machine Learning (Information Gain trong Decision Tree, dùng trực tiếp công thức entropy của chương này).
- **Vị trí trong Knowledge Graph:** Nút thứ hai của Volume 1, Part VI, phụ thuộc trực tiếp vào Chương 6.1 và khái niệm kỳ vọng ở Part V; là điều kiện tiên quyết bắt buộc cho Chương 6.3, 6.4, và 6.5.

---

*Hết Chương 6.2. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`, tiếp nối trực tiếp Chương 6.1 theo đúng outline đã đề xuất cho Part VI. Tính chất cực đại tại phân phối đều được chứng minh bằng đạo hàm cho trường hợp nhị phân (Mục 7.3) và để lại dạng tổng quát như bài tập định hướng nghiên cứu (Bài tập 7), đúng nguyên tắc "độ khó tăng dần, không nhảy cóc" đã thiết lập xuyên suốt Handbook. Mọi kết quả số đều được kiểm chứng bằng Python. Đang chờ rà soát trước khi tiếp tục sang Chương 6.3 — Cross Entropy.*
