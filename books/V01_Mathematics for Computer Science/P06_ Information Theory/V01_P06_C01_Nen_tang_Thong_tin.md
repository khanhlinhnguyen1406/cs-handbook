# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part VI — Information Theory
## Chương 6.1 — Nền tảng của Thông tin: Đo lường sự Bất định
### (Foundations of Information: Measuring Uncertainty)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 6.1 |
| Thuộc Part | VI — Information Theory |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 35–45 phút |
| Độ khó | ★★☆☆☆ |
| Kiến thức tiên quyết | Part V — Probability & Statistics (phân phối xác suất, biến ngẫu nhiên); Part IV — Calculus (hàm logarit, tính đơn điệu) |
| Chương liên quan | 6.2 — Entropy (chương này định nghĩa self-information, chương sau lấy kỳ vọng của nó); Volume 5, Part II — Machine Learning (self-information là viên gạch đầu tiên dẫn đến Cross-Entropy Loss) |
| Từ khóa | information, self-information, surprise, bit, nat, Shannon, logarithm, entropy (preview) |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Giải thích bằng trực giác vì sao "lượng thông tin" của một sự kiện phải liên quan nghịch đảo với xác suất xảy ra của nó.
- Định nghĩa và tính toán **Self-Information (Lượng tin riêng)** $I(x) = -\log_2 P(x)$ cho một sự kiện cụ thể.
- Phân biệt và chuyển đổi qua lại giữa hai đơn vị đo thông tin: **bit** (log cơ số 2) và **nat** (log tự nhiên).
- Chứng minh và áp dụng tính chất cộng tính (additivity) của self-information đối với các sự kiện độc lập.
- Kết nối khái niệm self-information với bài toán mã hóa dữ liệu (data encoding) trong kỹ thuật phần mềm.

---

### Câu hỏi khơi gợi

> *Vì sao một thông báo đẩy (push notification) như "Có động đất tại Thành phố Hồ Chí Minh" khiến bạn giật mình mở điện thoại ngay lập tức, trong khi một thông báo như "Hôm nay trời không mưa ở Thành phố Hồ Chí Minh vào mùa khô" gần như không khiến bạn quan tâm — dù cả hai đều là những câu "thông báo tin tức" có cùng độ dài? Điều gì, về mặt toán học, khiến hai câu nói có "lượng thông tin" khác nhau đến vậy?*

---

## 1. Tổng quan chương

Part V đã trang bị cho bạn ngôn ngữ để mô tả sự bất định: phân phối xác suất, biến ngẫu nhiên, kỳ vọng. Nhưng xác suất tự nó chỉ trả lời câu hỏi "khả năng xảy ra bao nhiêu?" — nó chưa trả lời một câu hỏi khác, tưởng gần gũi nhưng thực chất tách biệt: "nếu sự kiện đó *thực sự xảy ra*, thì việc biết được nó mang lại cho ta **bao nhiêu thông tin**?"

Chương này mở đầu Part VI — Information Theory bằng cách hình thức hóa chính xác câu hỏi đó. Chúng ta sẽ xây dựng khái niệm **Self-Information (Lượng tin riêng)** — con số đo "lượng bất ngờ" gắn với một sự kiện đơn lẻ. Đây là viên gạch nền tảng: mọi khái niệm còn lại của Part VI — Entropy (6.2), Cross Entropy (6.3), KL Divergence (6.4), Mutual Information (6.5) — đều là các phép biến đổi hoặc kỳ vọng được xây dựng trực tiếp trên self-information.

> **💡 Insight**
> Nếu bạn từng thắc mắc tại sao các thuật toán nén dữ liệu (data compression) như ZIP có thể nén văn bản tiếng Anh nhiều hơn một chuỗi số ngẫu nhiên, hoặc tại sao mô hình Machine Learning "phạt" nặng hơn khi dự đoán sai một sự kiện hiếm — câu trả lời cho cả hai bắt nguồn từ đúng một công thức bạn sẽ học trong chương này.

---

## 2. Bối cảnh lịch sử

Khái niệm định lượng "thông tin" gắn liền gần như tuyệt đối với một cái tên duy nhất.

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1948 | Claude Shannon | Công bố *A Mathematical Theory of Communication* — đặt nền móng cho toàn bộ Information Theory hiện đại, bao gồm định nghĩa self-information và entropy |
| 1948 | Bell Labs | Bối cảnh ra đời: Shannon làm việc tại Bell Labs, nghiên cứu bài toán truyền tín hiệu điện thoại đáng tin cậy qua đường dây nhiễu (noisy channel) — một bài toán kỹ thuật thuần túy, không phải triết học |
| Thập niên 1950–nay | Cộng đồng nghiên cứu Coding Theory | Mở rộng công trình của Shannon thành lý thuyết mã hóa sửa lỗi (error-correcting codes), nền tảng của mọi hệ thống truyền thông số hiện đại — Wi-Fi, viễn thông vệ tinh, lưu trữ dữ liệu |

Điều đáng chú ý là động cơ ban đầu của Shannon hoàn toàn mang tính kỹ thuật: ông cần một cách đo lường khách quan "dung lượng" tối đa của một đường truyền, không phụ thuộc vào ý nghĩa ngữ nghĩa của thông điệp. Chính vì loại bỏ hoàn toàn yếu tố ngữ nghĩa, định nghĩa của Shannon mới trở nên tổng quát đến mức áp dụng được cho mọi thứ — từ tín hiệu điện thoại, văn bản, cho đến ngày nay là nhãn phân loại (label) trong một mạng neural.

---

## 3. Động lực

Xét ba tình huống kỹ thuật quen thuộc, đều xoay quanh cùng một câu hỏi: "thông báo nào đáng giá hơn?"

- **Hệ thống giám sát an ninh mạng (cybersecurity monitoring):** một sự kiện đăng nhập thành công từ một địa chỉ IP quen thuộc, vào giờ làm việc bình thường, hầu như không đáng ghi log chi tiết. Nhưng một lần đăng nhập thành công từ một quốc gia lạ, lúc 3 giờ sáng — dù xác suất xảy ra cực thấp — lại là sự kiện cần cảnh báo ngay lập tức. Hệ thống phát hiện bất thường (anomaly detection) về bản chất đang gán "điểm quan trọng" cao hơn cho sự kiện có xác suất thấp hơn.
- **Nén dữ liệu (data compression):** trong một văn bản tiếng Anh, chữ cái "e" xuất hiện với tần suất rất cao, còn "z" cực kỳ hiếm. Các thuật toán nén hiệu quả (như Huffman Coding — sẽ nhắc lại ở Mục 11) gán mã ngắn cho "e" và mã dài hơn cho "z" — chính xác vì "e" mang ít thông tin hơn mỗi lần nó xuất hiện.
- **Ứng dụng thời tiết:** thông báo "Có mưa ở TP.HCM vào mùa mưa" gần như vô ích vì gần chắc chắn xảy ra; thông báo "Có tuyết rơi ở TP.HCM" (xác suất gần bằng 0) nếu xảy ra sẽ là một sự kiện gây chấn động toàn quốc.

Cả ba tình huống đều chia sẻ một quy luật chung: **sự kiện càng khó xảy ra, khi nó thực sự xảy ra, càng mang nhiều thông tin.** Mục tiêu của chương này là biến quan sát định tính đó thành một công thức toán học chính xác, có thể tính toán và lập trình được.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Lượng thông tin của một sự kiện giống như **mức độ "bất ngờ" (surprise)** mà bạn cảm nhận khi sự kiện đó xảy ra. Một sự kiện gần như chắc chắn (xác suất gần 1) hầu như không gây bất ngờ — biết nó xảy ra không cho bạn thêm thông tin gì so với những gì bạn đã đoán trước. Một sự kiện cực hiếm (xác suất gần 0) gây bất ngờ tối đa — nó "tiết lộ" cho bạn điều mà bạn gần như không thể đoán được.

Ba yêu cầu trực giác mà bất kỳ công thức đo thông tin nào cũng phải thỏa mãn:

| Yêu cầu trực giác | Diễn giải |
|---|---|
| Sự kiện chắc chắn ($P=1$) mang $0$ thông tin | Nếu bạn đã biết chắc trời sẽ nắng, câu "trời nắng" không cho bạn biết thêm điều gì |
| Sự kiện càng hiếm, thông tin càng lớn | $P$ giảm → lượng thông tin phải tăng — quan hệ **nghịch biến (decreasing)** |
| Hai sự kiện độc lập, thông tin phải **cộng lại** | Biết "đồng xu ra ngửa" VÀ biết "xúc xắc ra số 6" (hai sự kiện độc lập) phải cho tổng lượng thông tin bằng tổng thông tin của từng sự kiện riêng lẻ, không phải nhân hay phép toán khác |

Chỉ có đúng một họ hàm số thỏa mãn đồng thời cả ba yêu cầu này: hàm **logarit âm** của xác suất. Mục 7 sẽ trình bày vì sao logarit — và duy nhất logarit — là lựa chọn bắt buộc, không phải tùy chọn thẩm mỹ.

---

## 5. Trực quan hóa khái niệm

**Hình 6.1.1 — Đường cong Self-Information theo Xác suất**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
I(x) = -log₂ P(x)

Lượng thông tin (bit)
 │
7┤                                                    ●  P = 0.01 → I ≈ 6.64 bit
 │                                              ╱
6┤                                        ╱
 │                                  ╱
5┤                            ╱
 │                      ╱
4┤                ╱
 │           ╱
3┤       ╱
 │    ╱
2┤  ╱
 │╱
1┤●  P = 0.5 → I = 1 bit
 │
0┼──────●──────────────────────────────────────────── Xác suất P(x)
 0     0.5                                            1.0
                                              P = 1.0 → I = 0 bit
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Cho thấy trực tiếp quan hệ nghịch biến giữa xác suất và lượng thông tin — đường cong giảm dần về 0 khi $P \to 1$, và tăng vọt (tiệm cận $\infty$) khi $P \to 0$ |
| Điểm mấu chốt | Đường cong **không phải đường thẳng** — độ dốc tăng nhanh khi $P$ tiến về 0, nghĩa là sự khác biệt về thông tin giữa $P=0.01$ và $P=0.001$ lớn hơn nhiều so với giữa $P=0.5$ và $P=0.49$ |

---

**Hình 6.1.2 — So sánh Bốn Sự kiện Thực tế**

```text
Sự kiện                              P(x)        I(x) = -log₂P(x)
─────────────────────────────────────────────────────────────────
Tung đồng xu ra mặt bất kỳ            0.5          1.00 bit
Tung xúc xắc ra đúng số 6              1/6          2.58 bit
Trúng một số cụ thể trong xổ số 
  100 số (0–99)                       0.01          6.64 bit
Có tuyết rơi tại TP.HCM                ~0.00001     ~16.61 bit
```

*Mục đích:* Neo công thức trừu tượng vào các con số cụ thể, dễ kiểm chứng bằng tay. *Điểm mấu chốt:* sự kiện "có tuyết rơi tại TP.HCM" tuy có xác suất nhỏ hơn "trúng xổ số" chỉ khoảng 1000 lần, nhưng lượng thông tin chỉ tăng thêm khoảng 10 bit — minh chứng cho tính chất tăng chậm (logarit) chứ không tăng tuyến tính.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Self-Information (Lượng tin riêng)**
>
> Cho một sự kiện $x$ với xác suất xảy ra $P(x)$, trong đó $0 < P(x) \leq 1$. **Self-Information (Lượng tin riêng)** của $x$, ký hiệu $I(x)$, được định nghĩa:
>
> $$I(x) = -\log_b P(x)$$
>
> Trong đó $b$ là cơ số của logarit, quyết định đơn vị đo:
>
> - $b = 2$ → đơn vị **bit** (dùng phổ biến nhất trong Khoa học Máy tính)
> - $b = e$ (logarit tự nhiên, $\ln$) → đơn vị **nat** (dùng phổ biến trong tài liệu Machine Learning và tối ưu hóa liên tục)
> - $b = 10$ → đơn vị **hartley** (ít gặp, chủ yếu mang tính lịch sử)
>
> Toàn bộ Handbook, trừ khi có ghi chú khác, sử dụng $b=2$ (đơn vị bit) cho các ví dụ trực quan, và $\ln$ (đơn vị nat) khi làm việc với đạo hàm — vì đạo hàm của $\ln x$ đơn giản hơn $\log_2 x$ về mặt đại số.

**Quy ước quan trọng:** $I(x)$ chỉ được định nghĩa khi $P(x) > 0$. Nếu $P(x) = 0$ (sự kiện không thể xảy ra), $I(x)$ tiến đến $+\infty$ — một sự kiện "không thể" nếu xảy ra sẽ mang lượng thông tin vô hạn, điều này khớp hoàn toàn với trực giác ở Mục 4.

---

## 7. Nền tảng toán học

### 7.1 Vì sao phải là Logarit?

- **Ý nghĩa:** Mục 4 đặt ra ba yêu cầu trực giác. Ta cần một hàm $f(P)$ sao cho: (1) $f(1) = 0$; (2) $f$ nghịch biến trên $(0,1]$; (3) $f(P_1 \cdot P_2) = f(P_1) + f(P_2)$ với $P_1, P_2$ là xác suất của hai sự kiện độc lập.
- **Quan sát mấu chốt:** yêu cầu (3) là một **phương trình hàm (functional equation)** — nó đòi hỏi $f$ biến một phép **nhân** thành một phép **cộng**. Trong toán học, họ hàm số duy nhất có tính chất này chính là hàm logarit: $\log(P_1 \cdot P_2) = \log P_1 + \log P_2$.
- **Vì sao có dấu trừ:** vì $P \in (0,1]$ nên $\log P \leq 0$ (logarit của một số nhỏ hơn 1 luôn âm hoặc bằng 0). Để $I(x)$ là một đại lượng không âm — phù hợp với ý nghĩa "lượng" thông tin — ta cần đổi dấu: $I(x) = -\log P(x) = \log \frac{1}{P(x)}$.

> **📦 Formula Box — Self-Information**
>
> $$I(x) = -\log_2 P(x)$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $P(x)$ | Xác suất xảy ra của sự kiện $x$ (khái niệm đã học ở Part V) |
> | $-\log_2(\cdot)$ | Phép biến đổi nghịch biến, biến tích thành tổng, đảm bảo giá trị không âm |
> | **Diễn giải kỹ thuật** | Số bit tối thiểu (theo lý thuyết) cần thiết để mã hóa/thông báo sự kiện $x$ xảy ra, trong một hệ thống mã hóa tối ưu |
> | **Ứng dụng thường gặp** | Cơ sở lý thuyết của các thuật toán nén dữ liệu (Huffman Coding, Arithmetic Coding); nền tảng trực tiếp của Entropy (Chương 6.2) |

### 7.2 Tính chất Cộng tính (Additivity) cho Sự kiện Độc lập

Đây chính là yêu cầu (3) ở Mục 7.1, giờ được chứng minh cụ thể.

**Phát biểu:** nếu $X$ và $Y$ là hai biến ngẫu nhiên độc lập (Part V, Mục Independence), và $x, y$ là hai kết quả cụ thể, thì:

$$I(x, y) = I(x) + I(y)$$

**Chứng minh trực tiếp** (áp dụng kỹ thuật đã học ở Chương 1.4):

Vì $X, Y$ độc lập, theo định nghĩa xác suất độc lập (Part V): $P(x, y) = P(x) \cdot P(y)$.

Áp dụng định nghĩa self-information cho sự kiện đồng thời $(x,y)$:

$$I(x,y) = -\log_2 P(x,y) = -\log_2\big(P(x)\cdot P(y)\big)$$

Áp dụng tính chất logarit của một tích ($\log(ab) = \log a + \log b$):

$$= -\big(\log_2 P(x) + \log_2 P(y)\big) = -\log_2 P(x) - \log_2 P(y) = I(x) + I(y) \qquad \blacksquare$$

> **💡 Insight**
> Kết quả này giải thích chính xác ví dụ ở Mục 4: "đồng xu ra ngửa" ($I = 1$ bit) và "xúc xắc ra số 6" ($I \approx 2.58$ bit) là hai sự kiện độc lập, nên biết cả hai cùng lúc cho ta $1 + 2.58 = 3.58$ bit — không nhiều hơn, không ít hơn, đúng bằng tổng.

### 7.3 Chuyển đổi giữa Bit và Nat

Vì $\log_2 P = \dfrac{\ln P}{\ln 2}$ (công thức đổi cơ số logarit), ta có:

$$I_{\text{bit}}(x) = \frac{I_{\text{nat}}(x)}{\ln 2} \approx 1.4427 \times I_{\text{nat}}(x)$$

Nói cách khác, $1 \text{ nat} \approx 1.4427 \text{ bit}$. Handbook sẽ luôn ghi rõ đơn vị mỗi khi trình bày một giá trị self-information hoặc entropy, để tránh nhầm lẫn — một lỗi phổ biến khi so sánh kết quả giữa các tài liệu khác nhau.

---

## 8. Trực quan hóa quá trình thực thi

*(Chương này không có thuật toán/cơ chế nhiều bước riêng — self-information là một phép tính trực tiếp. Mục 11 sẽ giới thiệu ngắn gọn kết nối với thuật toán Huffman Coding như một ứng dụng, không phải cơ chế nội tại của định nghĩa.)*

---

## 9. Triển khai

```python
import math

def self_information(p, base=2):
    """Tính self-information I(x) = -log_b(P(x)).

    p: xác suất của sự kiện, 0 < p <= 1
    base: cơ số logarit (2 -> bit, math.e -> nat)
    """
    if not (0 < p <= 1):
        raise ValueError("Xác suất P(x) phải nằm trong khoảng (0, 1]")
    return -math.log(p, base)


def check_additivity(p_x, p_y):
    """Kiểm chứng tính chất cộng tính cho hai sự kiện độc lập:
    I(x, y) == I(x) + I(y), với P(x,y) = P(x) * P(y)."""
    i_x = self_information(p_x)
    i_y = self_information(p_y)
    i_xy = self_information(p_x * p_y)
    return math.isclose(i_xy, i_x + i_y, rel_tol=1e-9)
```

Hàm `self_information` triển khai trực tiếp Formula Box ở Mục 7.1, hỗ trợ cả hai đơn vị bit và nat thông qua tham số `base`. Hàm `check_additivity` là bản dịch trực tiếp chứng minh ở Mục 7.2 thành code kiểm chứng được — đặc biệt hữu ích để xác nhận trực giác bằng thực nghiệm trước khi tin tưởng hoàn toàn vào chứng minh hình thức.

---

## 10. Trực quan hóa kết quả tính toán

**Bảng giá trị self-information (đơn vị bit) cho các mức xác suất khác nhau**, chạy thực tế bằng hàm `self_information`:

| $P(x)$ | $I(x)$ (bit) | Diễn giải |
|---:|---:|---|
| 1.0 | 0.00 | Sự kiện chắc chắn — không mang thông tin |
| 0.5 | 1.00 | Đúng bằng 1 bit — trường hợp "công bằng" (fair coin) |
| 0.25 | 2.00 | Gấp đôi thông tin so với $P=0.5$ |
| 0.1 | 3.32 | |
| 0.01 | 6.64 | |
| 0.001 | 9.97 | |
| 0.0001 | 13.29 | |

**Kiểm chứng tính chất cộng tính** — chạy trên **1000 cặp xác suất ngẫu nhiên độc lập** khác nhau:

```text
Kiểm chứng I(x,y) = I(x) + I(y) cho sự kiện độc lập —
đúng ở toàn bộ 1000 phép thử ngẫu nhiên: True
```

Một ví dụ cụ thể để minh họa: $P(x) = 0.5$ (đồng xu), $P(y) = 1/6$ (xúc xắc ra số 6):

| Đại lượng | Giá trị |
|---|---|
| $I(x)$ | 1.00 bit |
| $I(y)$ | 2.58 bit |
| $I(x) + I(y)$ | 3.58 bit |
| $P(x,y) = P(x)\cdot P(y)$ | $1/12 \approx 0.0833$ |
| $I(x,y)$ tính trực tiếp | 3.58 bit |

Hai giá trị cuối khớp nhau, đúng như chứng minh ở Mục 7.2. *(Nhắc lại nguyên tắc từ Chương 1.4 và 1.5: 1000 phép thử ngẫu nhiên là minh chứng thực nghiệm mạnh, không thay thế chứng minh hình thức — chứng minh hình thức đầy đủ đã có ở Mục 7.2.)*

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Self-information không chỉ là một định nghĩa toán học trừu tượng — nó là cơ sở lý thuyết trực tiếp cho nhiều hệ thống kỹ thuật đang vận hành hằng ngày.

| Bối cảnh công nghiệp | Vai trò của Self-Information |
|---|---|
| Huffman Coding (thuật toán nén) | Gán độ dài mã (số bit) cho mỗi ký tự **xấp xỉ bằng** self-information của ký tự đó — ký tự có xác suất xuất hiện cao (self-information thấp) nhận mã ngắn |
| Hệ thống phát hiện bất thường (Anomaly Detection) | Một sự kiện log có self-information cao (xác suất thấp) được ưu tiên gắn cờ cảnh báo — nền tảng của nhiều hệ thống SIEM (Security Information and Event Management) |
| Thiết kế hệ thống thông báo (notification ranking) | Xếp hạng độ ưu tiên của thông báo dựa trên "mức độ bất ngờ" của sự kiện, thay vì chỉ dựa trên loại sự kiện |
| Lý thuyết mã hóa kênh truyền (Channel Coding) | Xác định dung lượng tối đa (channel capacity) mà một đường truyền có nhiễu có thể truyền tải một cách tin cậy — bài toán gốc mà Shannon giải quyết năm 1948 |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Self-information mà bạn vừa học là điểm khởi đầu của một nhánh nghiên cứu vẫn đang hoạt động sôi nổi: mối quan hệ giữa "thông tin", "độ phức tạp", và "khả năng nén".

Công trình gốc của Shannon (1948) đã chứng minh **Định lý Mã hóa Nguồn (Source Coding Theorem)**: không tồn tại phương pháp mã hóa nào có thể nén một nguồn dữ liệu xuống dưới mức trung bình bằng entropy của nguồn đó (Entropy — chủ đề của Chương 6.2, chính là kỳ vọng của self-information). Đây là một **giới hạn lý thuyết cứng (hard theoretical limit)** — không phải giới hạn công nghệ có thể vượt qua bằng thuật toán tốt hơn trong tương lai, mà là một giới hạn toán học, tương tự tinh thần với giới hạn tính toán được của Turing (đã gặp ở Chương 1.1) hay giới hạn chứng minh được của Gödel (Chương 1.4).

**Hướng nghiên cứu mở rộng liên quan** (sẽ không đi sâu trong Handbook này, nhưng đáng để người đọc biết đến): **Kolmogorov Complexity** — một định nghĩa khác về "lượng thông tin" của một đối tượng, không dựa trên xác suất mà dựa trên độ dài của chương trình ngắn nhất có thể sinh ra đối tượng đó. Hai lý thuyết (Shannon và Kolmogorov) độc lập về mặt lịch sử nhưng hội tụ về nhiều kết quả tương tự nhau — một chủ đề nghiên cứu liên ngành giữa Information Theory và Theory of Computation (Volume 2, Part IX).

**Câu hỏi mở** để suy ngẫm trước khi bước sang Chương 6.2: nếu self-information đo lượng thông tin của *một* sự kiện, làm sao ta đo lượng thông tin *trung bình* của *toàn bộ* một phân phối xác suất — ví dụ, "trung bình một ký tự tiếng Việt mang bao nhiêu bit thông tin"? Đây chính là câu hỏi mà Entropy (Chương 6.2) trả lời.

---

## 13. Ưu điểm

- **Nền tảng toán học đơn giản, chỉ cần logarit** — không đòi hỏi công cụ toán học nặng hơn những gì đã học ở Part IV và Part V.
- **Tính chất cộng tính (Mục 7.2)** giúp self-information kết hợp tự nhiên với các phép toán xác suất đã quen thuộc (độc lập, phân phối đồng thời).
- **Tổng quát, không phụ thuộc ngữ nghĩa** — áp dụng được cho bất kỳ loại sự kiện nào (văn bản, tín hiệu, nhãn phân loại, log hệ thống) miễn có thể gán xác suất.
- **Là nền tảng trực tiếp, không thể thay thế** cho toàn bộ các khái niệm còn lại của Part VI và của Cross-Entropy Loss trong Machine Learning (Volume 5).

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Self-information đo lượng thông tin của **một sự kiện đơn lẻ đã biết trước xác suất chính xác** — nó **không** đo lượng thông tin của một *phân phối* nói chung (đó là việc của Entropy, Chương 6.2), và **không** đo "ý nghĩa" hay "giá trị sử dụng" của thông tin đối với con người.

- **Phụ thuộc hoàn toàn vào $P(x)$ đã biết chính xác:** nếu xác suất ước lượng sai (ví dụ mô hình dùng $Q(x)$ thay vì $P(x)$ thật), giá trị self-information tính được sẽ sai lệch — vấn đề này chính là động lực cho Cross-Entropy (Chương 6.3).
- **Không phản ánh giá trị chủ quan:** một tin nhắn có self-information cao (xác suất thấp) về mặt toán học "chứa nhiều thông tin", nhưng chưa chắc *quan trọng* hay *hữu ích* với người nhận — Information Theory cố tình bỏ qua khía cạnh ngữ nghĩa này (Mục 2).
- **Không xác định khi $P(x) = 0$:** về mặt lý thuyết $I(x) \to \infty$, nhưng trong thực hành lập trình (Mục 9), giá trị $P(x)=0$ cần được xử lý riêng để tránh lỗi toán học (`log(0)` không xác định).

---

## 15. So sánh

**Bảng 6.1.1 — Xác suất và Self-Information: hai đại lượng nghịch biến**

| Đặc điểm | Xác suất $P(x)$ | Self-Information $I(x)$ |
|---|---|---|
| Khoảng giá trị | $[0, 1]$ | $[0, +\infty)$ |
| Sự kiện chắc chắn | $P(x) = 1$ | $I(x) = 0$ |
| Sự kiện không thể | $P(x) = 0$ | $I(x) \to \infty$ |
| Quan hệ với sự kiện độc lập | **Nhân**: $P(x,y) = P(x)P(y)$ | **Cộng**: $I(x,y) = I(x) + I(y)$ |
| Ý nghĩa trực giác | "Khả năng xảy ra" | "Mức độ bất ngờ nếu xảy ra" |

**Phân tích:** hai đại lượng này không phải hai cách nhìn tùy ý về cùng một hiện tượng — Mục 7.1 đã chứng minh $I(x) = -\log_2 P(x)$ là **hàm số duy nhất** (sai khác một hằng số nhân, quyết định bởi cơ số logarit) thỏa mãn đồng thời cả ba yêu cầu trực giác đặt ra ở Mục 4. Đây là một minh chứng cho nguyên tắc "một định nghĩa tốt trong toán học thường là định nghĩa duy nhất khả dĩ", chứ không phải một lựa chọn thẩm mỹ giữa nhiều phương án tương đương.

---

## 16. Tóm tắt

- **Self-Information** $I(x) = -\log_2 P(x)$ đo lượng thông tin (hay "mức độ bất ngờ") của một sự kiện đơn lẻ $x$, dựa hoàn toàn vào xác suất $P(x)$ của nó.
- Logarit là lựa chọn **bắt buộc về mặt toán học**, không phải tùy chọn — vì đó là họ hàm duy nhất biến phép nhân xác suất thành phép cộng thông tin (Mục 7.1).
- Đơn vị đo phổ biến nhất là **bit** ($\log_2$) và **nat** ($\ln$), với hệ số chuyển đổi $1 \text{ nat} \approx 1.4427 \text{ bit}$.
- Với hai sự kiện **độc lập**, self-information có **tính chất cộng tính**: $I(x,y) = I(x) + I(y)$ — đã được chứng minh trực tiếp và kiểm chứng bằng thực nghiệm.
- Self-information là nền tảng trực tiếp cho **Entropy** (Chương 6.2 — kỳ vọng của self-information trên toàn bộ phân phối) và gián tiếp cho Cross-Entropy Loss, hàm mất mát cốt lõi của Machine Learning hiện đại.

Chương 6.2 sẽ trả lời câu hỏi mở đặt ra ở Mục 12: làm sao đo lượng thông tin *trung bình* của cả một phân phối, bằng cách lấy kỳ vọng của chính $I(x)$ vừa học.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Tính $I(x)$ (theo đơn vị bit) cho các sự kiện sau: (a) tung một đồng xu công bằng ra mặt ngửa; (b) rút một lá bài cụ thể (ví dụ: Át Cơ) từ bộ bài 52 lá; (c) một sự kiện có xác suất $P(x) = 1$.
2. Chuyển đổi giá trị $I(x) = 5 \text{ bit}$ sang đơn vị nat.
3. Nếu $P(x) = 0.5$, hãy tính $I(x)$ theo cả ba đơn vị: bit, nat, và hartley (cơ số 10).

### Mức Trung bình (Intermediate)

4. Cho hai sự kiện độc lập $X$ (tung đồng xu, $P(\text{ngửa}) = 0.5$) và $Y$ (tung xúc xắc, $P(\text{ra số chẵn}) = 0.5$). Tính $I(x)$, $I(y)$, và $I(x,y)$ bằng hai cách — (a) tính trực tiếp từ $P(x,y)$; (b) cộng $I(x) + I(y)$ — và xác nhận hai kết quả khớp nhau.
5. Dùng chứng minh trực tiếp (kỹ thuật đã học ở Chương 1.4), chứng minh rằng $I(x)$ là một hàm **nghịch biến chặt (strictly decreasing)** theo $P(x)$ trên khoảng $(0, 1]$. *(Gợi ý: xét đạo hàm $\dfrac{dI}{dP}$.)*

### Mức Nâng cao (Advanced)

6. Trong một hệ thống ghi log an ninh mạng, giả sử một loại sự kiện đăng nhập bình thường có $P = 0.999$, còn một loại sự kiện đăng nhập bất thường có $P = 0.001$. Tính tỷ lệ $\dfrac{I(\text{bất thường})}{I(\text{bình thường})}$. Từ kết quả này, hãy giải thích bằng lời (không chỉ bằng số) vì sao hệ thống giám sát nên ưu tiên cảnh báo dựa trên self-information thay vì chỉ dựa trên "loại sự kiện" đơn thuần.
7. **(Định hướng nghiên cứu)** Xét một bảng chữ cái chỉ gồm 2 ký tự $\{A, B\}$ với $P(A) = p$ và $P(B) = 1-p$. Vẽ (bằng tay hoặc bằng code) đồ thị của $I(A)$ và $I(B)$ theo $p$ trên cùng một hệ trục, với $p$ chạy từ $0.01$ đến $0.99$. Quan sát: tại giá trị $p$ nào thì $I(A) = I(B)$? Kết quả này sẽ được dùng lại trực tiếp ở Chương 6.2 khi khảo sát entropy cực đại của phân phối hai giá trị (binary distribution).

---

## 18. Dự án nhỏ

**Không áp dụng cho chương này.**

Tương tự tiền lệ ở Chương 1.5, chương này tập trung xây dựng nền tảng khái niệm toán học thuần túy hơn là một sản phẩm kỹ thuật độc lập. Self-information sẽ được vận dụng trực tiếp trong Dự án tích hợp ở Chương 6.3 (Cross Entropy), nơi có ứng dụng thực hành rõ ràng trong bài toán phân loại (classification).

---

## 19. Tự đánh giá

- [ ] Tôi có thể giải thích bằng lời, không cần công thức, vì sao sự kiện hiếm phải mang nhiều thông tin hơn sự kiện phổ biến.
- [ ] Tôi có thể tự tính $I(x) = -\log_2 P(x)$ cho một xác suất cụ thể mà không cần tra lại công thức.
- [ ] Tôi hiểu vì sao logarit là lựa chọn toán học bắt buộc (không phải ngẫu nhiên) để định nghĩa self-information — có thể giải thích lại lập luận ở Mục 7.1.
- [ ] Tôi có thể chứng minh và kiểm chứng bằng số tính chất cộng tính $I(x,y) = I(x) + I(y)$ cho sự kiện độc lập.
- [ ] Tôi phân biệt được rõ ràng đơn vị bit và nat, và biết cách chuyển đổi giữa hai đơn vị.

Nếu Bài tập 5 (chứng minh tính nghịch biến bằng đạo hàm) còn khó khăn, đây là dấu hiệu nên ôn lại nhanh khái niệm đạo hàm ở Part IV — Calculus trước khi tiếp tục sang Chương 6.2, nơi kỹ thuật lấy đạo hàm sẽ được dùng lại để tìm cực trị của Entropy.

---

## 20. Đọc thêm

- **Sách:** Marc Peter Deisenroth, A. Aldo Faisal, Cheng Soon Ong, *Mathematics for Machine Learning* — phần nền tảng xác suất làm tiền đề trực tiếp cho Information Theory. *(Xem BOOKS.md — Volume 1.)*
- **Paper gốc:** Claude Shannon, *A Mathematical Theory of Communication* (1948) — nguồn gốc của toàn bộ định nghĩa trong chương này. *(Xem PAPERS.md, TIMELINE.md, SCIENTISTS.md.)*
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc giới thiệu nhập môn về Kolmogorov Complexity — một định nghĩa thay thế về "lượng thông tin", để thấy Information Theory không chỉ có một cách tiếp cận duy nhất.
- **Chương tiếp theo:** Chương 6.2 — Entropy.

---

### Liên kết chương (Cross References)

- **Chương trước:** Chương cuối cùng của Part V — Probability & Statistics (khái niệm xác suất, biến ngẫu nhiên, và tính độc lập được sử dụng trực tiếp xuyên suốt chương này).
- **Chương tiếp theo:** 6.2 — Entropy (lấy kỳ vọng của self-information $I(x)$ trên toàn bộ phân phối để thu được entropy $H(X)$).
- **Chương liên quan xa hơn:** Chương 6.3 — Cross Entropy (mở rộng self-information khi xác suất thật $P$ khác xác suất mô hình ước lượng $Q$); Volume 2, Part IX — Theory of Computation (kết nối với Kolmogorov Complexity, Mục 12); Volume 5, Part II — Machine Learning (Cross-Entropy Loss là ứng dụng trực tiếp của toàn bộ chuỗi khái niệm bắt đầu từ chương này).
- **Vị trí trong Knowledge Graph:** Nút đầu tiên của Volume 1, Part VI, phụ thuộc trực tiếp vào Part V (Probability) và gián tiếp vào Part IV (Calculus, cho Bài tập 5); là điều kiện tiên quyết bắt buộc cho toàn bộ bốn chương còn lại của Part VI.

---

*Hết Chương 6.1. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`, khớp với outline đã đề xuất cho Part VI (self-information, không có Mini Project theo đúng tiền lệ thiết lập ở Chương 1.5 cho các chương nền tảng toán học thuần túy). Mọi kết quả về tính chất cộng tính đều được kiểm chứng thực nghiệm bằng Python (1000 phép thử ngẫu nhiên), đồng thời phân biệt rõ ràng kiểm chứng thực nghiệm với chứng minh hình thức — đúng nguyên tắc đã thiết lập từ Chương 1.4 và 1.5. Đang chờ rà soát trước khi tiếp tục sang Chương 6.2 — Entropy.*
