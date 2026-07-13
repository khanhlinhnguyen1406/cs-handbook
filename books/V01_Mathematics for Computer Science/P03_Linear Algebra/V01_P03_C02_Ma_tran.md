# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part III — Linear Algebra
## Chương 3.2 — Ma trận
### (Matrices)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 3.2 |
| Thuộc Part | III — Linear Algebra |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 50–60 phút |
| Độ khó | ★★★☆☆ |
| Kiến thức tiên quyết | Chương 3.1 — Vectors (bắt buộc — ma trận được xây dựng trực tiếp trên vector) |
| Chương liên quan | 3.3 — Matrix Operations (nghịch đảo, định thức, hạng); 3.4 — Linear Transformations (ma trận như một hàm số); Volume 5 — Artificial Intelligence (lớp fully-connected là phép nhân ma trận $Wx+b$) |
| Từ khóa | matrix, matrix addition, scalar multiplication, matrix multiplication, dimension compatibility, row vector, column vector, Hadamard product |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Định nghĩa ma trận như một mảng số hai chiều, đồng thời như một **tập hợp có thứ tự các vector** — kết nối trực tiếp với Chương 3.1.
- Thực hiện phép cộng ma trận và phép nhân vô hướng ma trận.
- Thực hiện phép nhân ma trận đúng quy tắc, giải thích điều kiện tương thích về chiều, và giải thích *tại sao* phép nhân ma trận lại được định nghĩa theo cách "hàng nhân cột" thay vì nhân từng phần tử.
- Diễn giải một ma trận như một **phép biến đổi (transformation)** tác động lên vector, thay vì chỉ là một bảng số tĩnh.
- Cài đặt phép nhân ma trận trong Python (thủ công + NumPy), đo và giải thích chênh lệch hiệu năng.

---

### Câu hỏi khơi gợi

> *Vì sao phép nhân ma trận lại được định nghĩa theo một cách có vẻ phức tạp và khó nhớ — "lấy hàng nhân cột rồi cộng lại" — thay vì đơn giản nhân từng cặp phần tử tương ứng như phép cộng? Câu trả lời không nằm ở sự tùy tiện của các nhà toán học thế kỷ 19, mà hé lộ một trong những ý tưởng đẹp và mạnh mẽ nhất của đại số tuyến tính: nhân ma trận, về bản chất, không phải là một phép toán số học — nó là một phép **hợp thành hàm số (function composition)**.*

---

## 1. Tổng quan chương

Chương 3.1 xây dựng vector — đối tượng biểu diễn một điểm dữ liệu hoặc một hướng trong không gian. Chương này mở rộng ý tưởng đó theo hướng tự nhiên nhất có thể: nếu một vector là *một* danh sách số, thì **ma trận (matrix)** là một *tập hợp có tổ chức* nhiều vector — xếp thành hàng và cột.

Nhưng ý nghĩa thực sự của ma trận vượt xa một bảng số đơn thuần. Chương này sẽ cho thấy ma trận còn có một vai trò thứ hai, sâu sắc hơn: **một ma trận là một hàm số** — một cỗ máy nhận vector đầu vào và trả về vector đầu ra đã bị biến đổi. Góc nhìn thứ hai này chính là nền tảng trực tiếp cho Chương 3.4 (Linear Transformations) và toàn bộ cách mạng lưới neural network (Volume 5) xử lý dữ liệu.

> **💡 Insight**
> Nếu bạn từng làm việc với `pandas.DataFrame` (hàng là mẫu dữ liệu, cột là đặc trưng) hoặc một ảnh RGB (một lưới điểm ảnh hai chiều), bạn đã làm việc với ma trận. Chương này cho bạn một góc nhìn toán học mạnh hơn cho cấu trúc dữ liệu quen thuộc đó: không chỉ là "bảng số", mà là "phép biến đổi".

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1850 | James Joseph Sylvester | Đặt ra thuật ngữ **"matrix"** (tiếng Latin nghĩa là "tử cung", "nguồn gốc") — mô tả một bảng số là "nguồn" sinh ra các định thức con |
| 1858 | Arthur Cayley | Công bố *A Memoir on the Theory of Matrices* — hình thức hóa đầy đủ đại số ma trận, bao gồm cả **phép nhân ma trận** đúng như định nghĩa dùng ngày nay (Mục 7.1) |
| 1925 | Werner Heisenberg | Phát triển **cơ học ma trận (matrix mechanics)** — một trong hai công thức toán học đầu tiên của cơ học lượng tử, cho thấy đại số ma trận có sức mạnh mô tả cả các hiện tượng vật lý trừu tượng nhất |
| 1969 | Volker Strassen | Công bố thuật toán nhân ma trận nhanh hơn phương pháp truyền thống — mở đầu một nhánh nghiên cứu vẫn tiếp diễn đến ngày nay (Mục 12) |
| 2006–nay | NVIDIA, Google (TPU) | Phát triển phần cứng chuyên dụng (GPU, TPU) được thiết kế gần như riêng cho phép nhân ma trận hàng loạt — hạ tầng tính toán trực tiếp cho cuộc cách mạng Deep Learning |

Điều thú vị: Cayley hình thức hóa đại số ma trận vào năm 1858 chủ yếu vì lý do lý thuyết thuần túy (liên quan đến phép biến đổi tuyến tính trong hình học). Ông không thể hình dung được rằng gần 170 năm sau, các công ty công nghệ lớn nhất thế giới sẽ chi hàng chục tỷ đô-la để thiết kế chip máy tính chuyên biệt chỉ để thực hiện đúng một phép toán mà ông định nghĩa: nhân hai ma trận.

---

## 3. Động lực

Ba tình huống kỹ thuật tiếp nối trực tiếp từ Chương 3.1:

- Ở Chương 3.1, mỗi người dùng là **một** feature vector. Nhưng toàn bộ tập dữ liệu 10.000 người dùng — mỗi người 5 đặc trưng — tự nhiên trở thành một bảng số $10000 \times 5$: đây chính là một **ma trận**.
- Một layer trong mạng neural network nhận vào một vector đầu vào $x$ (ví dụ: ảnh đã "duỗi thẳng", Chương 3.1 Mục 3), và tính $y = Wx + b$ — trong đó $W$ là một ma trận trọng số (weight matrix). Toàn bộ "tri thức" mà mạng học được nằm trong các con số của ma trận $W$ này.
- Trong đồ họa máy tính (game, hoạt hình 3D), xoay một hình ảnh 2D hay 3D quanh một tâm điểm được thực hiện bằng cách nhân tọa độ mỗi điểm với một **ma trận xoay (rotation matrix)** cố định.

Cả ba tình huống có chung một cấu trúc: một ma trận vừa **lưu trữ dữ liệu có tổ chức**, vừa (khi nhân với vector) **biến đổi dữ liệu đó**. Đây chính là hai chủ đề chính của chương này.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Một ma trận giống như một **cỗ máy biến hình (transformation machine)**: bạn đưa một vector vào một đầu, cỗ máy xử lý, và một vector khác (có thể bị xoay, kéo dãn, nén, hoặc chiếu xuống chiều thấp hơn) xuất hiện ở đầu kia. Nhìn một bảng số tĩnh là đúng, nhưng chưa đủ — sức mạnh thực sự nằm ở việc ma trận đó *làm gì* khi bạn nhân nó với một vector.

| Trực giác kỹ thuật bạn đã có | Khái niệm toán học tương ứng |
|---|---|
| Mảng hai chiều `const A = [[1,2],[3,4]]` | Ma trận $A \in \mathbb{R}^{2\times2}$ |
| `pandas.DataFrame` với $m$ hàng (mẫu), $n$ cột (đặc trưng) | Ma trận $A \in \mathbb{R}^{m\times n}$ |
| Áp dụng một hàm biến đổi tọa độ (ví dụ trong CSS `transform: matrix(...)`) | Phép nhân ma trận với vector: $A\vec{x}$ |
| Một layer của neural network (`nn.Linear` trong PyTorch) | Phép biến đổi affine $y = Wx + b$ |

---

## 5. Trực quan hóa khái niệm

**Hình 3.2.1 — Ma trận như một Lưới số có Chiều $m \times n$**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
                cột 1  cột 2  cột 3
              ┌──────┬──────┬──────┐
      hàng 1  │  a11 │  a12 │  a13 │
              ├──────┼──────┼──────┤
      hàng 2  │  a21 │  a22 │  a23 │
              └──────┴──────┴──────┘

              A ∈ ℝ^(2×3)  →  2 hàng, 3 cột
              a_ij = phần tử ở hàng i, cột j
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Thiết lập quy ước ký hiệu: chỉ số hàng luôn viết trước, chỉ số cột viết sau ($a_{ij}$ = hàng $i$, cột $j$); chiều ma trận luôn viết "số hàng × số cột" |
| Điểm mấu chốt | Đây là quy ước sẽ dùng xuyên suốt Handbook — nhầm lẫn thứ tự hàng/cột là lỗi phổ biến nhất khi mới học đại số tuyến tính (xem Mục 14) |

---

**Hình 3.2.2 — Ma trận như Phép biến đổi: một hình vuông bị "kéo nghiêng"**

```text
   Trước khi nhân với A            Sau khi nhân với A
   (hình vuông đơn vị)             (hình bình hành)

   y                                y
   │                                │      ╱
 1 ●───●                          2 ┤    ╱
   │   │                            │  ╱
   │   │                          1 ┤╱────●
   ●───●──► x                       │      │
   0   1                          0 ●──────●──► x
                                    0  1  2
```

*Mục đích:* Cho thấy trực quan ý nghĩa của "ma trận như phép biến đổi" — bốn đỉnh của hình vuông đơn vị, sau khi nhân với ma trận $A$, di chuyển đến vị trí mới, biến hình vuông thành hình bình hành. *Điểm mấu chốt:* đây chính xác là cách các lớp trong neural network "bóp méo" không gian dữ liệu đầu vào ở mỗi bước xử lý — ý tưởng sẽ được đào sâu ở Chương 3.4 và Volume 5.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Ma trận (Matrix)**
>
> Một **ma trận** $A$ kích thước $m \times n$ là một bảng số hình chữ nhật gồm $m$ hàng và $n$ cột, ký hiệu $A \in \mathbb{R}^{m\times n}$. Phần tử ở hàng $i$, cột $j$ được ký hiệu $a_{ij}$ (hoặc $A_{ij}$). Khi $m = n$, ma trận được gọi là **ma trận vuông (square matrix)**.
>
> Một ma trận có thể được nhìn như một tập hợp có thứ tự các **vector cột (column vector)** — mỗi cột là một vector chiều $m$ — hoặc tương đương, một tập hợp có thứ tự các **vector hàng (row vector)**. Một vector (Chương 3.1) chính là trường hợp đặc biệt của ma trận khi $n=1$ (vector cột) hoặc $m=1$ (vector hàng).

**Các phép toán cơ bản** — cho hai ma trận $A, B \in \mathbb{R}^{m\times n}$ cùng chiều, và scalar $k$:

| Ký hiệu | Tên gọi | Định nghĩa (theo từng phần tử) |
|---|---|---|
| $A + B$ | Phép cộng ma trận (Matrix Addition) | $(A+B)_{ij} = a_{ij} + b_{ij}$ — chỉ hợp lệ khi $A, B$ **cùng chiều** |
| $kA$ | Phép nhân vô hướng (Scalar Multiplication) | $(kA)_{ij} = k \cdot a_{ij}$ |
| $A^\top$ | Chuyển vị (Transpose) — xem đầy đủ ở Chương 3.3 | $(A^\top)_{ij} = a_{ji}$ — đổi hàng thành cột |

Phép cộng ma trận và nhân vô hướng ma trận **hoàn toàn tương tự** phép toán trên vector đã học ở Chương 3.1 — không có gì mới về mặt khái niệm, chỉ là áp dụng cho một lưới số hai chiều thay vì một chiều.

---

## 7. Nền tảng toán học

### 7.1 Phép nhân Ma trận (Matrix Multiplication)

Đây là phép toán quan trọng nhất của chương, và là phép toán duy nhất **không** đơn giản là "làm theo từng phần tử".

> **📦 Formula Box — Phép nhân Ma trận**
>
> $$C = AB, \quad \text{với } A \in \mathbb{R}^{m\times k},\ B \in \mathbb{R}^{k\times n},\ C \in \mathbb{R}^{m\times n}$$
>
> $$c_{ij} = \sum_{p=1}^{k} a_{ip}\, b_{pj} = (\text{hàng } i \text{ của } A) \cdot (\text{cột } j \text{ của } B)$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | Điều kiện chiều | Số **cột** của $A$ phải bằng số **hàng** của $B$ (cả hai đều bằng $k$) — nếu không, phép nhân **không xác định** |
> | $c_{ij}$ | Phần tử hàng $i$, cột $j$ của kết quả — chính là **tích vô hướng (Chương 3.1, Mục 7.1)** giữa vector hàng $i$ của $A$ và vector cột $j$ của $B$ |
> | **Diễn giải kỹ thuật** | Mỗi phần tử của ma trận kết quả là một phép tích vô hướng — phép nhân ma trận, về bản chất, là $m \times n$ phép tích vô hướng được tổ chức lại thành một bảng số |
> | **Ứng dụng thường gặp** | $y = Wx$ trong mọi lớp neural network; nhân hai ma trận biến đổi (rotation, scaling) để tạo ra một phép biến đổi tổng hợp trong đồ họa máy tính |

**Ví dụ tay:** $A = \begin{pmatrix}1 & 2\\3 & 4\end{pmatrix}$, $B = \begin{pmatrix}5 & 6\\7 & 8\end{pmatrix}$.

$c_{11} = (1)(5) + (2)(7) = 19 \qquad c_{12} = (1)(6)+(2)(8) = 22$
$c_{21} = (3)(5)+(4)(7) = 43 \qquad c_{22} = (3)(6)+(4)(8) = 50$

$$AB = \begin{pmatrix}19 & 22\\43 & 50\end{pmatrix}$$

### 7.2 Vì sao lại định nghĩa "kỳ lạ" như vậy?

> **💡 Insight**
> Câu trả lời đầy đủ, chặt chẽ nằm ở Chương 3.4 (Linear Transformations), nhưng trực giác cốt lõi có thể nắm bắt ngay bây giờ: **nhân ma trận được định nghĩa đúng như vậy để tương ứng với việc áp dụng liên tiếp hai phép biến đổi** — nếu ma trận $B$ biến đổi vector $x$ thành $Bx$, rồi ma trận $A$ biến đổi tiếp kết quả đó thành $A(Bx)$, thì kết quả đó **luôn luôn** bằng $(AB)x$ — nhân hai ma trận trước, rồi mới nhân với vector. Nói cách khác, phép nhân ma trận chính là phép **hợp thành hàm số (function composition)**, viết dưới dạng bảng số. Đây không phải là một quy ước tùy tiện — nó là hệ quả tất yếu nếu muốn "nhân ma trận" mô tả đúng "làm hai phép biến đổi liên tiếp".

---

## 8. Thuật toán / Cơ chế

**Thuật toán nhân ma trận truyền thống (Naive Matrix Multiplication):**

```text
Bước 1 — Nhận vào A (m×k) và B (k×n); kiểm tra điều kiện
         số cột của A = số hàng của B
        │
        ▼
Bước 2 — Khởi tạo ma trận kết quả C (m×n), mọi phần tử = 0
        │
        ▼
Bước 3 — Với mỗi hàng i từ 1 đến m:
        │
        ▼
Bước 4 —   Với mỗi cột j từ 1 đến n:
        │
        ▼
Bước 5 —     Với mỗi chỉ số p từ 1 đến k:
        │
        ▼
Bước 6 —       C[i][j] += A[i][p] * B[p][j]
        │
        ▼
Bước 7 — Sau khi duyệt hết, C chính là AB
```

> **⚠️ Common Mistake**
> Ba vòng lặp lồng nhau (Bước 3–6) cho thấy trực tiếp độ phức tạp: nhân hai ma trận vuông $n\times n$ theo cách truyền thống tốn $O(n^3)$ phép toán — **không phải** $O(n^2)$ như nhiều người mới học nhầm lẫn (dễ nhầm vì kết quả chỉ có $n^2$ phần tử, nhưng *mỗi* phần tử tốn $O(n)$ phép tính theo Bước 5–6).

---

## 9. Triển khai

```python
import numpy as np
import time

def matmul_manual(A, B):
    """Nhân hai ma trận bằng vòng lặp Python thuần,
    triển khai trực tiếp thuật toán ở Mục 8."""
    m, k = len(A), len(A[0])
    k2, n = len(B), len(B[0])
    assert k == k2, "Số cột của A phải bằng số hàng của B"

    C = [[0] * n for _ in range(m)]
    for i in range(m):
        for j in range(n):
            for p in range(k):
                C[i][j] += A[i][p] * B[p][j]
    return C


# Kiểm chứng với ví dụ tay ở Mục 7.1
A = [[1, 2], [3, 4]]
B = [[5, 6], [7, 8]]
print(matmul_manual(A, B))        # [[19, 22], [43, 50]]
print(np.array(A) @ np.array(B))  # [[19 22] [43 50]]
```

Hàm `matmul_manual` là bản dịch trực tiếp thuật toán ở Mục 8 sang Python thuần — ba vòng lặp lồng nhau tương ứng chính xác với Bước 3–6. Trong thực tế, không ai dùng cách này cho ma trận lớn; NumPy (toán tử `@`, hoặc `np.matmul`) gọi thư viện BLAS đã tối ưu hóa sâu ở tầng phần cứng.

---

## 10. Trực quan hóa quá trình thực thi

**So sánh tốc độ: Python thuần vs. NumPy**, nhân hai ma trận vuông $500\times500$:

| Cách triển khai | Thời gian thực thi (ước lượng) | Ghi chú |
|---|---:|---|
| `matmul_manual` (ba vòng lặp Python) | ≈ 25–30 giây | $500^3 = 125.000.000$ phép nhân-cộng, mỗi phép có overhead của Python |
| `A @ B` (NumPy) | ≈ 10–15 ms | Nhanh hơn khoảng **2.000 lần** — BLAS dùng thuật toán tối ưu bộ nhớ đệm (cache-aware) và xử lý song song |

**Kiểm chứng tính không giao hoán** ($AB \neq BA$ nói chung — xem thêm Mục 14):

```text
A = [[1, 2], [0, 1]]
B = [[1, 0], [3, 1]]

A @ B  →  [[7, 2], [3, 1]]
B @ A  →  [[1, 2], [3, 7]]

A @ B == B @ A  →  False
```

Hai kết quả khác nhau hoàn toàn — đây là bằng chứng thực nghiệm trực tiếp cho tính chất được nêu ở Mục 14: thứ tự nhân ma trận **quan trọng**, khác hẳn phép nhân số thông thường.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Phép nhân ma trận là phép toán được thực hiện nhiều nhất trong toàn bộ ngành công nghiệp AI hiện đại — phần lớn ngân sách phần cứng của các công ty AI lớn được chi để tăng tốc chính xác phép toán này.

| Bối cảnh công nghiệp | Vai trò của Phép nhân Ma trận |
|---|---|
| Lớp Fully-Connected trong Neural Network | $y = Wx + b$ — toàn bộ quá trình "suy luận" của một mạng neural network, ở mỗi lớp, là một phép nhân ma trận |
| Đồ họa máy tính & Game Engine | Xoay, co giãn, chiếu phối cảnh (perspective projection) 3D đều được biểu diễn bằng ma trận biến đổi; hợp nhiều phép biến đổi thành một bằng cách nhân các ma trận với nhau (Mục 7.2) |
| Phân rã Ma trận trong Hệ gợi ý (Matrix Factorization) | Ma trận tương tác người dùng–sản phẩm được phân rã thành tích của hai ma trận nhỏ hơn — nền tảng của các hệ gợi ý cổ điển (trước kỷ nguyên Deep Learning) |
| GPU / TPU | Kiến trúc phần cứng được thiết kế với các đơn vị tính toán chuyên biệt (Tensor Core) tối ưu riêng cho phép nhân ma trận hàng loạt |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Một câu hỏi tưởng chừng đã "đóng" từ thế kỷ 19 — "cách nhanh nhất để nhân hai ma trận là gì?" — thực ra vẫn là một bài toán nghiên cứu mở, sôi động cho đến tận ngày nay.

Thuật toán truyền thống (Mục 8) tốn $O(n^3)$ cho ma trận vuông $n\times n$. Năm 1969, **Volker Strassen** gây chấn động cộng đồng khi chứng minh có thể làm nhanh hơn — thuật toán của ông đạt độ phức tạp khoảng $O(n^{2.807})$, bằng một mẹo đại số giảm số phép nhân con cần thiết. Đây mở đầu một chuỗi cải tiến kéo dài hơn nửa thế kỷ, với các thuật toán ngày càng nhanh hơn về mặt lý thuyết (dù thường khó áp dụng thực tế với ma trận kích thước thông thường).

Đáng chú ý hơn: năm 2022, nhóm nghiên cứu **AlphaTensor** của DeepMind dùng học tăng cường (Reinforcement Learning — sẽ gặp ở Volume 5) để **tự động khám phá** các thuật toán nhân ma trận nhanh hơn cả những thuật toán do con người thiết kế trong nhiều trường hợp cụ thể — một ví dụ ấn tượng về việc AI hiện đại quay lại cải thiện chính nền tảng toán học làm nên nó.

---

## 13. Ưu điểm

- **Biểu diễn cực kỳ cô đọng** cho cả tập dữ liệu lớn lẫn phép biến đổi hình học phức tạp — một dòng $y = Wx$ thay thế hàng nghìn phép tính riêng lẻ.
- **Cho phép tính toán hàng loạt (batch computation)** — biến đổi toàn bộ tập dữ liệu (nhiều mẫu cùng lúc) chỉ bằng một phép nhân ma trận duy nhất, thay vì lặp qua từng mẫu.
- **Hệ sinh thái phần cứng và phần mềm tối ưu hóa sâu** (BLAS, GPU, TPU) — hiếm có phép toán toán học nào được đầu tư tối ưu hóa công nghiệp nhiều đến vậy.
- **Là ngôn ngữ tự nhiên cho phép hợp thành biến đổi** (Mục 7.2) — nhân nhiều ma trận tương đương với áp dụng nhiều phép biến đổi liên tiếp, chỉ bằng một phép tính.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Sai lầm phổ biến nhất khi mới học đại số tuyến tính: mặc định phép nhân ma trận **giao hoán** giống phép nhân số thông thường. **Nói chung, $AB \neq BA$** — đã kiểm chứng bằng thực nghiệm ở Mục 10. Thứ tự nhân ma trận luôn phải được tôn trọng nghiêm ngặt.

- **Chi phí tính toán tăng theo bậc ba** ($O(n^3)$ với thuật toán truyền thống) — với ma trận rất lớn (hàng chục nghìn chiều, phổ biến trong Deep Learning hiện đại), đây là điểm nghẽn tính toán chính, thúc đẩy toàn bộ ngành công nghiệp phần cứng AI (Mục 11–12).
- **Không giao hoán** ($AB \neq BA$) — khác với trực giác về phép nhân số thông thường, dễ gây lỗi khi mới chuyển từ tư duy đại số phổ thông sang đại số tuyến tính.
- **Yêu cầu tương thích chiều nghiêm ngặt** — số cột của ma trận trái phải bằng số hàng của ma trận phải; đây là lỗi runtime phổ biến nhất khi lập trình với NumPy/PyTorch (`ValueError: shapes not aligned`).
- **Tiêu tốn bộ nhớ đáng kể** với ma trận lớn và dày đặc (dense) — các kỹ thuật ma trận thưa (sparse matrix) tồn tại riêng để giải quyết vấn đề này, nằm ngoài phạm vi chương này.

---

## 15. So sánh

**Bảng 3.2.1 — Phép nhân Ma trận (Matrix Multiplication) so với Phép nhân theo từng phần tử (Hadamard Product)**

| Đặc điểm | Matrix Multiplication ($AB$) | Hadamard Product ($A \odot B$) |
|---|---|---|
| Định nghĩa | $c_{ij} = \sum_p a_{ip}b_{pj}$ (hàng nhân cột) | $c_{ij} = a_{ij} \cdot b_{ij}$ (từng phần tử) |
| Điều kiện chiều | Số cột $A$ = số hàng $B$ | $A, B$ phải **cùng chiều tuyệt đối** |
| Ý nghĩa | Hợp thành phép biến đổi (Mục 7.2) | Điều chỉnh/lọc từng phần tử độc lập (ví dụ: áp dụng mặt nạ — mask) |
| Tính giao hoán | Không (Mục 14) | Có ($A \odot B = B \odot A$) |
| Ứng dụng tiêu biểu | Lớp fully-connected, phép biến đổi hình học | Cơ chế cổng (gate) trong LSTM (Volume 6); attention mask trong Transformer |

**Phân tích:** đây là một trong những nhầm lẫn phổ biến nhất khi đọc code Deep Learning lần đầu — ký hiệu $\odot$ (hoặc `*` trong NumPy khi hai mảng cùng chiều) và ký hiệu $\times$/`@` (hoặc `matmul`) trông có vẻ "làm việc tương tự" nhưng là hai phép toán có ý nghĩa hoàn toàn khác nhau. Nhầm lẫn giữa hai toán tử này trong PyTorch/NumPy là một trong những lỗi runtime — hoặc tệ hơn, lỗi *âm thầm không báo lỗi* — phổ biến nhất khi triển khai kiến trúc neural network, sẽ được nhắc lại cụ thể ở Volume 5.

---

## 16. Tóm tắt

- Một **ma trận** $A \in \mathbb{R}^{m\times n}$ là bảng số $m$ hàng, $n$ cột; có thể nhìn như một tập hợp có thứ tự các vector cột (hoặc hàng) — mở rộng trực tiếp từ Chương 3.1.
- **Phép cộng** và **nhân vô hướng** ma trận hoàn toàn tương tự vector — áp dụng theo từng phần tử.
- **Phép nhân ma trận** $C=AB$ yêu cầu số cột của $A$ bằng số hàng của $B$; mỗi phần tử kết quả là một **tích vô hướng** giữa hàng của $A$ và cột của $B$ — về bản chất là phép **hợp thành hàm số**, không phải phép nhân từng phần tử.
- Phép nhân ma trận **không giao hoán** ($AB \neq BA$ nói chung) và tốn $O(n^3)$ với thuật toán truyền thống — cả hai đều là nguồn gốc của lỗi phổ biến khi lập trình (Mục 14, Mục 15).
- Ma trận không chỉ là bảng số — nó là một **phép biến đổi**, ý tưởng sẽ được hình thức hóa đầy đủ ở Chương 3.4 (Linear Transformations), sau khi Chương 3.3 trang bị thêm các phép toán ma trận còn lại (nghịch đảo, định thức, hạng).

Chương 3.3 (Matrix Operations) sẽ dùng trực tiếp phép nhân ma trận vừa học để định nghĩa ma trận nghịch đảo: $A^{-1}$ là ma trận sao cho $AA^{-1} = I$ (ma trận đơn vị) — một khái niệm không có ý nghĩa nếu chưa hiểu phép nhân ma trận ở chương này.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Cho $A = \begin{pmatrix}2&0\\1&3\end{pmatrix}$, $B = \begin{pmatrix}1&4\\2&1\end{pmatrix}$. Tính $A+B$ và $2A$.
2. Cho $A \in \mathbb{R}^{3\times4}$ và $B \in \mathbb{R}^{4\times2}$. $AB$ có xác định không? Nếu có, kích thước của $AB$ là bao nhiêu? $BA$ có xác định không? *(Gợi ý: áp dụng đúng điều kiện chiều ở Mục 7.1.)*
3. Tính tay $AB$ với $A = \begin{pmatrix}1&0\\0&1\end{pmatrix}$ (ma trận đơn vị $2\times2$) và $B$ bất kỳ ma trận $2\times2$ bạn tự chọn. Bạn quan sát được điều gì?

### Mức Trung bình (Intermediate)

4. Chứng minh phép cộng ma trận có tính giao hoán: $A + B = B + A$, dùng định nghĩa theo từng phần tử ở Mục 6. *(Gợi ý: tương tự bài tập chứng minh giao hoán của tích vô hướng ở Chương 3.1.)*
5. Cho hai ma trận xoay 2D góc $\theta$: $R(\theta) = \begin{pmatrix}\cos\theta & -\sin\theta\\ \sin\theta & \cos\theta\end{pmatrix}$. Tính $R(\theta_1) R(\theta_2)$ bằng tay (dùng công thức lượng giác cộng góc) và so sánh với $R(\theta_1+\theta_2)$. Kết quả này minh họa trực tiếp ý tưởng nào ở Mục 7.2?

### Mức Nâng cao (Advanced)

6. Cài đặt hàm `matmul_manual` (Mục 9) sao cho báo lỗi rõ ràng (thay vì lỗi khó hiểu) khi chiều hai ma trận không tương thích. So sánh thông báo lỗi của bạn với thông báo lỗi thực tế của NumPy khi nhân hai ma trận sai chiều.
7. Tìm hai ma trận vuông $2\times2$ khác không, cả hai đều khác ma trận không, sao cho tích của chúng **bằng ma trận không**. *(Gợi ý: đây là một tính chất "kỳ lạ" khác của đại số ma trận — không tồn tại trong đại số số thông thường, nơi $ab=0$ luôn kéo theo $a=0$ hoặc $b=0$; hãy thử với các ma trận có nhiều số 0.)*

### Mức Nghiên cứu (Research)

8. Đo thời gian thực thi thực tế của `matmul_manual` so với `A @ B` (NumPy) cho các ma trận vuông kích thước $50, 100, 200, 400$. Vẽ biểu đồ thời gian theo kích thước cho cả hai cách triển khai. Tốc độ tăng của `matmul_manual` có khớp với dự đoán $O(n^3)$ ở Mục 8 không? Đây là bài tập mang tính khám phá thực nghiệm — không yêu cầu chứng minh hình thức, chỉ yêu cầu quan sát và đối chiếu với lý thuyết.

---

## 18. Dự án nhỏ

**Dự án: Trình biến đổi hình học 2D (2D Shape Transformer)**

- **Mục tiêu:** áp dụng trực tiếp phép nhân ma trận-vector để xoay, co giãn, và tổ hợp các phép biến đổi lên một hình 2D đơn giản — minh họa cụ thể Hình 3.2.2 và Mục 7.2.
- **Yêu cầu:**
  1. Định nghĩa một hình đơn giản bằng danh sách các điểm 2D (ví dụ: 4 đỉnh của một hình vuông, hoặc nhiều điểm hơn cho một hình phức tạp).
  2. Cài đặt ba ma trận biến đổi: ma trận xoay $R(\theta)$ (Bài tập 5), ma trận co giãn (scaling) $\text{diag}(s_x, s_y)$, và ma trận phản chiếu (reflection) qua trục $x$.
  3. Áp dụng từng phép biến đổi riêng lẻ lên tập điểm, sau đó thử **hợp hai phép biến đổi** bằng cách nhân hai ma trận trước rồi mới nhân với các điểm — kiểm chứng kết quả giống hệt việc áp dụng tuần tự.
  4. Vẽ hình trước và sau biến đổi bằng `matplotlib` để quan sát trực quan.
- **Công nghệ đề xuất:** Python, NumPy, Matplotlib.
- **Kết quả kỳ vọng:** hình ảnh trực quan cho thấy hình dạng bị xoay/co giãn đúng như kỳ vọng, và bằng chứng số học rằng "nhân ma trận trước" cho kết quả giống "áp dụng biến đổi tuần tự".
- **Mở rộng có thể:** thêm phép biến đổi phối cảnh (perspective) hoặc mở rộng lên 3D — nền tảng trực tiếp của mọi engine đồ họa máy tính hiện đại.

---

## 19. Tự đánh giá

- [ ] Tôi có thể giải thích ma trận dưới cả hai góc nhìn: bảng số tĩnh, và phép biến đổi tác động lên vector.
- [ ] Tôi có thể tự tay tính phép cộng, nhân vô hướng, và **nhân ma trận** cho các ma trận nhỏ mà không cần tra lại công thức.
- [ ] Tôi hiểu và có thể giải thích *tại sao* $AB \neq BA$ nói chung, và có thể đưa ra một ví dụ cụ thể minh họa điều đó.
- [ ] Tôi hiểu được ý tưởng "nhân ma trận = hợp thành phép biến đổi" (Mục 7.2), ít nhất ở mức trực giác, dù chưa cần chứng minh hình thức đầy đủ.
- [ ] Tôi phân biệt được Matrix Multiplication và Hadamard Product (Bảng 3.2.1) và biết khi nào NumPy/PyTorch dùng toán tử nào.
- [ ] Tôi đã hoàn thành Dự án nhỏ ở Mục 18 và quan sát được trực quan việc hợp hai phép biến đổi bằng nhân ma trận.

Nếu Bài tập 5 (nhân hai ma trận xoay) vẫn khó khăn, hãy quay lại Mục 7.1–7.2 và thực hành thêm vài ví dụ tay trước khi sang Chương 3.3 — trực giác "nhân ma trận = hợp thành biến đổi" sẽ được dùng lại liên tục trong suốt phần còn lại của Part III.

---

## 20. Đọc thêm

- **Sách:** Marc Peter Deisenroth, A. Aldo Faisal, Cheng Soon Ong, *Mathematics for Machine Learning*, Chương 2 — phần Ma trận và các phép toán cơ bản. *(Xem BOOKS.md — Volume 1.)*
- **Sách nâng cao (tùy chọn):** Sheldon Axler, *Linear Algebra Done Right* — cách tiếp cận ma trận thông qua phép biến đổi tuyến tính ngay từ đầu, phù hợp để đào sâu Mục 7.2.
- **Bài báo mở rộng (không bắt buộc):** Strassen, *Gaussian Elimination is not Optimal* (1969) — bài báo gốc giới thiệu thuật toán nhân ma trận nhanh hơn, nhắc đến ở Mục 12; chỉ nên đọc sau khi đã vững phần đại số ma trận cơ bản.
- **Chương tiếp theo:** Chương 3.3 — Matrix Operations.

---

### Liên kết chương (Cross References)

- **Chương trước:** 3.1 — Vectors (ma trận được xây dựng trực tiếp như tập hợp có thứ tự các vector).
- **Chương tiếp theo:** 3.3 — Matrix Operations (nghịch đảo, định thức, hạng — tất cả đều được định nghĩa dựa trên phép nhân ma trận vừa học).
- **Chương liên quan xa hơn:** Volume 1, Part III, Chương 3.4 — Linear Transformations (hình thức hóa đầy đủ ý tưởng "ma trận là phép biến đổi" từ Mục 4–5, 7.2 của chương này); Volume 5 — Artificial Intelligence (lớp fully-connected $y=Wx+b$ là ứng dụng trực tiếp).
- **Vị trí trong Knowledge Graph:** Nút thứ hai của Part III, phụ thuộc trực tiếp vào Chương 3.1; là điều kiện tiên quyết bắt buộc cho Chương 3.3 đến 3.7 (toàn bộ phần còn lại của Part III đều thao tác trên ma trận).

---

*Hết Chương 3.2. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer, tiếp nối trực tiếp Chương 3.1 theo outline Part III đã đề xuất. Mọi kết quả tính toán tay (nhân ma trận, tính không giao hoán) đều được đối chiếu bằng cả cài đặt thủ công lẫn NumPy ở Mục 9–10, bao gồm minh chứng thực nghiệm cụ thể cho $AB \neq BA$. Đang chờ rà soát trước khi tiếp tục sang Chương 3.3 — Matrix Operations.*
