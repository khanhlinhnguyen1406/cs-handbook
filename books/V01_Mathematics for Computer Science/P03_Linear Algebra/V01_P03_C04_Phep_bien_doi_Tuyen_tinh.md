# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part III — Linear Algebra
## Chương 3.4 — Phép biến đổi Tuyến tính
### (Linear Transformations)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 3.4 |
| Thuộc Part | III — Linear Algebra |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 55–65 phút |
| Độ khó | ★★★★☆ |
| Kiến thức tiên quyết | Chương 3.1 — Vectors; Chương 3.2 — Matrices; Chương 3.3 — Matrix Operations (bắt buộc — chương này hình thức hóa chính xác trực giác "ma trận là phép biến đổi" đã dùng từ đầu Part III) |
| Chương liên quan | 3.5 — Systems of Linear Equations (dùng trực tiếp khái niệm hạng, span để phân tích tính giải được); 3.6 — Eigenvalues and Eigenvectors; Volume 5 — Artificial Intelligence (lớp linear/affine trong neural network, vì sao cần hàm phi tuyến giữa các lớp) |
| Từ khóa | linear transformation, additivity, homogeneity, standard basis, span, linear independence, basis, dimension, composition of transformations |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Phát biểu chính xác định nghĩa hình thức của phép biến đổi tuyến tính (linear transformation) qua hai tính chất: tính cộng và tính thuần nhất.
- Chứng minh (hoặc bác bỏ) một hàm số cho trước có phải là phép biến đổi tuyến tính hay không.
- Xây dựng ma trận biểu diễn của một phép biến đổi tuyến tính bằng cách áp dụng nó lên các vector cơ sở chuẩn.
- Giải thích chính xác vì sao hợp thành (composition) hai phép biến đổi tuyến tính tương ứng với phép nhân ma trận — hình thức hóa trực giác đã gợi mở ở Chương 3.2, Mục 7.2.
- Định nghĩa và kiểm tra tính độc lập tuyến tính (linear independence), span, và cơ sở (basis) của một tập vector.
- Giải thích vì sao xếp chồng nhiều lớp tuyến tính liên tiếp mà không có hàm phi tuyến xen giữa là vô nghĩa về mặt biểu diễn — một cạm bẫy kỹ thuật thực sự trong thiết kế neural network.

---

### Câu hỏi khơi gợi

> *Vì sao phép biến đổi $x \mapsto 2x$ được gọi là "tuyến tính", nhưng $x \mapsto 2x + 3$ — chỉ khác một hằng số cộng thêm — lại không? Và vì sao $x \mapsto x^2$ hoàn toàn không phải là phép biến đổi tuyến tính? Câu trả lời không chỉ là một quy ước đặt tên — nó vạch ra chính xác ranh giới giữa những gì một ma trận có thể biểu diễn, và những gì nó không bao giờ có thể biểu diễn được, dù ma trận đó lớn đến đâu.*

---

## 1. Tổng quan chương

Ba chương trước xây dựng công cụ (vector, ma trận, các phép toán trên ma trận) và liên tục gợi ý một trực giác: "ma trận là một phép biến đổi". Chương này biến trực giác đó thành một định nghĩa toán học chính xác, và chứng minh một kết quả trung tâm của toàn bộ đại số tuyến tính: **mọi phép biến đổi tuyến tính giữa các không gian hữu hạn chiều đều tương ứng với đúng một ma trận** — và ngược lại.

Chương này cũng hình thức hóa đầy đủ khái niệm **hạng (rank)** đã giới thiệu ở Chương 3.3 (khi đó chỉ ở mức trực giác), thông qua hai khái niệm mới: **span** và **độc lập tuyến tính (linear independence)**. Đây là bộ từ vựng chính xác cần thiết để Chương 3.5 phân tích khi nào một hệ phương trình tuyến tính có nghiệm, và Chương 3.6 định nghĩa eigenvector.

> **💡 Insight**
> Sau chương này, khi thấy dòng code `y = W @ x` trong bất kỳ mô hình Machine Learning nào, bạn sẽ không chỉ đọc nó như "nhân ma trận với vector" — bạn sẽ đọc được chính xác *loại* phép biến đổi hình học nào có thể xảy ra (và loại nào không thể) chỉ từ việc biết đó là một phép biến đổi tuyến tính.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1844 | Hermann Grassmann | Trong *Die lineale Ausdehnungslehre* (đã nhắc ở Chương 3.1), hình thức hóa không chỉ không gian vector mà cả khái niệm **ánh xạ tuyến tính (linear map)** giữa các không gian đó, ở dạng tổng quát $n$ chiều |
| 1888 | Giuseppe Peano | Đưa ra hệ tiên đề hiện đại cho không gian vector và phép biến đổi tuyến tính trong *Calcolo Geometrico* — đặt nền móng cho cách trình bày đại số tuyến tính dùng trong sách giáo khoa ngày nay |
| Đầu thế kỷ 20 | Phát triển Giải tích Hàm (Functional Analysis) | Các nhà toán học (David Hilbert và cộng sự) mở rộng khái niệm phép biến đổi tuyến tính sang không gian **vô hạn chiều** — nền tảng lý thuyết gián tiếp cho các phương pháp kernel trong Machine Learning cổ điển (Volume 5) |
| Giữa thế kỷ 20 | Đại số tuyến tính trở thành ngôn ngữ chuẩn của Khoa học Máy tính | Với sự ra đời của máy tính điện tử, phép biến đổi tuyến tính (biểu diễn bằng ma trận) trở thành công cụ tính toán trung tâm cho đồ họa máy tính, xử lý tín hiệu, và sau này là Machine Learning |

Điều đáng chú ý: định nghĩa hình thức "phép biến đổi tuyến tính" ra đời **sau** khi con người đã sử dụng các trường hợp cụ thể của nó (xoay, co giãn hình học) hàng thế kỷ mà không cần một định nghĩa tổng quát. Đây là mô hình lặp lại nhiều lần trong lịch sử Toán học: trực giác và ứng dụng cụ thể thường đi trước sự hình thức hóa trừu tượng — đúng như trình tự Handbook đang dẫn dắt bạn qua Part III (trực giác từ Chương 3.1–3.3, hình thức hóa ở chương này).

---

## 3. Động lực

Ba tình huống kỹ thuật minh họa vì sao cần định nghĩa chính xác phép biến đổi tuyến tính:

- Một lớp (layer) trong mạng neural network thực hiện $y = Wx$. Nếu bạn xếp chồng hai lớp như vậy liên tiếp mà không có gì khác xen giữa: $y = W_2(W_1x) = (W_2W_1)x$ — theo Mục 7.2, đây **vẫn chỉ là một phép biến đổi tuyến tính duy nhất**, dù có bao nhiêu lớp đi nữa. Đây chính là lý do kỹ thuật vì sao mọi kiến trúc Deep Learning thực tế đều cần **hàm kích hoạt phi tuyến (nonlinear activation function)** xen giữa các lớp — nếu không, hàng trăm lớp cũng chỉ tương đương với đúng một phép biến đổi tuyến tính duy nhất, không mạnh hơn.
- Bạn xây dựng pipeline biến đổi trong đồ họa 3D (model → view → projection). Mỗi bước là một ma trận; toàn bộ pipeline được "gộp" thành một ma trận duy nhất bằng cách nhân chúng lại — bạn đã dùng Mục 7.2 mà không biết.
- Bạn cần biết một tập đặc trưng (feature) trong dữ liệu có thực sự "độc lập thông tin" hay không (Chương 3.3, Mục 7.3, nhưng khi đó chỉ ở mức trực giác) — Mục 6 của chương này cho một định nghĩa chính xác, kiểm chứng được: **độc lập tuyến tính**.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Một phép biến đổi tuyến tính **giữ nguyên gốc tọa độ tại chỗ**, và **không bao giờ bẻ cong không gian**: đường thẳng luôn biến thành đường thẳng, các đường song song vẫn song song, khoảng cách đều nhau trên một đường thẳng vẫn đều nhau sau biến đổi. Nếu hình dung một tấm lưới ô vuông đều đặn, sau một phép biến đổi tuyến tính, tấm lưới có thể bị xoay, kéo dãn, nén, hay cắt xiên (shear) — nhưng các đường lưới vẫn luôn là các đường thẳng song song, cách đều nhau.

| Trực giác kỹ thuật bạn đã có | Khái niệm toán học tương ứng |
|---|---|
| Hàm `scale(x) => 2 * x` | Phép biến đổi tuyến tính hợp lệ |
| Hàm `affine(x) => 2 * x + 3` | **Không** phải phép biến đổi tuyến tính thuần túy (gọi là phép biến đổi **affine** — Mục 15) |
| Hàm `square(x) => x * x` | **Không** phải phép biến đổi tuyến tính (Mục 6 giải thích chính xác vì sao) |
| Số cột thực sự "mới" trong một bảng dữ liệu (không suy ra được từ các cột khác) | Số vector độc lập tuyến tính (Mục 6) |
| Tập hợp mọi kết quả có thể tạo ra bằng cách tổ hợp một số "màu cơ bản" (RGB) | Span của tập vector cơ sở |

> **⚠️ Common Mistake**
> Trong tài liệu Machine Learning, một "lớp tuyến tính" (`nn.Linear` trong PyTorch) hầu như luôn thực hiện $y = Wx + b$ — có cả số hạng $b$ cộng thêm. Theo định nghĩa toán học chặt chẽ ở Mục 6, đây **không phải** phép biến đổi tuyến tính (vì không thỏa mãn $T(\vec{0})=\vec{0}$) — nó là một **phép biến đổi affine**. Đây không phải lỗi của cộng đồng Machine Learning; chỉ là một sự khác biệt thuật ngữ giữa hai lĩnh vực cần nhận biết rõ để tránh nhầm lẫn khi đọc tài liệu (Mục 15 phân tích đầy đủ).

---

## 5. Trực quan hóa khái niệm

**Hình 3.4.1 — Lưới ô vuông trước và sau Phép biến đổi Tuyến tính**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
   Trước biến đổi                Sau biến đổi tuyến tính
   (lưới đều, vuông góc)         (lưới vẫn thẳng, song song,
                                   đều đặn — chỉ bị xoay/kéo xiên)

   ┼──┼──┼──┼                    ╱──╱──╱──╱
   ┼──┼──┼──┼        T          ╱──╱──╱──╱
   ┼──┼──┼──┼      ────►       ╱──╱──╱──╱
   ┼──┼──┼──┼                  ╱──╱──╱──╱

                    (so sánh — phép biến đổi PHI tuyến,
                     ví dụ x²: lưới bị bẻ cong, không còn
                     là các đường thẳng song song đều đặn)
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Minh họa trực quan chính xác ranh giới "tuyến tính" — lưới ô vuông sau biến đổi vẫn giữ các đường thẳng song song, đều đặn; đây là dấu hiệu nhận biết trực quan (không thay thế cho kiểm tra bằng định nghĩa Mục 6) |
| Điểm mấu chốt | Nếu sau biến đổi, các đường lưới bị **bẻ cong** hoặc khoảng cách giữa chúng **không còn đều**, phép biến đổi đó chắc chắn **không** tuyến tính |

---

**Hình 3.4.2 — Span của hai Vector: Toàn bộ Mặt phẳng so với Chỉ một Đường thẳng**

```text
   Hai vector KHÔNG song song        Hai vector SONG SONG
   → span = toàn bộ mặt phẳng        → span = chỉ một đường thẳng

        ↑ v₂                              ↑ v₂
        │  ↗ v₁                           │  ↗ v₁
    ────┼────►                        ────┼────►
        │                                 │
   (mọi điểm trên mặt phẳng           (chỉ các điểm nằm trên
    đều viết được thành                đúng một đường thẳng đó
    tổ hợp của v₁, v₂)                 mới viết được thành
                                       tổ hợp của v₁, v₂)
```

*Mục đích:* Minh họa trực quan khái niệm span (Mục 6) — với hai vector không song song, "tổ hợp tuyến tính" của chúng phủ kín toàn bộ mặt phẳng 2D; với hai vector song song (phụ thuộc tuyến tính), span chỉ là một đường thẳng — một chiều thông tin đã bị "lãng phí". *Điểm mấu chốt:* đây chính xác là hình ảnh hình học của khái niệm **độc lập tuyến tính** sẽ định nghĩa chính thức ở Mục 6, và trực tiếp giải thích lại vì sao ma trận có cột song song ở Chương 3.3 lại suy biến ($\det=0$).

---

## 6. Định nghĩa hình thức

> **📌 Remember — Phép biến đổi Tuyến tính (Linear Transformation)**
>
> Một hàm số $T: \mathbb{R}^n \to \mathbb{R}^m$ được gọi là **phép biến đổi tuyến tính** nếu, với **mọi** vector $\vec{u}, \vec{v} \in \mathbb{R}^n$ và **mọi** số thực $c$:
>
> 1. **Tính cộng (Additivity):** $T(\vec{u} + \vec{v}) = T(\vec{u}) + T(\vec{v})$
> 2. **Tính thuần nhất (Homogeneity):** $T(c\vec{u}) = c\, T(\vec{u})$
>
> Hai điều kiện này thường được gộp chung gọi là **nguyên lý chồng chất (superposition principle)**. Hệ quả trực tiếp: mọi phép biến đổi tuyến tính đều thỏa $T(\vec{0}) = \vec{0}$ — nếu một hàm không thỏa điều này, nó **chắc chắn không** tuyến tính (kiểm tra nhanh, loại trừ ngay các phép biến đổi affine như $T(x)=2x+3$).

**Định lý Biểu diễn Ma trận (Matrix Representation Theorem)** — kết quả trung tâm của chương:

> Mọi phép biến đổi tuyến tính $T:\mathbb{R}^n \to \mathbb{R}^m$ đều tương ứng **duy nhất** với một ma trận $A \in \mathbb{R}^{m\times n}$ sao cho $T(\vec{x}) = A\vec{x}$ với mọi $\vec{x}$. Ma trận này được xây dựng bằng cách áp dụng $T$ lên từng **vector cơ sở chuẩn (standard basis)** $\vec{e}_1, \dots, \vec{e}_n$ (trong đó $\vec{e}_i$ có thành phần thứ $i$ bằng 1, còn lại bằng 0), rồi xếp các kết quả $T(\vec{e}_i)$ làm các **cột** của $A$ — chi tiết công thức ở Mục 7.1.

**Ba khái niệm còn lại của chương** — cho một tập vector $\{\vec{v}_1, \dots, \vec{v}_k\}$:

| Khái niệm | Định nghĩa |
|---|---|
| Tổ hợp tuyến tính (Linear Combination) | Bất kỳ vector nào có dạng $c_1\vec{v}_1 + c_2\vec{v}_2 + \dots + c_k\vec{v}_k$ với $c_i$ là các số thực |
| Span | Tập hợp **tất cả** các tổ hợp tuyến tính có thể tạo ra từ $\{\vec{v}_1,\dots,\vec{v}_k\}$ |
| Độc lập tuyến tính (Linear Independence) | Tập $\{\vec{v}_1,\dots,\vec{v}_k\}$ được gọi là **độc lập tuyến tính** nếu phương trình $c_1\vec{v}_1+\dots+c_k\vec{v}_k = \vec{0}$ **chỉ** có nghiệm $c_1=\dots=c_k=0$ (không có nghiệm nào khác) — nói cách khác, không vector nào trong tập có thể viết thành tổ hợp của các vector còn lại |
| Cơ sở (Basis) | Một tập vector vừa **độc lập tuyến tính**, vừa **span** toàn bộ không gian đang xét |
| Chiều (Dimension) | Số lượng vector trong một cơ sở của không gian — luôn là một số cố định, không phụ thuộc vào việc chọn cơ sở nào |

---

## 7. Nền tảng toán học

### 7.1 Xây dựng Ma trận Biểu diễn

> **📦 Formula Box — Ma trận Biểu diễn của Phép biến đổi Tuyến tính**
>
> $$A = \Big[\ T(\vec{e}_1)\ \Big|\ T(\vec{e}_2)\ \Big|\ \dots\ \Big|\ T(\vec{e}_n)\ \Big]$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $\vec{e}_i$ | Vector cơ sở chuẩn thứ $i$ — ví dụ trong $\mathbb{R}^2$: $\vec{e}_1=(1,0)$, $\vec{e}_2=(0,1)$ |
> | $T(\vec{e}_i)$ | Kết quả khi áp dụng $T$ lên $\vec{e}_i$ — một vector cột chiều $m$ |
> | **Diễn giải kỹ thuật** | Vì mọi vector $\vec{x}=(x_1,\dots,x_n)$ đều viết được là $\vec{x}=x_1\vec{e}_1+\dots+x_n\vec{e}_n$, và $T$ tuyến tính (Mục 6), nên $T(\vec{x}) = x_1T(\vec{e}_1)+\dots+x_nT(\vec{e}_n) = A\vec{x}$ — đây chính là cách chứng minh Định lý Biểu diễn Ma trận |
> | **Ứng dụng thường gặp** | Tìm ma trận cụ thể cho bất kỳ phép biến đổi hình học nào (xoay, co giãn, chiếu) chỉ bằng cách hỏi "vector cơ sở đi về đâu?" |

**Ví dụ tay:** cho $T(x,y) = (2x,\ x+y)$. Áp dụng lên cơ sở chuẩn của $\mathbb{R}^2$: $T(\vec{e}_1)=T(1,0)=(2,1)$, $T(\vec{e}_2)=T(0,1)=(0,1)$. Vậy $A = \begin{pmatrix}2&0\\1&1\end{pmatrix}$ — kiểm chứng: $A\begin{pmatrix}x\\y\end{pmatrix} = \begin{pmatrix}2x\\x+y\end{pmatrix}$, khớp chính xác định nghĩa gốc của $T$.

### 7.2 Hợp thành Phép biến đổi = Phép nhân Ma trận

> **💡 Insight**
> Đây chính là lời giải thích đầy đủ cho Insight đã nêu ở Chương 3.2, Mục 7.2. Nếu $T_1$ có ma trận biểu diễn $A$ và $T_2$ có ma trận biểu diễn $B$, thì phép **hợp thành** $(T_1 \circ T_2)(\vec{x}) = T_1(T_2(\vec{x}))$ — "áp dụng $T_2$ trước, rồi $T_1$" — có ma trận biểu diễn chính xác là $AB$. Điều này **không phải** định nghĩa tùy ý; nó là hệ quả **bắt buộc** của Định lý Biểu diễn Ma trận (Mục 7.1) áp dụng hai lần liên tiếp: $T_1(T_2(\vec{x})) = A(B\vec{x}) = (AB)\vec{x}$, dùng đúng định nghĩa phép nhân ma trận (Chương 3.2, Mục 7.1) làm cho đẳng thức $A(B\vec{x})=(AB)\vec{x}$ luôn đúng.

**Rank được hình thức hóa lại chính xác:** hạng của ma trận $A$ (Chương 3.3, Mục 7.3) chính là **chiều của span** các vector cột của $A$ — tương đương, số chiều "thực sự sử dụng" của không gian đầu ra khi áp dụng phép biến đổi $A$. Đây là định nghĩa chính xác thay thế cho trực giác "số cột độc lập thông tin" đã dùng tạm ở Chương 3.3.

---

## 8. Thuật toán / Cơ chế

**Kiểm tra Độc lập Tuyến tính:**

```text
Bước 1 — Nhận vào k vector, mỗi vector chiều n
        │
        ▼
Bước 2 — Xếp k vector này làm k cột của một ma trận A (kích thước n×k)
        │
        ▼
Bước 3 — Tính hạng của A (dùng khử Gauss — chi tiết đầy đủ ở Chương 3.5)
        │
        ▼
Bước 4 — Nếu rank(A) = k (bằng đúng số vector đưa vào):
        │         → Tập vector ĐỘC LẬP TUYẾN TÍNH
        ▼
         Nếu rank(A) < k:
                  → Tập vector PHỤ THUỘC TUYẾN TÍNH
```

**Xây dựng Ma trận Biểu diễn của một Phép biến đổi:**

```text
Bước 1 — Nhận vào hàm T và số chiều đầu vào n
        │
        ▼
Bước 2 — Với mỗi chỉ số i từ 1 đến n:
        │
        ▼
Bước 3 —   Tạo vector cơ sở chuẩn eᵢ (toàn số 0, riêng vị trí i bằng 1)
        │
        ▼
Bước 4 —   Tính T(eᵢ) — đây sẽ là cột thứ i của ma trận kết quả
        │
        ▼
Bước 5 — Ghép n vector T(e₁),...,T(eₙ) thành các cột của ma trận A
```

---

## 9. Triển khai

```python
import numpy as np

def is_linear(T, n, num_tests=5, tol=1e-9):
    """Kiểm tra tính tuyến tính bằng thực nghiệm số học,
    dựa trên hai điều kiện ở Mục 6."""
    for _ in range(num_tests):
        u, v = np.random.randn(n), np.random.randn(n)
        c = np.random.randn()

        additive = np.allclose(T(u + v), T(u) + T(v), atol=tol)
        homogeneous = np.allclose(T(c * u), c * T(u), atol=tol)
        if not (additive and homogeneous):
            return False
    return True


def matrix_representation(T, n):
    """Xây dựng ma trận biểu diễn — triển khai trực tiếp
    thuật toán ở Mục 8 và Formula Box Mục 7.1."""
    columns = []
    for i in range(n):
        e_i = np.zeros(n)
        e_i[i] = 1
        columns.append(T(e_i))
    return np.column_stack(columns)


# Kiểm chứng với ví dụ tay Mục 7.1: T(x, y) = (2x, x + y)
T = lambda v: np.array([2 * v[0], v[0] + v[1]])

print(is_linear(T, n=2))              # True
print(matrix_representation(T, n=2))  # [[2. 0.] [1. 1.]]  — khớp ví dụ tay
```

Hàm `is_linear` **không chứng minh chặt chẽ** tính tuyến tính (chỉ kiểm tra một số mẫu ngẫu nhiên hữu hạn) — đây là một phép kiểm tra thực nghiệm hữu ích để phát hiện nhanh trường hợp *không* tuyến tính, không thay thế chứng minh hình thức bằng đại số (Bài tập 4–5). Hàm `matrix_representation` là bản dịch trực tiếp thuật toán ở Mục 8 — áp dụng $T$ lên từng vector cơ sở chuẩn và ghép kết quả thành các cột.

---

## 10. Trực quan hóa quá trình thực thi

**Kiểm tra một hàm KHÔNG tuyến tính** — $T_{\text{sq}}(x,y) = (x^2, y)$:

```text
is_linear(T_sq, n=2)   →  False

Chi tiết một lần kiểm tra thất bại:
  u = [1, 0],  v = [1, 0]
  T(u+v) = T([2,0]) = [4, 0]
  T(u)+T(v) = [1,0] + [1,0] = [2, 0]
  4 ≠ 2   → vi phạm tính cộng (Additivity), Mục 6
```

**Kiểm chứng Mục 7.2 — Hợp thành = Phép nhân Ma trận**, dùng hai ma trận xoay (Chương 3.2, Bài tập 5):

```text
T1 = xoay 30°,  T2 = xoay 45°
A1 = matrix_representation(T1, 2)
A2 = matrix_representation(T2, 2)

# Cách 1 — áp dụng tuần tự trên một vector x = (1, 0)
T1(T2(x))            →  [ 0.259, 0.966 ]

# Cách 2 — nhân hai ma trận trước, rồi áp dụng lên x
(A1 @ A2) @ x         →  [ 0.259, 0.966 ]   ✓ khớp hoàn toàn

# Và đúng như dự đoán, đây chính là ma trận xoay 75° (30°+45°)
matrix_representation(lambda v: rotate(v, 75), 2)  →  [ 0.259, 0.966 ]  ✓
```

Ba cách tính — hợp thành hàm số trực tiếp, nhân hai ma trận rồi áp dụng, và tính thẳng ma trận xoay $75°$ — cho **cùng một kết quả**, minh chứng thực nghiệm trực tiếp cho định lý ở Mục 7.2 (tổng quát hóa Bài tập 5 của Chương 3.2, khi đó chỉ kiểm chứng bằng công thức lượng giác).

**Kiểm tra độc lập tuyến tính:**

```text
np.linalg.matrix_rank([[1,2],[2,4]])   →  1   (2 vector, hạng chỉ 1 → PHỤ THUỘC)
np.linalg.matrix_rank([[1,0],[0,1]])   →  2   (2 vector, hạng đủ 2 → ĐỘC LẬP)
```

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Kết quả ở Mục 3 và Mục 7.2 — nhiều lớp tuyến tính liên tiếp gộp lại thành một lớp tuyến tính duy nhất — không phải một chi tiết lý thuyết trừu tượng. Đây là lý do kỹ thuật cụ thể, có thể kiểm chứng bằng code, giải thích một quyết định thiết kế nền tảng của mọi kiến trúc Deep Learning hiện đại.

| Bối cảnh công nghiệp | Vai trò của Phép biến đổi Tuyến tính |
|---|---|
| Kiến trúc Neural Network | Mỗi lớp fully-connected là một phép biến đổi affine; hàm kích hoạt phi tuyến (ReLU, sigmoid) bắt buộc phải xen giữa các lớp — nếu không, toàn bộ mạng dù sâu đến đâu cũng chỉ tương đương một phép biến đổi tuyến tính duy nhất (Mục 3, Mục 7.2) |
| Pipeline Đồ họa 3D (Model-View-Projection) | Nhiều phép biến đổi hình học liên tiếp được **gộp trước** thành một ma trận duy nhất bằng phép nhân ma trận (Mục 7.2), giúp GPU chỉ cần nhân mỗi đỉnh hình học với một ma trận thay vì nhiều ma trận riêng lẻ — tối ưu hiệu năng đáng kể |
| Trích chọn đặc trưng (Feature Selection) | Kiểm tra độc lập tuyến tính (Mục 8) giúp phát hiện và loại bỏ các đặc trưng dư thừa thông tin trước khi huấn luyện mô hình, tránh vấn đề đa cộng tuyến đã nêu ở Chương 3.3, Mục 11 |
| Xử lý tín hiệu số (Digital Signal Processing) | Bộ lọc tuyến tính (linear filter), ví dụ bộ lọc làm mịn (smoothing filter) trong xử lý âm thanh/ảnh, chính là các phép biến đổi tuyến tính theo đúng định nghĩa Mục 6 |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Ranh giới "tuyến tính so với phi tuyến" đã học trong chương này chính là câu hỏi trung tâm của một trong những định lý nền tảng nhất của lý thuyết Deep Learning hiện đại.

**Định lý Xấp xỉ Tổng quát (Universal Approximation Theorem)** — sẽ được trình bày đầy đủ ở Volume 5 — chứng minh rằng một mạng neural network với **ít nhất một hàm kích hoạt phi tuyến**, dù chỉ với một lớp ẩn, về mặt lý thuyết có thể xấp xỉ được *bất kỳ* hàm số liên tục nào với độ chính xác tùy ý. Điều kiện "phi tuyến" trong định lý này không phải ngẫu nhiên — nó chính xác là hệ quả trực tiếp của kết quả vừa chứng minh ở Mục 7.2: nếu mọi lớp đều tuyến tính, toàn mạng chỉ có thể biểu diễn được các phép biến đổi tuyến tính, một tập hàm số rất hạn chế (không thể biểu diễn cả một hàm đơn giản như $x^2$, như đã thấy ở Mục 10).

Song song đó, việc mở rộng khái niệm phép biến đổi tuyến tính sang không gian vô hạn chiều (Mục 2) là nền tảng của **phương pháp kernel (kernel methods)** trong Machine Learning cổ điển — kỹ thuật cho phép các thuật toán vốn chỉ xử lý được quan hệ tuyến tính (như Support Vector Machine) gián tiếp xử lý được cả quan hệ phi tuyến, bằng cách biến đổi dữ liệu sang một không gian khác trước khi áp dụng phép biến đổi tuyến tính — một chủ đề sẽ trở lại ở Volume 5.

---

## 13. Ưu điểm

- **Biểu diễn hữu hạn cho một họ phép biến đổi vô hạn** — Định lý Biểu diễn Ma trận (Mục 7.1) nghĩa là chỉ cần $n \times m$ con số để mô tả đầy đủ mọi phép biến đổi tuyến tính có thể có giữa $\mathbb{R}^n$ và $\mathbb{R}^m$.
- **Hợp thành cực kỳ đơn giản** — chỉ cần nhân ma trận (Mục 7.2), không cần suy luận lại từ định nghĩa hàm số gốc.
- **Toàn bộ công cụ phân tích từ Chương 3.3** (định thức, nghịch đảo, hạng) áp dụng trực tiếp, không cần công cụ mới.
- **Nền tảng cho tính toán song song hiệu quả** — vì mọi phép biến đổi tuyến tính quy về phép nhân ma trận, toàn bộ tối ưu hóa phần cứng đã có (Chương 3.2, Mục 11) áp dụng ngay lập tức.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Nhầm lẫn phổ biến nhất: cho rằng thêm nhiều lớp "tuyến tính" (hoặc affine, không có hàm kích hoạt phi tuyến xen giữa) sẽ làm mô hình mạnh hơn. Theo Mục 7.2 và Mục 12, điều này **hoàn toàn sai** — nhiều lớp tuyến tính liên tiếp luôn gộp lại thành một lớp tuyến tính duy nhất, không tăng thêm khả năng biểu diễn.

- **Không thể biểu diễn quan hệ phi tuyến** — theo đúng định nghĩa (Mục 6), không có ma trận nào có thể biểu diễn $x \mapsto x^2$ hay bất kỳ quan hệ cong nào; đây là giới hạn nền tảng, không phải thiếu sót kỹ thuật.
- **Thuật ngữ "tuyến tính" trong Machine Learning thường lỏng lẻo hơn định nghĩa toán học** (Mục 4, Common Mistake) — cần phân biệt rõ affine và linear khi đọc tài liệu chuyên ngành.
- **Kiểm tra độc lập tuyến tính bằng số học có thể không ổn định** với vector gần phụ thuộc tuyến tính — liên quan trực tiếp đến vấn đề ma trận gần suy biến đã nêu ở Chương 3.3, Mục 12.

---

## 15. So sánh

**Bảng 3.4.1 — Linear, Affine, và Nonlinear**

| Đặc điểm | Linear ($T(x)=Ax$) | Affine ($T(x)=Ax+b$) | Nonlinear (ví dụ $T(x)=x^2$) |
|---|---|---|---|
| Thỏa $T(\vec{0})=\vec{0}$? | Có | Chỉ khi $b=\vec{0}$ | Tùy hàm cụ thể |
| Thỏa tính cộng/thuần nhất (Mục 6)? | Có | Không (trừ khi $b=0$) | Không |
| Biểu diễn hình học | Xoay, co giãn, chiếu, cắt xiên — luôn giữ gốc tọa độ cố định | Như Linear, cộng thêm một phép **tịnh tiến (translation)** | Bẻ cong không gian — đường thẳng có thể thành đường cong |
| Ví dụ kỹ thuật | Ma trận xoay thuần túy trong đồ họa | Lớp `nn.Linear` trong PyTorch ($Wx+b$); công thức hồi quy tuyến tính | Hàm kích hoạt (ReLU, sigmoid); các quan hệ tự nhiên như $x^2$, $\sin(x)$ |
| Hợp thành nhiều lớp liên tiếp | Vẫn là Linear (Mục 7.2) | Vẫn là Affine (không tăng khả năng biểu diễn) | Có thể xấp xỉ hàm bất kỳ (Universal Approximation, Mục 12) |

**Phân tích:** dòng cuối cùng của bảng là kết luận quan trọng nhất của toàn chương, mang tính ứng dụng trực tiếp: cả Linear lẫn Affine, dù xếp chồng bao nhiêu lớp, đều **không** tăng khả năng biểu diễn — chỉ khi có ít nhất một thành phần Nonlinear xen vào, khả năng biểu diễn mới thực sự mở rộng. Đây là lý do kỹ thuật cốt lõi, không phải quy ước tùy tiện, giải thích vì sao mọi kiến trúc Deep Learning hiện đại đều bắt buộc có hàm kích hoạt phi tuyến.

---

## 16. Tóm tắt

- Một **phép biến đổi tuyến tính** $T$ thỏa hai điều kiện: tính cộng và tính thuần nhất (Mục 6) — kéo theo $T(\vec{0})=\vec{0}$, một phép kiểm tra nhanh loại trừ hữu ích.
- **Định lý Biểu diễn Ma trận:** mọi phép biến đổi tuyến tính hữu hạn chiều tương ứng duy nhất với một ma trận, xây dựng bằng cách áp dụng $T$ lên các vector cơ sở chuẩn (Mục 7.1).
- **Hợp thành hai phép biến đổi tuyến tính = nhân hai ma trận biểu diễn** (Mục 7.2) — hình thức hóa đầy đủ Insight đã gợi mở từ Chương 3.2.
- **Span**, **độc lập tuyến tính**, và **cơ sở (basis)** (Mục 6) cung cấp định nghĩa chính xác cho khái niệm hạng đã dùng trực giác ở Chương 3.3.
- Xếp chồng nhiều lớp tuyến tính (hoặc affine) liên tiếp **không** làm tăng khả năng biểu diễn — bắt buộc cần hàm phi tuyến để vượt qua giới hạn này (Mục 3, 12, 14, Bảng 3.4.1) — kiến thức nền tảng trực tiếp cho Volume 5.

Chương 3.5 (Systems of Linear Equations) sẽ dùng trực tiếp span và hạng vừa định nghĩa để trả lời câu hỏi: khi nào hệ phương trình $Ax=b$ có nghiệm, có nghiệm duy nhất, hay vô số nghiệm?

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Kiểm tra bằng định nghĩa Mục 6: hàm $T(x,y) = (x+y,\ x-y)$ có phải phép biến đổi tuyến tính không? Chỉ ra rõ bước kiểm tra tính cộng và tính thuần nhất.
2. Tìm ma trận biểu diễn của phép biến đổi $T(x,y,z) = (2x,\ y+z,\ 0)$, dùng phương pháp Mục 7.1 (áp dụng lên $\vec{e}_1,\vec{e}_2,\vec{e}_3$).
3. Ba vector $(1,0,0)$, $(0,1,0)$, $(0,0,1)$ (cơ sở chuẩn của $\mathbb{R}^3$) có độc lập tuyến tính không? Giải thích bằng định nghĩa Mục 6, không dùng thư viện.

### Mức Trung bình (Intermediate)

4. Chứng minh $T(x,y) = xy$ (nhân hai thành phần với nhau) **không** phải phép biến đổi tuyến tính, bằng cách tìm một phản ví dụ cụ thể vi phạm tính cộng hoặc tính thuần nhất — tương tự cách Mục 10 chứng minh $T_{\text{sq}}$ không tuyến tính.
5. Cho hai vector $\vec{v}_1=(1,2)$, $\vec{v}_2=(3,4)$. Chứng minh chúng độc lập tuyến tính bằng định nghĩa (giải phương trình $c_1\vec{v}_1+c_2\vec{v}_2=\vec{0}$ và chỉ ra nghiệm duy nhất là $c_1=c_2=0$).

### Mức Nâng cao (Advanced)

6. Cài đặt hàm `compose_transformations(T1, T2, n)` trả về ma trận biểu diễn của $T_1 \circ T_2$, dùng `matrix_representation` (Mục 9) và kiểm chứng kết quả khớp với $A_1 A_2$ (nhân trực tiếp hai ma trận biểu diễn riêng lẻ), tương tự thực nghiệm ở Mục 10 nhưng viết thành hàm tái sử dụng được.
7. Cho ba vector trong $\mathbb{R}^2$: $(1,0)$, $(0,1)$, $(2,3)$. Ba vector này có độc lập tuyến tính không? *(Gợi ý: một tập hơn 2 vector trong không gian 2 chiều luôn phụ thuộc tuyến tính — hãy giải thích tại sao bằng ngôn ngữ "span" ở Mục 6, liên hệ với Hình 3.4.2.)*

### Mức Nghiên cứu (Research)

8. Xây dựng thực nghiệm: tạo một mạng "giả" gồm 5 lớp tuyến tính liên tiếp (5 ma trận ngẫu nhiên nhân liên tiếp, không có hàm phi tuyến xen giữa) bằng NumPy. Tính ma trận tích của cả 5 lớp, rồi so sánh kết quả áp dụng "5 lớp riêng lẻ" với kết quả áp dụng "một ma trận gộp duy nhất" trên cùng một vector đầu vào ngẫu nhiên. Sau đó thử thêm hàm ReLU (`np.maximum(0, x)`) xen giữa mỗi lớp, và giải thích bằng lời tại sao lúc này việc "gộp thành một ma trận duy nhất" không còn thực hiện được nữa. Đây là bài tập mang tính khám phá thực nghiệm, minh chứng trực tiếp cho Mục 3, 12, 14 — không yêu cầu chứng minh hình thức bổ sung.

---

## 18. Dự án nhỏ

**Dự án: Bộ kiểm tra Tính Tuyến tính và Trực quan hóa Phép biến đổi 2D**

- **Mục tiêu:** xây dựng công cụ tổng hợp áp dụng toàn bộ chương — kiểm tra tính tuyến tính, tìm ma trận biểu diễn, và trực quan hóa hiệu ứng của phép biến đổi lên một lưới điểm, tái hiện trực quan Hình 3.4.1.
- **Yêu cầu:**
  1. Viết hàm `analyze_transformation(T, n)` nhận vào một hàm Python bất kỳ và số chiều $n$; trả về: có phải tuyến tính không (dùng `is_linear`, Mục 9), và nếu có, ma trận biểu diễn tương ứng (dùng `matrix_representation`).
  2. Thử nghiệm với ít nhất bốn hàm: một phép xoay (tuyến tính), một phép affine có cộng hằng số (không tuyến tính), một hàm bậc hai như $T_{\text{sq}}$ ở Mục 10 (không tuyến tính), và một phép chiếu xuống một trục (tuyến tính, ví dụ $T(x,y)=(x,0)$).
  3. Với các trường hợp tuyến tính, dùng `matplotlib` vẽ một lưới điểm 2D trước và sau biến đổi, tái hiện Hình 3.4.1.
- **Công nghệ đề xuất:** Python, NumPy, Matplotlib.
- **Kết quả kỳ vọng:** công cụ phân loại đúng cả bốn hàm thử nghiệm, cùng hình ảnh trực quan minh họa rõ ràng phép biến đổi tuyến tính giữ lưới ô vuông thẳng, đều đặn.
- **Mở rộng có thể:** mở rộng công cụ để tự động phát hiện và báo cáo phép biến đổi là affine (không tuyến tính nhưng có dạng $Ax+b$) thay vì chỉ báo "không tuyến tính" — bằng cách kiểm tra $T(x) - T(\vec{0})$ có tuyến tính hay không.

---

## 19. Tự đánh giá

- [ ] Tôi có thể phát biểu chính xác hai điều kiện định nghĩa phép biến đổi tuyến tính (Mục 6) và áp dụng chúng để kiểm tra một hàm cụ thể.
- [ ] Tôi có thể xây dựng ma trận biểu diễn của một phép biến đổi tuyến tính bằng cách áp dụng nó lên các vector cơ sở chuẩn.
- [ ] Tôi hiểu và có thể giải thích vì sao hợp thành hai phép biến đổi tuyến tính tương ứng chính xác với phép nhân hai ma trận biểu diễn (Mục 7.2).
- [ ] Tôi phân biệt được Linear, Affine, và Nonlinear (Bảng 3.4.1), và có thể giải thích vì sao thuật ngữ "linear layer" trong Machine Learning thường thực ra là affine.
- [ ] Tôi có thể giải thích, bằng ngôn ngữ kỹ thuật, vì sao xếp chồng nhiều lớp tuyến tính không làm tăng khả năng biểu diễn của một mô hình — và vì sao cần hàm kích hoạt phi tuyến.
- [ ] Tôi kiểm tra được tính độc lập tuyến tính của một tập vector, cả bằng tay (vector nhỏ) lẫn bằng NumPy (`matrix_rank`).
- [ ] Tôi đã hoàn thành Dự án nhỏ ở Mục 18, phân loại đúng cả các trường hợp tuyến tính và không tuyến tính.

Nếu Bài tập 8 (thực nghiệm gộp 5 lớp tuyến tính) khiến khái niệm ở Mục 12/14 trở nên rõ ràng hơn nhiều so với chỉ đọc lý thuyết, hãy ghi nhớ trực giác này — nó sẽ là nền tảng trực tiếp khi Volume 5 giải thích vì sao kiến trúc Deep Learning được thiết kế như hiện tại.

---

## 20. Đọc thêm

- **Sách nâng cao (khuyến nghị chính cho chương này):** Sheldon Axler, *Linear Algebra Done Right* — cuốn sách này xây dựng toàn bộ đại số tuyến tính bắt đầu từ phép biến đổi tuyến tính (thay vì từ ma trận), cách tiếp cận rất gần với tinh thần của chương này; đặc biệt phù hợp để đào sâu Mục 6–7.
- **Sách:** Marc Peter Deisenroth, A. Aldo Faisal, Cheng Soon Ong, *Mathematics for Machine Learning*, Chương 2, phần về ánh xạ tuyến tính. *(Xem BOOKS.md — Volume 1.)*
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc thêm về Universal Approximation Theorem (nhắc ở Mục 12) — bản chứng minh đầy đủ đòi hỏi kiến thức Giải tích (Volume 1, Part IV) nên phù hợp hơn để quay lại sau khi hoàn thành Part IV.
- **Chương tiếp theo:** Chương 3.5 — Systems of Linear Equations.

---

### Liên kết chương (Cross References)

- **Chương trước:** 3.3 — Matrix Operations (định thức, nghịch đảo, hạng — chương này hình thức hóa chính xác khái niệm hạng qua span và độc lập tuyến tính).
- **Chương tiếp theo:** 3.5 — Systems of Linear Equations (dùng trực tiếp span, hạng, và độc lập tuyến tính để phân tích tính giải được của $Ax=b$).
- **Chương liên quan xa hơn:** 3.6 — Eigenvalues and Eigenvectors (dùng khái niệm cơ sở và span để định nghĩa không gian riêng — eigenspace); Volume 5 — Artificial Intelligence (Universal Approximation Theorem, Mục 12; vai trò của hàm kích hoạt phi tuyến trong kiến trúc neural network).
- **Vị trí trong Knowledge Graph:** Nút thứ tư của Part III, phụ thuộc trực tiếp vào Chương 3.1–3.3; là điều kiện tiên quyết trực tiếp cho Chương 3.5 và 3.6, đồng thời là cầu nối khái niệm quan trọng nhất giữa Part III và Volume 5.

---

*Hết Chương 3.4. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer, tiếp nối trực tiếp Chương 3.3 theo outline Part III đã đề xuất. Chương này hình thức hóa đầy đủ trực giác "ma trận là phép biến đổi" đã dùng xuyên suốt từ Chương 3.2, chứng minh Định lý Biểu diễn Ma trận, và giải thích chính xác bằng cả lý thuyết lẫn thực nghiệm code vì sao Deep Learning cần hàm phi tuyến. Đang chờ rà soát trước khi tiếp tục sang Chương 3.5 — Systems of Linear Equations.*
