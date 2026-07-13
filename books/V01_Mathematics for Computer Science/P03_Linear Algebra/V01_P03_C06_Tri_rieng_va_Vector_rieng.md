# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part III — Linear Algebra
## Chương 3.6 — Trị riêng và Vector riêng
### (Eigenvalues and Eigenvectors)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 3.6 |
| Thuộc Part | III — Linear Algebra |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 60–70 phút |
| Độ khó | ★★★★☆ |
| Kiến thức tiên quyết | Chương 3.1–3.5 (bắt buộc — đặc biệt Chương 3.3, Mục "ma trận đối xứng"; Chương 3.4, "phép biến đổi tuyến tính"; Chương 3.5, "giải hệ phương trình" — dùng trực tiếp để tìm eigenvector) |
| Chương liên quan | 3.7 — Singular Value Decomposition (tổng quát hóa eigen-decomposition cho ma trận không vuông); Volume 5 — Artificial Intelligence (PCA, phân tích ổn định huấn luyện); Volume 4 — Data Engineering (Google PageRank) |
| Từ khóa | eigenvalue, eigenvector, characteristic equation, characteristic polynomial, eigenspace, diagonalization, spectral theorem, power iteration |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Định nghĩa eigenvalue và eigenvector cả về mặt hình học (hướng bất biến) lẫn đại số ($A\vec{v}=\lambda\vec{v}$).
- Thiết lập và giải phương trình đặc trưng (characteristic equation) $\det(A-\lambda I)=0$ để tìm eigenvalue cho ma trận $2\times2$.
- Tìm eigenvector tương ứng với mỗi eigenvalue bằng cách giải hệ phương trình thuần nhất (dùng trực tiếp kỹ thuật Chương 3.5).
- Giải thích khái niệm chéo hóa ma trận (diagonalization) $A=PDP^{-1}$ và điều kiện để một ma trận chéo hóa được.
- Phát biểu và giải thích ý nghĩa của Định lý Phổ (Spectral Theorem) cho ma trận đối xứng — cầu nối trực tiếp đến PCA (Volume 5) và SVD (Chương 3.7).
- Cài đặt tính toán eigenvalue/eigenvector bằng tay và bằng NumPy, đối chiếu kết quả.

---

### Câu hỏi khơi gợi

> *Khi một phép biến đổi xoay, kéo dãn, và cắt xiên một không gian theo một cách phức tạp, liệu có tồn tại những **hướng đặc biệt** mà phép biến đổi đó hoàn toàn không làm đổi hướng — chỉ kéo dài hoặc thu ngắn chúng dọc theo đúng hướng ban đầu? Nếu những hướng như vậy tồn tại, chúng tiết lộ điều gì về bản chất sâu xa nhất của phép biến đổi — một bản chất không phụ thuộc vào việc bạn chọn hệ tọa độ nào để mô tả nó?*

---

## 1. Tổng quan chương

Năm chương trước trang bị đầy đủ công cụ để **mô tả** và **phân tích** một phép biến đổi tuyến tính bất kỳ. Chương này đặt ra một câu hỏi hẹp hơn nhưng sâu sắc hơn nhiều: với một phép biến đổi cụ thể $A$, có tồn tại những **hướng bất biến (invariant directions)** — những vector mà $A$ chỉ kéo dài hay thu ngắn, không hề xoay đi — hay không?

Những hướng đặc biệt đó gọi là **eigenvector**, và hệ số kéo dài/thu ngắn tương ứng gọi là **eigenvalue**. Đây không phải là một tò mò toán học thuần túy — chúng tiết lộ **bản chất nội tại** của một phép biến đổi, độc lập với việc chọn hệ tọa độ, và là công cụ toán học trực tiếp đứng sau PCA (giảm chiều dữ liệu), phân tích ổn định của hệ thống động, và thuật toán PageRank của Google.

> **💡 Insight**
> Từ "eigen" trong "eigenvalue" và "eigenvector" là từ tiếng Đức, nghĩa đen là "riêng", "đặc trưng của chính nó" — không dịch sang tiếng Anh vì không có từ tương đương gọn ghẽ. Cái tên này phản ánh chính xác bản chất của khái niệm: eigenvector là hướng "thuộc về", "đặc trưng riêng" của chính phép biến đổi $A$, không phụ thuộc vào cách bạn chọn hệ trục tọa độ để mô tả nó.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| Thế kỷ 18 | Leonhard Euler | Nghiên cứu **trục quay chính (principal axes)** của vật rắn trong cơ học — về bản chất toán học, đây chính là bài toán tìm eigenvector, dù chưa mang tên gọi đó |
| 1820s | Augustin-Louis Cauchy | Nghiên cứu các "trục chính" của dạng toàn phương (quadratic form), một bài toán tương đương với tìm eigenvalue của ma trận đối xứng |
| Đầu thế kỷ 20 | David Hilbert | Đặt ra thuật ngữ **"eigenwert"** (eigenvalue) trong bối cảnh phương trình tích phân, sau này được dịch/mượn nguyên sang tiếng Anh |
| 1925 | Werner Heisenberg | Trong cơ học ma trận (đã nhắc ở Chương 3.2), các mức năng lượng lượng tử quan sát được tương ứng chính xác với **eigenvalue** của ma trận biểu diễn năng lượng — một trong những ứng dụng vật lý gây chấn động nhất của khái niệm này |
| 1901–1933 | Karl Pearson, Harold Hotelling | Pearson đề xuất ý tưởng Phân tích Thành phần Chính (PCA) năm 1901; Hotelling hình thức hóa đầy đủ bằng ngôn ngữ eigen-decomposition năm 1933 — nền tảng trực tiếp cho ứng dụng ở Volume 5 |

Điểm đáng chú ý: khái niệm eigenvector được khám phá độc lập trong nhiều bối cảnh vật lý hoàn toàn khác nhau — cơ học vật rắn (Euler), dạng toàn phương (Cauchy), phương trình tích phân (Hilbert), rồi cơ học lượng tử (Heisenberg) — trước khi được nhận ra là **cùng một khái niệm toán học** đang xuất hiện lặp lại dưới nhiều lớp áo khác nhau. Đây là một ví dụ kinh điển về sức mạnh của trừu tượng hóa toán học: một công cụ, nhiều ứng dụng tưởng chừng không liên quan.

---

## 3. Động lực

Ba tình huống kỹ thuật minh họa sức mạnh của eigenvalue/eigenvector:

- **Giảm chiều dữ liệu (Volume 5):** với một tập dữ liệu nhiều chiều, ma trận hiệp phương sai (covariance matrix — luôn đối xứng, Chương 3.3) mô tả mức độ biến thiên của dữ liệu theo mọi hướng. **Eigenvector có eigenvalue lớn nhất** chỉ đúng hướng mà dữ liệu biến thiên nhiều nhất — đây chính là nguyên lý cốt lõi của PCA.
- **Phân tích ổn định:** khi áp dụng lặp đi lặp lại cùng một phép biến đổi $A$ (mô hình tăng trưởng dân số, chuỗi Markov, hay việc lan truyền tín hiệu qua nhiều lớp neural network giống nhau), hành vi dài hạn của hệ thống — hội tụ về 0, bùng nổ ra vô cùng, hay ổn định — hoàn toàn được quyết định bởi **độ lớn của eigenvalue lớn nhất**.
- **Google PageRank:** thứ hạng của một trang web trên Google (phiên bản gốc, 1998) được tính bằng cách tìm **eigenvector trội (dominant eigenvector)** của một ma trận khổng lồ biểu diễn cấu trúc liên kết giữa các trang web — một trong những ứng dụng thương mại thành công nhất trong lịch sử của khái niệm toán học được Euler nghiên cứu 250 năm trước.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Hầu hết các vector, khi bị một phép biến đổi $A$ tác động, sẽ vừa bị xoay vừa bị kéo dài. Nhưng có một số hướng **đặc biệt** — eigenvector — nơi $A$ **chỉ kéo dài hoặc thu ngắn**, không hề xoay đi đâu cả: vector kết quả vẫn nằm chính xác trên đường thẳng ban đầu. Hệ số kéo dài/thu ngắn đó chính là eigenvalue.

| Trực giác kỹ thuật bạn đã có | Khái niệm toán học tương ứng |
|---|---|
| Hướng "hút" ổn định của một hệ thống lặp (attractor) | Eigenvector ứng với eigenvalue có trị tuyệt đối lớn nhất |
| Trục chính của một hình elip | Eigenvector của ma trận đối xứng (Mục 5, Hình 3.6.2) |
| Trạng thái ổn định (steady state) của chuỗi Markov | Eigenvector ứng với eigenvalue $=1$ |
| Tần số dao động tự nhiên của một kết cấu cơ khí | Eigenvalue của ma trận độ cứng (stiffness matrix) trong kỹ thuật kết cấu |

---

## 5. Trực quan hóa khái niệm

**Hình 3.6.1 — Eigenvector: Hướng KHÔNG bị xoay đi**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
   Vector THÔNG THƯỜNG                Eigenvector
   (bị biến đổi A xoay đi)            (chỉ bị kéo dài, KHÔNG xoay)

        Av                                  Av = λv
       ╱                                        ╲
      ╱                                          ╲
     ╱   v                                        ╲    v
    ╱   ╱                                          ╲  ╱
   ╱   ╱                                            ╲╱
      (Av lệch hướng               (Av nằm chính xác trên
       so với v)                    cùng đường thẳng với v,
                                     chỉ dài hơn λ lần)
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Đối chiếu trực quan giữa một vector "thông thường" (bị $A$ xoay lệch hướng) và một eigenvector (chỉ bị kéo dài dọc theo đúng hướng cũ) |
| Điểm mấu chốt | Đây chính xác là nội dung phương trình định nghĩa $A\vec{v}=\lambda\vec{v}$ ở Mục 6 — vế trái là kết quả biến đổi, vế phải là chính $\vec{v}$ nhân với một số vô hướng $\lambda$, nghĩa là **không đổi hướng** |

---

**Hình 3.6.2 — Hình tròn Đơn vị biến thành Hình elip: Trục là các Eigenvector**

```text
   Hình tròn đơn vị              Sau biến đổi bởi A (đối xứng)
                                  → Hình elip, trục = eigenvector

        ___                            ╱‾‾‾‾╲
      ╱     ╲                        ╱        ╲
     │   ●   │        A            │            │
      ╲     ╱       ────►           ╲          ╱
        ‾‾‾                           ╲______╱
                                     (độ dài mỗi trục = |eigenvalue|
                                      tương ứng hướng đó)
```

*Mục đích:* Minh họa hình học sâu sắc nhất của chương — với **ma trận đối xứng** (Chương 3.3), phép biến đổi biến hình tròn đơn vị thành một hình elip mà **các trục chính trùng đúng với các eigenvector**, và **độ dài mỗi trục bằng trị tuyệt đối eigenvalue tương ứng**. *Điểm mấu chốt:* đây là hình ảnh trực quan của **Định lý Phổ (Spectral Theorem)** — sẽ phát biểu chính xác ở Mục 6 — và là nền tảng hình học trực tiếp cho PCA (Volume 5) lẫn SVD (Chương 3.7).

---

## 6. Định nghĩa hình thức

> **📌 Remember — Eigenvalue và Eigenvector**
>
> Cho ma trận vuông $A \in \mathbb{R}^{n\times n}$. Một số vô hướng $\lambda$ (có thể thực hoặc phức) được gọi là **eigenvalue** của $A$ nếu tồn tại một vector $\vec{v} \neq \vec{0}$ sao cho:
>
> $$A\vec{v} = \lambda\vec{v}$$
>
> Vector $\vec{v}$ khi đó được gọi là **eigenvector** ứng với eigenvalue $\lambda$. Chú ý: định nghĩa yêu cầu $\vec{v}\neq\vec{0}$ (vector không luôn thỏa mãn phương trình một cách tầm thường, không mang thông tin gì).

**Phương trình đặc trưng (Characteristic Equation):** viết lại định nghĩa trên thành $(A-\lambda I)\vec{v} = \vec{0}$ — đây là một **hệ phương trình thuần nhất** (Chương 3.5). Để có nghiệm $\vec{v}\neq\vec{0}$ (nghiệm không tầm thường), theo Chương 3.3 Mục 7.3, ma trận $(A-\lambda I)$ phải **suy biến**:

$$\det(A - \lambda I) = 0$$

Đây gọi là **phương trình đặc trưng**; vế trái, khai triển ra, là một đa thức bậc $n$ theo $\lambda$, gọi là **đa thức đặc trưng (characteristic polynomial)**.

**Không gian riêng (Eigenspace):** với mỗi eigenvalue $\lambda$, tập hợp tất cả eigenvector ứng với $\lambda$ (cộng thêm vector không) tạo thành một **eigenspace** — chính là tập nghiệm của hệ thuần nhất $(A-\lambda I)\vec{v}=\vec{0}$, giải được bằng khử Gauss (Chương 3.5).

**Chéo hóa (Diagonalization):** nếu $A$ có đủ $n$ eigenvector **độc lập tuyến tính** (Chương 3.4), $A$ có thể viết dưới dạng:

$$A = PDP^{-1}$$

trong đó $P$ là ma trận có các cột là $n$ eigenvector đó, và $D$ là ma trận đường chéo chứa các eigenvalue tương ứng. Ma trận không có đủ eigenvector độc lập tuyến tính được gọi là **không chéo hóa được (defective)**.

> **📌 Remember — Định lý Phổ (Spectral Theorem)**
>
> Nếu $A$ là **ma trận đối xứng** ($A=A^\top$, Chương 3.3), thì: (1) mọi eigenvalue của $A$ đều là **số thực** (không có số phức); và (2) $A$ **luôn** chéo hóa được, với các eigenvector có thể chọn **trực giao (orthogonal)** với nhau. Đây là lý do Hình 3.6.2 luôn đúng cho ma trận đối xứng — và là lý do ma trận hiệp phương sai (luôn đối xứng) trong PCA (Mục 3, Volume 5) luôn cho một tập trục chính vuông góc, dễ diễn giải.

---

## 7. Nền tảng toán học

### 7.1 Phương trình Đặc trưng cho Ma trận $2\times2$

> **📦 Formula Box — Phương trình Đặc trưng $2\times2$**
>
> $$A = \begin{pmatrix}a&b\\c&d\end{pmatrix} \quad\Longrightarrow\quad \lambda^2 - \text{tr}(A)\,\lambda + \det(A) = 0$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $\text{tr}(A) = a+d$ | **Vết (trace)** của ma trận — tổng các phần tử trên đường chéo chính; đồng thời bằng tổng hai eigenvalue: $\lambda_1+\lambda_2=\text{tr}(A)$ |
> | $\det(A) = ad-bc$ | Định thức (Chương 3.3); đồng thời bằng **tích** hai eigenvalue: $\lambda_1\lambda_2=\det(A)$ |
> | **Diễn giải kỹ thuật** | Đây là phương trình bậc hai quen thuộc, giải bằng công thức nghiệm $\lambda = \dfrac{\text{tr}(A)\pm\sqrt{\text{tr}(A)^2-4\det(A)}}{2}$ |
> | **Ứng dụng thường gặp** | Kiểm tra nhanh: nếu $\det(A)=0$ (ma trận suy biến, Chương 3.3), một trong hai eigenvalue chắc chắn bằng 0 — liên hệ trực tiếp Mục 14 |

**Ví dụ tay:** $A = \begin{pmatrix}4&1\\2&3\end{pmatrix}$. $\text{tr}(A)=7$, $\det(A)=12-2=10$. Phương trình: $\lambda^2-7\lambda+10=0 \Rightarrow (\lambda-5)(\lambda-2)=0 \Rightarrow \lambda_1=5,\ \lambda_2=2$.

### 7.2 Tìm Eigenvector

Với mỗi eigenvalue $\lambda_i$ vừa tìm được, giải hệ thuần nhất $(A-\lambda_i I)\vec{v}=\vec{0}$ **bằng khử Gauss** (Chương 3.5) — hệ này luôn có vô số nghiệm (do $(A-\lambda_i I)$ suy biến theo cách xây dựng), nên chọn một nghiệm đại diện (thường chuẩn hóa về vector đơn vị, Chương 3.1, Mục 7.2).

**Tiếp tục ví dụ:** với $\lambda_1=5$: $(A-5I)\vec{v}=\begin{pmatrix}-1&1\\2&-2\end{pmatrix}\vec{v}=\vec{0} \Rightarrow -v_1+v_2=0 \Rightarrow v_1=v_2$. Chọn $\vec{v}_1=(1,1)$. Với $\lambda_2=2$: $(A-2I)\vec{v}=\begin{pmatrix}2&1\\2&1\end{pmatrix}\vec{v}=\vec{0} \Rightarrow 2v_1+v_2=0$. Chọn $\vec{v}_2=(1,-2)$.

---

## 8. Thuật toán / Cơ chế

**Quy trình tính Eigenvalue/Eigenvector (trường hợp tổng quát):**

```text
Bước 1 — Lập đa thức đặc trưng: khai triển det(A - λI)
        │
        ▼
Bước 2 — Giải đa thức đặc trưng = 0 để tìm các nghiệm λ
         (chính là các eigenvalue)
        │
        ▼
Bước 3 — Với mỗi λ tìm được:
        │
        ▼
Bước 4 —   Giải hệ thuần nhất (A - λI)v = 0 bằng khử Gauss
           (Chương 3.5) để tìm eigenvector tương ứng
        │
        ▼
Bước 5 — (Tùy chọn) Chuẩn hóa mỗi eigenvector về vector đơn vị
```

> **⚠️ Common Mistake**
> Bước 2 — giải đa thức đặc trưng — chỉ đơn giản cho ma trận $2\times2$ hoặc $3\times3$ (công thức nghiệm bậc hai/ba). Với $n\geq5$, **Định lý Abel–Ruffini** (một kết quả nổi tiếng của đại số trừu tượng) chứng minh rằng **không tồn tại công thức nghiệm tổng quát** bằng căn thức cho đa thức bậc 5 trở lên — nghĩa là về nguyên tắc, không thể "giải" đa thức đặc trưng bằng công thức đóng cho ma trận lớn. Trong thực hành, eigenvalue của ma trận lớn **luôn** được tính bằng thuật toán số lặp (Mục 12), không phải bằng cách giải đa thức đặc trưng tường minh.

---

## 9. Triển khai

```python
import numpy as np

def eigen_2x2(A):
    """Tính eigenvalue và eigenvector cho ma trận 2×2 —
    triển khai trực tiếp Mục 7.1–7.2."""
    a, b = A[0]
    c, d = A[1]
    trace = a + d
    det = a * d - b * c

    disc = trace**2 - 4 * det
    sqrt_disc = disc**0.5
    lambda1 = (trace + sqrt_disc) / 2
    lambda2 = (trace - sqrt_disc) / 2

    def eigenvector_for(lam):
        # Giải (A - λI)v = 0 cho trường hợp 2×2 (nếu b ≠ 0)
        if abs(b) > 1e-12:
            v = np.array([1.0, (lam - a) / b])
        else:
            v = np.array([(lam - d) / c, 1.0])
        return v / np.linalg.norm(v)  # chuẩn hóa — Chương 3.1, Mục 7.2

    return (lambda1, eigenvector_for(lambda1)), (lambda2, eigenvector_for(lambda2))


# Kiểm chứng với ví dụ tay Mục 7.1–7.2
A = [[4, 1], [2, 3]]
print(eigen_2x2(A))
# ((5.0, [0.707, 0.707]), (2.0, [0.447, -0.894]))
# → hướng khớp (1,1) và (1,-2) đã tính tay, chỉ khác vì đã chuẩn hóa

# Đối chiếu NumPy
eigenvalues, eigenvectors = np.linalg.eig(np.array(A))
print(eigenvalues)   # [5. 2.]
print(eigenvectors)  # các cột là eigenvector đã chuẩn hóa
```

Hàm `eigen_2x2` là bản dịch trực tiếp Mục 7.1–7.2 sang Python, chỉ dùng được cho ma trận $2\times2$. Trong thực hành với ma trận kích thước bất kỳ, luôn dùng `np.linalg.eig` — cài đặt bằng thuật toán số lặp ổn định (Mục 12), không dựa trên giải đa thức đặc trưng tường minh.

---

## 10. Trực quan hóa quá trình thực thi

**Kiểm chứng $A\vec{v}=\lambda\vec{v}$** với ví dụ Mục 7:

```text
A = [[4,1],[2,3]],  λ₁=5,  v₁=[1,1] (chưa chuẩn hóa)

A @ v₁            →  [5, 5]
λ₁ * v₁           →  [5, 5]     ✓ khớp hoàn toàn — đúng định nghĩa Mục 6
```

**Kiểm chứng Định lý Phổ (Mục 6)** — với ma trận đối xứng $S = \begin{pmatrix}2&1\\1&2\end{pmatrix}$:

```text
eigenvalues, eigenvectors = np.linalg.eig(S)
eigenvalues       →  [3. 1.]              (đều là số thực — đúng Spectral Theorem)

eigenvectors[:,0] · eigenvectors[:,1]
                  →  ≈ 0                   (tích vô hướng ≈ 0 → trực giao,
                                             đúng như Spectral Theorem dự đoán)
```

**Kiểm chứng $\text{tr}(A)$ và $\det(A)$ bằng tổng/tích eigenvalue** (Formula Box Mục 7.1):

```text
A = [[4,1],[2,3]]
eigenvalues = [5, 2]

tr(A) = 4+3 = 7          sum(eigenvalues) = 5+2 = 7      ✓
det(A) = 12-2 = 10       prod(eigenvalues) = 5*2 = 10    ✓
```

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Eigenvalue/eigenvector là một trong số ít công cụ toán học xuất hiện đồng thời trong lý thuyết đồ thị, Machine Learning cổ điển, cơ khí kết cấu, và lý thuyết điều khiển — hiếm có khái niệm nào có phổ ứng dụng rộng đến vậy.

| Bối cảnh công nghiệp | Vai trò của Eigenvalue/Eigenvector |
|---|---|
| Phân tích Thành phần Chính (PCA, Volume 5) | Eigenvector của ma trận hiệp phương sai chỉ ra các hướng biến thiên chính của dữ liệu; eigenvalue tương ứng cho biết mức độ biến thiên theo hướng đó |
| Google PageRank | Thứ hạng trang web là eigenvector trội của ma trận liên kết web — tính bằng thuật toán Power Iteration (Mục 12) |
| Phân tích kết cấu / Cơ khí | Eigenvalue của ma trận độ cứng cho biết **tần số dao động tự nhiên** của một kết cấu — thông tin sống còn để tránh cộng hưởng phá hủy công trình |
| Ổn định Huấn luyện Neural Network (Volume 5–6) | Eigenvalue của ma trận trọng số (hoặc ma trận Hessian của hàm mất mát) liên quan trực tiếp đến hiện tượng **exploding/vanishing gradient** — một vấn đề kỹ thuật trung tâm khi huấn luyện mạng sâu |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Vì Định lý Abel–Ruffini (Mục 8) loại trừ khả năng có công thức đóng cho ma trận lớn, việc tính eigenvalue/eigenvector hiệu quả cho ma trận khổng lồ vẫn là một lĩnh vực nghiên cứu tính toán số sôi động.

**Power Iteration** là thuật toán lặp đơn giản nhất, minh họa rõ ý tưởng cốt lõi: bắt đầu từ một vector ngẫu nhiên $\vec{x}_0$, lặp đi lặp lại $\vec{x}_{k+1} = A\vec{x}_k / \|A\vec{x}_k\|$ — sau đủ nhiều vòng lặp, $\vec{x}_k$ hội tụ về **eigenvector trội** (ứng với eigenvalue có trị tuyệt đối lớn nhất). Đây chính xác là thuật toán gốc đứng sau **Google PageRank** (Mục 11) — với ma trận liên kết web có hàng tỷ trang, không có cách nào giải đa thức đặc trưng, nhưng Power Iteration vẫn hội tụ hiệu quả về đúng eigenvector cần tìm.

Một hướng nghiên cứu khác — **Lý thuyết Đồ thị Phổ (Spectral Graph Theory)** — nghiên cứu cách eigenvalue của các ma trận liên quan đến đồ thị (ma trận kề, ma trận Laplace) tiết lộ tính chất cấu trúc sâu xa (tính liên thông, phát hiện cộng đồng — community detection), một chủ đề sẽ gặp lại khi Handbook thảo luận thuật toán đồ thị (Volume 3) và mạng neural network trên đồ thị — Graph Neural Networks (Volume 6). Song song đó, phân tích eigenvalue của ma trận Hessian trong quá trình huấn luyện Deep Learning là một hướng nghiên cứu lý thuyết đang tiếp diễn, nhằm giải thích vì sao một số kiến trúc mạng huấn luyện ổn định hơn những kiến trúc khác.

---

## 13. Ưu điểm

- **Tiết lộ bản chất nội tại của phép biến đổi**, độc lập với hệ tọa độ — eigenvalue không đổi dù bạn xoay hệ trục tọa độ đi bao nhiêu.
- **Đơn giản hóa việc áp dụng lặp lại một phép biến đổi** — với eigenvector $\vec{v}$: $A^k\vec{v} = \lambda^k\vec{v}$, biến một phép nhân ma trận $k$ lần tốn kém thành một phép lũy thừa số vô hướng đơn giản.
- **Nền tảng trực tiếp cho giảm chiều dữ liệu** (PCA, Mục 3, 11) — một trong những kỹ thuật được dùng nhiều nhất trong toàn bộ Machine Learning cổ điển.
- **Công cụ chẩn đoán ổn định** cho hệ thống động và huấn luyện mô hình (Mục 11) — biết eigenvalue giúp dự đoán hành vi dài hạn mà không cần mô phỏng đầy đủ.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Một ngộ nhận phổ biến: cho rằng **mọi** ma trận vuông đều chéo hóa được. Điều này **sai** — ma trận không có đủ $n$ eigenvector độc lập tuyến tính (ví dụ $\begin{pmatrix}1&1\\0&1\end{pmatrix}$, chỉ có duy nhất một hướng eigenvector độc lập dù là ma trận $2\times2$) được gọi là **ma trận suy biến-eigen (defective)**, và không thể viết dưới dạng $PDP^{-1}$ — dù vẫn có eigenvalue hợp lệ.

- **Không phải ma trận nào cũng chéo hóa được** (Common Mistake ở trên) — chỉ ma trận có đủ eigenvector độc lập tuyến tính mới thỏa $A=PDP^{-1}$.
- **Eigenvalue của ma trận không đối xứng có thể là số phức**, ngay cả khi mọi phần tử của ma trận là số thực — nội dung số phức nằm ngoài phạm vi Volume 1 nhưng cần lưu ý khi gặp trong thực hành (`np.linalg.eig` có thể trả về mảng số phức).
- **Giải đa thức đặc trưng tường minh không khả thi cho ma trận lớn** (Mục 8, Định lý Abel–Ruffini) — luôn cần thuật toán số lặp (Mục 12) trong thực hành.
- **Không phải mọi ma trận đối xứng đều mang ý nghĩa "elip" trực quan như Hình 3.6.2** nếu có eigenvalue âm — khi đó hình dạng biến đổi phức tạp hơn (bao gồm cả phản chiếu), dù Định lý Phổ vẫn đúng về mặt đại số.

---

## 15. So sánh

**Bảng 3.6.1 — Ma trận Tổng quát so với Ma trận Đối xứng**

| Đặc điểm | Ma trận Tổng quát | Ma trận Đối xứng ($A=A^\top$) |
|---|---|---|
| Eigenvalue | Có thể là số phức | Luôn là số thực (Định lý Phổ, Mục 6) |
| Eigenvector | Không nhất thiết trực giao với nhau | Luôn có thể chọn trực giao với nhau |
| Luôn chéo hóa được? | Không (có thể "defective", Mục 14) | Có — luôn luôn (Định lý Phổ) |
| Hình ảnh hình học của phép biến đổi | Phức tạp, không nhất thiết có "trục" rõ ràng | Hình elip (Hình 3.6.2) với trục chính chính xác là eigenvector |
| Ví dụ kỹ thuật điển hình | Ma trận trọng số neural network tổng quát | Ma trận hiệp phương sai (PCA, Volume 5), ma trận Laplace của đồ thị |

**Phân tích:** bảng này giải thích chính xác vì sao PCA (Volume 5) luôn hoạt động "đẹp" về mặt toán học — vì ma trận hiệp phương sai **luôn đối xứng theo cấu trúc**, PCA luôn được đảm bảo có eigenvalue thực và eigenvector trực giao, không bao giờ rơi vào các trường hợp khó xử ở cột bên trái của bảng. Đây là một minh chứng cụ thể cho việc nhận diện đúng "loại ma trận đặc biệt" (Chương 3.3, Bảng 3.3.1) giúp dự đoán trước hành vi toán học, trước cả khi tính toán cụ thể.

---

## 16. Tóm tắt

- **Eigenvector** là hướng bất biến của một phép biến đổi — chỉ bị kéo dài/thu ngắn theo hệ số **eigenvalue** $\lambda$, không bị xoay đi ($A\vec{v}=\lambda\vec{v}$).
- Eigenvalue tìm được bằng **phương trình đặc trưng** $\det(A-\lambda I)=0$; eigenvector tương ứng tìm bằng cách **giải hệ thuần nhất** $(A-\lambda I)\vec{v}=\vec{0}$ (dùng lại kỹ thuật Chương 3.5).
- Ma trận có đủ eigenvector độc lập tuyến tính thì **chéo hóa được**: $A=PDP^{-1}$.
- **Định lý Phổ:** ma trận **đối xứng** (Chương 3.3) luôn có eigenvalue thực và luôn chéo hóa được với eigenvector trực giao — nền tảng toán học trực tiếp cho PCA (Volume 5).
- Với ma trận lớn, eigenvalue được tính bằng **thuật toán số lặp** (Power Iteration và các biến thể tinh vi hơn), không phải giải đa thức đặc trưng tường minh (Mục 8, 12).

Chương 3.7 (Singular Value Decomposition) sẽ tổng quát hóa toàn bộ ý tưởng của chương này cho **cả ma trận không vuông** — một mở rộng cần thiết vì phần lớn dữ liệu thực tế (ví dụ ma trận người dùng–sản phẩm) không phải ma trận vuông, nên khái niệm eigen-decomposition thuần túy không áp dụng trực tiếp được.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Cho $A = \begin{pmatrix}3&0\\0&5\end{pmatrix}$ (ma trận đường chéo). Tìm eigenvalue và eigenvector mà không cần dùng phương trình đặc trưng — chỉ bằng quan sát trực tiếp cấu trúc ma trận đường chéo. *(Gợi ý: liên hệ Bảng 3.3.1, Chương 3.3.)*
2. Giải phương trình đặc trưng cho $A = \begin{pmatrix}1&2\\2&1\end{pmatrix}$ để tìm hai eigenvalue.
3. Kiểm chứng bằng tay: với $A$ ở Bài tập 2 và $\lambda_1$ vừa tìm được, hãy giải $(A-\lambda_1I)\vec{v}=\vec{0}$ để tìm eigenvector tương ứng.

### Mức Trung bình (Intermediate)

4. Cho $A = \begin{pmatrix}2&0\\0&3\end{pmatrix}$. Chứng minh bằng tay rằng $\text{tr}(A) = \lambda_1+\lambda_2$ và $\det(A)=\lambda_1\lambda_2$ (Formula Box Mục 7.1) đúng cho trường hợp cụ thể này.
5. Cho ma trận $A$ ở Bài tập 2 (đối xứng). Tính hai eigenvector, sau đó kiểm chứng bằng tay rằng tích vô hướng (Chương 3.1, Mục 7.1) giữa chúng bằng 0 — đúng như Định lý Phổ dự đoán.

### Mức Nâng cao (Advanced)

6. Cài đặt thuật toán **Power Iteration** (Mục 12) từ đầu bằng NumPy: khởi tạo vector ngẫu nhiên, lặp $\vec{x}_{k+1}=A\vec{x}_k/\|A\vec{x}_k\|_2$ khoảng 50 vòng, so sánh kết quả cuối cùng với eigenvector trội tính bằng `np.linalg.eig`.
7. Chứng minh ma trận $\begin{pmatrix}1&1\\0&1\end{pmatrix}$ (nhắc ở Mục 14, Common Mistake) chỉ có **một** hướng eigenvector độc lập tuyến tính (dù là ma trận $2\times2$, cần đủ 2 hướng độc lập để chéo hóa được). *(Gợi ý: tìm eigenvalue trước — chú ý cả hai nghiệm của phương trình đặc trưng trùng nhau — rồi giải hệ thuần nhất tương ứng và quan sát số biến tự do.)*

### Mức Nghiên cứu (Research)

8. Xây dựng một ma trận liên kết đơn giản mô phỏng PageRank: tạo 5 "trang web" với các liên kết ngẫu nhiên giữa chúng (ma trận $5\times5$ với giá trị biểu diễn xác suất chuyển từ trang này sang trang khác, mỗi cột tổng bằng 1). Dùng Power Iteration (Bài tập 6) để tìm eigenvector trội (ứng với eigenvalue $=1$) — đây chính là "thứ hạng PageRank" của 5 trang web. So sánh kết quả với `np.linalg.eig`. Đây là bài tập mang tính khám phá thực nghiệm, tái hiện thu nhỏ chính xác thuật toán đã cách mạng hóa công cụ tìm kiếm web.

---

## 18. Dự án nhỏ

**Dự án: Trực quan hóa Eigenvector và Mô phỏng PageRank Thu nhỏ**

- **Mục tiêu:** kết hợp trực quan hóa hình học (Hình 3.6.1–3.6.2) với ứng dụng thực tế của Power Iteration (Mục 12), tổng hợp toàn bộ chương thành một dự án hoàn chỉnh.
- **Yêu cầu:**
  1. Với một ma trận đối xứng $2\times2$ tự chọn, vẽ bằng `matplotlib`: hình tròn đơn vị, hình elip kết quả sau biến đổi, và hai eigenvector (dưới dạng mũi tên) chồng lên hình — tái hiện trực quan Hình 3.6.2.
  2. Cài đặt Power Iteration (Bài tập 6) thành một hàm tái sử dụng được.
  3. Dùng hàm đó để giải bài toán PageRank thu nhỏ (Bài tập 8) cho một mạng lưới ít nhất 8 "trang web" tự thiết kế; vẽ biểu đồ cột thứ hạng thu được.
  4. Viết một đoạn nhận xét ngắn: mạng lưới liên kết như thế nào (nhiều trang trỏ vào một trang trung tâm?) tạo ra thứ hạng PageRank cao nhất — kết nối trực giác hình học của Power Iteration với kết quả thứ hạng.
- **Công nghệ đề xuất:** Python, NumPy, Matplotlib.
- **Kết quả kỳ vọng:** hình ảnh trực quan rõ ràng cho cả eigenvector hình học lẫn kết quả PageRank thu nhỏ, cùng nhận xét kết nối được hai phần của dự án.
- **Mở rộng có thể:** thử nghiệm với ma trận liên kết không đối xứng (mạng liên kết thực tế trên web không đối xứng — trang A trỏ đến B không có nghĩa B trỏ lại A) và quan sát Power Iteration vẫn hoạt động dù Định lý Phổ không còn áp dụng.

---

## 19. Tự đánh giá

- [ ] Tôi có thể phát biểu chính xác định nghĩa eigenvalue/eigenvector cả bằng công thức ($A\vec{v}=\lambda\vec{v}$) lẫn bằng lời (hướng không bị xoay).
- [ ] Tôi có thể thiết lập và giải phương trình đặc trưng cho ma trận $2\times2$, sau đó tìm eigenvector tương ứng bằng cách giải hệ thuần nhất.
- [ ] Tôi có thể phát biểu Định lý Phổ và giải thích vì sao nó quan trọng đối với PCA.
- [ ] Tôi phân biệt được ma trận chéo hóa được và không chéo hóa được (defective), và biết ít nhất một ví dụ cụ thể cho trường hợp sau.
- [ ] Tôi hiểu ý tưởng cốt lõi của Power Iteration và có thể giải thích vì sao nó hội tụ về eigenvector trội mà không cần giải đa thức đặc trưng.
- [ ] Tôi đã hoàn thành Dự án nhỏ ở Mục 18, bao gồm cả trực quan hóa hình học lẫn mô phỏng PageRank thu nhỏ.

Nếu Bài tập 7 (ma trận không chéo hóa được) vẫn khó hình dung, hãy quay lại Mục 14 và thử tính tay từng bước — hiện tượng "hai eigenvalue trùng nhau nhưng không đủ eigenvector độc lập" là một trường hợp biên quan trọng, sẽ giúp hiểu sâu hơn điều kiện chính xác của phép chéo hóa trước khi sang Chương 3.7.

---

## 20. Đọc thêm

- **Sách:** Marc Peter Deisenroth, A. Aldo Faisal, Cheng Soon Ong, *Mathematics for Machine Learning*, Chương 4 — phần Eigenvalues, Eigenvectors, và ứng dụng vào PCA. *(Xem BOOKS.md — Volume 1.)*
- **Sách nâng cao (tùy chọn):** Sheldon Axler, *Linear Algebra Done Right* — cách trình bày đặc biệt nổi tiếng của cuốn sách này là **trì hoãn định thức** và xây dựng eigenvalue trực tiếp từ phép biến đổi tuyến tính; đọc song song với Handbook để có góc nhìn đối chiếu thú vị.
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc thêm về Định lý Abel–Ruffini (nhắc ở Mục 8) — một trong những kết quả đẹp nhất của đại số trừu tượng, giải thích vì sao "giải phương trình bằng căn thức" có giới hạn cơ bản.
- **Chương tiếp theo:** Chương 3.7 — Singular Value Decomposition.

---

### Liên kết chương (Cross References)

- **Chương trước:** 3.5 — Systems of Linear Equations (tìm eigenvector dùng trực tiếp kỹ thuật giải hệ thuần nhất vừa học).
- **Chương tiếp theo:** 3.7 — Singular Value Decomposition (tổng quát hóa eigen-decomposition cho ma trận không vuông, dùng lại trực tiếp Định lý Phổ của chương này).
- **Chương liên quan xa hơn:** Volume 4 — Data Engineering (Google PageRank, Mục 11–12); Volume 5 — Artificial Intelligence (PCA dùng trực tiếp Định lý Phổ, Mục 6); Volume 6 — Advanced AI (ổn định huấn luyện mạng sâu, exploding/vanishing gradient, Mục 11–12).
- **Vị trí trong Knowledge Graph:** Nút thứ sáu của Part III, phụ thuộc trực tiếp vào toàn bộ Chương 3.1–3.5; là điều kiện tiên quyết trực tiếp cho Chương 3.7, và là một trong những nút quan trọng nhất kết nối Part III với Volume 4–6.

---

*Hết Chương 3.6. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer, tiếp nối trực tiếp Chương 3.5 theo outline Part III đã đề xuất. Chương này hợp nhất Định lý Phổ (kết nối trực tiếp ma trận đối xứng từ Chương 3.3) với kỹ thuật giải hệ thuần nhất (Chương 3.5), và giới thiệu Power Iteration như cầu nối sang cả ứng dụng công nghiệp (PageRank) lẫn hướng nghiên cứu tính toán số hiện đại. Đang chờ rà soát trước khi tiếp tục sang Chương 3.7 — Singular Value Decomposition, chương cuối cùng của Part III.*
