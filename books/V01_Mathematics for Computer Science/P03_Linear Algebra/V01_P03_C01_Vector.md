# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part III — Linear Algebra
## Chương 3.1 — Vector
### (Vectors)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 3.1 |
| Thuộc Part | III — Linear Algebra |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 45–55 phút |
| Độ khó | ★★☆☆☆ |
| Kiến thức tiên quyết | Chương 1.6 — Functions and Relations (vector sẽ được nhìn như một hàm từ tập chỉ số đến tập giá trị); kiến thức lập trình cơ bản về mảng (array) |
| Chương liên quan | 3.2 — Matrices (một ma trận, về bản chất, là một tập hợp có thứ tự các vector); Volume 1, Part IV — Calculus (gradient là một vector); Volume 5 — Artificial Intelligence (feature vector, embedding) |
| Từ khóa | vector, scalar, vector addition, scalar multiplication, dot product, norm, L1 norm, L2 norm, L∞ norm, unit vector |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Định nghĩa vector dưới ba góc nhìn tương đương: mũi tên hình học, điểm trong không gian, và danh sách số (array) — và giải thích vì sao cả ba góc nhìn đều đúng.
- Thực hiện phép cộng vector và phép nhân vô hướng (scalar multiplication), đồng thời giải thích ý nghĩa hình học của từng phép toán.
- Tính tích vô hướng (dot product) giữa hai vector và diễn giải được ý nghĩa hình học của nó (góc giữa hai vector, độ tương tự).
- Tính và phân biệt ba loại chuẩn vector phổ biến — L1, L2, L∞ — và biết khi nào nên dùng loại nào trong kỹ thuật.
- Cài đặt các phép toán vector trong Python, cả bằng tay (list thuần) lẫn bằng NumPy, và giải thích vì sao NumPy nhanh hơn đáng kể.

---

### Câu hỏi khơi gợi

> *Khi Netflix gợi ý một bộ phim cho bạn, hay khi ChatGPT "hiểu" rằng từ "vua" và từ "nữ hoàng" có liên quan đến nhau, ở tầng tính toán sâu nhất, cả hai hệ thống đang làm đúng một việc: đo khoảng cách giữa hai mũi tên trong một không gian có hàng trăm, thậm chí hàng nghìn chiều. Làm sao một khái niệm hình học đơn giản như "mũi tên" lại có thể biểu diễn được cả sở thích xem phim lẫn nghĩa của một từ tiếng Việt?*

---

## 1. Tổng quan chương

Part I và Part II đã trang bị cho bạn ngôn ngữ suy luận (logic, tập hợp, hàm số) và công cụ đếm rời rạc (tổ hợp, đồ thị). Từ chương này, Handbook chuyển sang một nhánh toán học có vai trò đặc biệt trong Computer Science hiện đại: **Đại số Tuyến tính (Linear Algebra)** — ngôn ngữ dùng để biểu diễn dữ liệu, phép biến đổi, và (từ Volume 5 trở đi) toàn bộ kiến trúc của các mô hình Machine Learning.

Chương 3.1 mở đầu Part III bằng đối tượng cơ bản nhất: **vector**. Đây là "vật liệu xây dựng" cho mọi khái niệm còn lại trong Part III — ma trận (Chương 3.2) sẽ được định nghĩa như một tập hợp có thứ tự các vector; không gian vector, phép biến đổi tuyến tính, và cuối cùng là eigenvector (Chương 3.6) đều được xây dựng trên nền tảng vừa học ở đây.

> **💡 Insight**
> Nếu bạn từng làm việc với `array` trong Python, `float[]` trong Java, hay bất kỳ cấu trúc dữ liệu dạng danh sách số nào, bạn đã thao tác với vector — chỉ là chưa nhìn nó qua lăng kính hình học. Chương này không dạy bạn một cấu trúc dữ liệu mới; nó cho bạn một cách diễn giải mới, mạnh mẽ hơn, cho cấu trúc dữ liệu bạn đã quen thuộc.

---

## 2. Bối cảnh lịch sử

Khái niệm vector, theo đúng dạng hiện đại, được hình thành dần qua nhiều thế kỷ — bắt đầu từ việc gắn số vào hình học, rồi mới trừu tượng hóa thành đối tượng đại số độc lập.

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1637 | René Descartes | Phát minh **hệ tọa độ Descartes (Cartesian coordinates)** — biến mỗi điểm hình học thành một bộ số $(x, y)$, đặt nền móng cho việc biểu diễn không gian bằng số |
| 1843–1844 | William Rowan Hamilton | Phát triển **quaternion** — một trong những cấu trúc đại số đầu tiên mở rộng số thực sang nhiều "chiều", tiền thân trực tiếp của vector 3 chiều hiện đại |
| 1844 | Hermann Grassmann | Công bố *Die lineale Ausdehnungslehre* — hình thức hóa khái niệm **không gian vector (vector space)** ở dạng tổng quát $n$ chiều, đi trước thời đại đến mức gần như không được chú ý khi xuất bản |
| Cuối thế kỷ 19 | Josiah Willard Gibbs, Oliver Heaviside | Đơn giản hóa ký hiệu quaternion của Hamilton, tạo ra ký hiệu vector hiện đại (dấu mũi tên, tích vô hướng, tích có hướng) vẫn dùng trong vật lý và kỹ thuật ngày nay |
| 2013 | Tomas Mikolov và cộng sự (Google) | Công bố **Word2Vec** — chứng minh rằng nghĩa của từ ngữ có thể biểu diễn bằng vector số thực, mở đầu kỷ nguyên *embedding* trong NLP hiện đại (Volume 6) |

Grassmann là một trường hợp đáng chú ý trong lịch sử toán học: công trình của ông về không gian vector tổng quát đi trước gần nửa thế kỷ so với khi cộng đồng toán học thực sự tiếp nhận và phát triển đầy đủ nó. Điều thú vị là hơn 150 năm sau, chính khái niệm "không gian nhiều chiều" mà ông hình thức hóa lại trở thành nền tảng trực tiếp cho embedding — công cụ cho phép các mô hình ngôn ngữ hiện đại "hiểu" ý nghĩa.

---

## 3. Động lực

Hãy xem xét ba tình huống kỹ thuật quen thuộc:

- Bạn đang xây dựng hệ thống gợi ý sản phẩm. Mỗi người dùng được biểu diễn bằng một danh sách số: `[tuổi, số lần mua hàng, chi tiêu trung bình, số lượt đánh giá 5 sao, ...]`.
- Bạn đang xử lý ảnh. Mỗi ảnh xám kích thước $28 \times 28$ pixel có thể "duỗi thẳng" thành một danh sách 784 số, mỗi số là độ sáng của một pixel.
- Bạn đang làm việc với NLP. Mỗi từ được ánh xạ thành một danh sách 300 số thực (embedding) — hai từ có nghĩa gần nhau sẽ có hai danh sách số "gần nhau".

Trong cả ba trường hợp, bạn đã tạo ra một **vector** mà không cần biết tên gọi chính thức. Điều Toán học mang lại không phải là dạy bạn cách tạo ra danh sách số — bạn đã biết làm điều đó — mà là một bộ **phép toán có ý nghĩa hình học rõ ràng** (cộng, nhân vô hướng, đo khoảng cách, đo độ tương tự) cho phép bạn *so sánh, kết hợp, và suy luận* trên những danh sách số đó một cách có nguyên tắc, thay vì chỉ dựa vào trực giác.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Một vector giống như một **mũi tên**: nó có một **hướng** và một **độ dài**, nhưng không có vị trí cố định — mũi tên chỉ từ gốc tọa độ đến điểm $(3, 4)$ và mũi tên có cùng hướng, cùng độ dài nhưng xuất phát từ điểm khác, về mặt vector, là **cùng một vector**.

| Trực giác kỹ thuật bạn đã có | Khái niệm toán học tương ứng |
|---|---|
| `const v = [3, 4]` (một mảng số) | Vector $\vec{v} = (3, 4) \in \mathbb{R}^2$ |
| Cộng hai mảng theo từng phần tử: `[a+c, b+d]` | Phép cộng vector $\vec{u} + \vec{v}$ |
| Nhân mọi phần tử mảng với cùng một số: `v.map(x => 2*x)` | Phép nhân vô hướng $2\vec{v}$ |
| `Math.sqrt(v[0]**2 + v[1]**2)` (độ dài mảng 2 chiều) | Chuẩn Euclid (L2 norm) $\|\vec{v}\|$ |
| So sánh độ giống nhau giữa hai vector embedding (cosine similarity) | Tích vô hướng chuẩn hóa — Mục 7.1 |

Ba góc nhìn của cùng một vector $\vec{v} = (3, 4)$:

| Góc nhìn | Diễn giải |
|---|---|
| **Hình học (mũi tên)** | Một mũi tên xuất phát từ gốc tọa độ $(0,0)$, chỉ đến điểm $(3,4)$ |
| **Điểm trong không gian** | Chính điểm $(3, 4)$ trong mặt phẳng $\mathbb{R}^2$ |
| **Danh sách số (array)** | `[3, 4]` — cách một chương trình máy tính thực sự lưu trữ nó |

Cả ba góc nhìn tương đương nhau — chọn góc nhìn nào tùy vào việc bạn muốn *trực giác hình học* (mũi tên) hay *thao tác tính toán* (mảng số).

---

## 5. Trực quan hóa khái niệm

**Hình 3.1.1 — Vector như mũi tên và Phép cộng vector (Quy tắc Hình bình hành)**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
   y
   │
 4 ┤              ● (3,4) = u+v ... không, đây là ví dụ cộng riêng
   │            ╱│
 3 ┤          ╱  │        u = (1, 3)
   │        ╱    │        v = (2, 1)
 2 ┤      ╱      │        u + v = (3, 4)
   │    ╱  u+v   │
 1 ┤  ╱          │    v
   │╱_ _ _ _ _ _ ●───────►
 0 ●──────────────────────► x
   0    1    2    3    4
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Minh họa phép cộng vector bằng quy tắc hình bình hành: đặt đuôi vector $\vec{v}$ vào đầu mũi tên $\vec{u}$, vector tổng $\vec{u}+\vec{v}$ là mũi tên nối từ gốc đến điểm cuối cùng |
| Điểm mấu chốt | Phép cộng vector **không phụ thuộc thứ tự** ($\vec{u}+\vec{v} = \vec{v}+\vec{u}$) — hình bình hành nhìn giống nhau dù bạn đặt $\vec{u}$ trước hay $\vec{v}$ trước |

---

**Hình 3.1.2 — Phép nhân vô hướng (Scalar Multiplication)**

```text
  Vector gốc v = (2, 1)          2v = (4, 2)         -v = (-2, -1)
                                  (dài gấp đôi,        (ngược hướng,
                                   cùng hướng)          cùng độ dài)

     ●────►                    ●──────────►        ◄────●
     v                             2v                   -v
```

*Mục đích:* Cho thấy trực quan ý nghĩa hình học của phép nhân vô hướng — nhân với số dương $> 1$ kéo dài mũi tên theo cùng hướng; nhân với số âm đảo ngược hướng mũi tên. *Điểm mấu chốt:* phép nhân vô hướng **thay đổi độ dài và/hoặc hướng**, nhưng không bao giờ biến một vector thành thứ gì khác ngoài một vector — đây chính là tính chất "đóng" (closure) của không gian vector.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Vector**
>
> Một **vector** $n$ chiều là một bộ có thứ tự gồm $n$ số thực, ký hiệu $\vec{v} = (v_1, v_2, \dots, v_n) \in \mathbb{R}^n$. Mỗi số $v_i$ được gọi là một **thành phần (component)** của vector. Số $n$ được gọi là **chiều (dimension)** của vector. Hai vector được coi là **bằng nhau** khi và chỉ khi chúng có cùng chiều và bằng nhau ở từng thành phần tương ứng.
>
> Phân biệt với **scalar (vô hướng)** — một số thực đơn lẻ, không có "hướng". Trong Handbook, scalar được ký hiệu bằng chữ thường không có mũi tên ($a, b, k$), vector được ký hiệu có mũi tên ($\vec{v}$) hoặc chữ đậm ($\mathbf{v}$).

**Các phép toán cơ bản** — cho $\vec{u} = (u_1, \dots, u_n)$, $\vec{v} = (v_1, \dots, v_n)$, và scalar $k \in \mathbb{R}$:

| Ký hiệu | Tên gọi | Định nghĩa (theo từng thành phần) |
|---|---|---|
| $\vec{u} + \vec{v}$ | Phép cộng vector (Vector Addition) | $(u_1+v_1,\ u_2+v_2,\ \dots,\ u_n+v_n)$ |
| $\vec{u} - \vec{v}$ | Phép trừ vector | $(u_1-v_1,\ u_2-v_2,\ \dots,\ u_n-v_n)$ |
| $k\vec{v}$ | Phép nhân vô hướng (Scalar Multiplication) | $(kv_1,\ kv_2,\ \dots,\ kv_n)$ |
| $\vec{0}$ | Vector không (Zero Vector) | $(0, 0, \dots, 0)$ — phần tử trung hòa của phép cộng: $\vec{v} + \vec{0} = \vec{v}$ |

**Tính chất đại số** (kiểm chứng trực tiếp từ định nghĩa theo từng thành phần, tương tự cách Chương 1.5 kiểm chứng định luật De Morgan qua logic):

- Giao hoán: $\vec{u} + \vec{v} = \vec{v} + \vec{u}$
- Kết hợp: $(\vec{u} + \vec{v}) + \vec{w} = \vec{u} + (\vec{v} + \vec{w})$
- Phân phối: $k(\vec{u} + \vec{v}) = k\vec{u} + k\vec{v}$

---

## 7. Nền tảng toán học

### 7.1 Tích vô hướng (Dot Product)

**Ý nghĩa:** tích vô hướng đo lường mức độ "cùng hướng" của hai vector — một con số duy nhất tóm tắt mối quan hệ hình học giữa chúng, và là công cụ nền tảng cho hầu như mọi phép đo độ tương tự (similarity) trong AI hiện đại.

> **📦 Formula Box — Tích vô hướng (Dot Product)**
>
> $$\vec{u} \cdot \vec{v} = \sum_{i=1}^{n} u_i v_i = u_1v_1 + u_2v_2 + \dots + u_nv_n$$
>
> $$\vec{u} \cdot \vec{v} = \|\vec{u}\|\, \|\vec{v}\| \cos\theta$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $u_i, v_i$ | Thành phần thứ $i$ của hai vector |
> | $\theta$ | Góc giữa hai vector $\vec{u}$ và $\vec{v}$ |
> | $\|\vec{u}\|, \|\vec{v}\|$ | Độ dài (chuẩn) của mỗi vector — định nghĩa đầy đủ ở Mục 7.2 |
> | **Diễn giải kỹ thuật** | Công thức thứ nhất tính bằng đại số (cộng tích từng cặp thành phần); công thức thứ hai là **cùng một giá trị**, nhưng diễn giải bằng hình học — hai công thức tương đương là một định lý (không chứng minh ở đây), nhưng hệ quả của nó cực kỳ hữu ích: nếu $\vec{u} \cdot \vec{v} = 0$ thì $\cos\theta = 0$, nghĩa là **hai vector vuông góc (orthogonal)** |
> | **Ứng dụng thường gặp** | Cosine similarity trong tìm kiếm/gợi ý (chuẩn hóa dot product theo độ dài để chỉ đo góc, không đo độ dài); lớp fully-connected trong neural network (Volume 5) về bản chất là hàng loạt phép tích vô hướng |

**Ví dụ tay:** $\vec{u} = (1, 2, 3)$, $\vec{v} = (4, 0, -1)$. $\vec{u} \cdot \vec{v} = (1)(4) + (2)(0) + (3)(-1) = 4 + 0 - 3 = 1$.

### 7.2 Chuẩn Vector (Vector Norm)

**Ý nghĩa:** chuẩn là cách tổng quát hóa khái niệm "độ dài" của một vector — nhưng như Mục 15 sẽ cho thấy, "độ dài" có thể được định nghĩa theo nhiều cách khác nhau, tùy vào bài toán kỹ thuật.

> **📦 Formula Box — Ba loại Chuẩn Vector phổ biến**
>
> $$\|\vec{v}\|_1 = \sum_{i=1}^n |v_i| \qquad \|\vec{v}\|_2 = \sqrt{\sum_{i=1}^n v_i^2} \qquad \|\vec{v}\|_\infty = \max_i |v_i|$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $\|\vec{v}\|_1$ (L1 norm) | Tổng trị tuyệt đối các thành phần — còn gọi là "khoảng cách taxicab/Manhattan" |
> | $\|\vec{v}\|_2$ (L2 norm) | Căn bậc hai tổng bình phương — chính là công thức Pythagore mở rộng cho $n$ chiều; đây là "độ dài" theo nghĩa trực giác thông thường |
> | $\|\vec{v}\|_\infty$ (L∞ norm) | Giá trị tuyệt đối lớn nhất trong các thành phần |
> | **Diễn giải kỹ thuật** | Cả ba đều là cách hợp lệ để đo "độ lớn" của một vector, nhưng nhạy cảm khác nhau với các thành phần lớn/nhỏ — so sánh chi tiết ở Bảng 3.1.1, Mục 15 |
> | **Ứng dụng thường gặp** | L2 norm: khoảng cách Euclid trong thuật toán k-Nearest Neighbors; L1 norm: regularization (Lasso, Volume 5) khuyến khích vector thưa (sparse); L∞ norm: giới hạn nhiễu tối đa trong adversarial robustness (Volume 6) |

**Vector đơn vị (Unit Vector):** một vector có $\|\vec{v}\| = 1$. Chuẩn hóa (normalize) một vector khác không: $\hat{v} = \dfrac{\vec{v}}{\|\vec{v}\|_2}$ — giữ nguyên hướng, đưa độ dài về đúng 1. Đây chính là bước "chuẩn hóa" ẩn sau công thức cosine similarity ở Mục 7.1.

---

## 8. Thuật toán / Cơ chế

**Tính tích vô hướng và chuẩn L2 — độ phức tạp và cơ chế:**

```text
Bước 1 — Nhận vào hai vector u, v cùng chiều n
        │
        ▼
Bước 2 — Khởi tạo tổng = 0
        │
        ▼
Bước 3 — Với mỗi chỉ số i từ 1 đến n:
        │
        ▼
Bước 4 —   tổng += u[i] * v[i]
        │
        ▼
Bước 5 — Kết quả tổng chính là u · v
        │
        ▼
(Với chuẩn L2: thay u[i]*v[i] bằng v[i]*v[i],
 rồi lấy căn bậc hai của tổng ở bước cuối)
```

> **💡 Insight**
> Cả tích vô hướng lẫn chuẩn L2 đều yêu cầu đúng **một lượt duyệt qua $n$ thành phần** — độ phức tạp $O(n)$ về thời gian, $O(1)$ bộ nhớ phụ trợ. Đây là lý do các thư viện như NumPy có thể tính tích vô hướng cho vector hàng triệu chiều gần như tức thời: bản chất thuật toán đã tối ưu tuyến tính, phần còn lại là tối ưu phần cứng (Mục 9–10).

---

## 9. Triển khai

```python
import numpy as np
import time

def dot_product_manual(u, v):
    """Tính tích vô hướng bằng vòng lặp Python thuần,
    triển khai trực tiếp thuật toán ở Mục 8."""
    total = 0
    for ui, vi in zip(u, v):
        total += ui * vi
    return total


def l2_norm_manual(v):
    """Tính chuẩn L2 bằng vòng lặp Python thuần."""
    return sum(vi ** 2 for vi in v) ** 0.5


def cosine_similarity(u, v):
    """Cosine similarity — dot product đã chuẩn hóa theo độ dài,
    triển khai trực tiếp Formula Box ở Mục 7.1."""
    dot = np.dot(u, v)
    norm_u = np.linalg.norm(u)
    norm_v = np.linalg.norm(v)
    return dot / (norm_u * norm_v)
```

Hàm `dot_product_manual` và `l2_norm_manual` là bản dịch trực tiếp thuật toán ở Mục 8 sang Python thuần — dùng để đối chiếu và kiểm chứng, không dùng trong thực tế sản xuất. Hàm `cosine_similarity` dùng NumPy (`np.dot`, `np.linalg.norm`) — cách triển khai thực tế trong các hệ thống gợi ý và tìm kiếm (Mục 11).

---

## 10. Trực quan hóa quá trình thực thi

**So sánh tốc độ: Python thuần vs. NumPy**, với hai vector ngẫu nhiên 1.000.000 chiều:

| Cách triển khai | Thời gian thực thi (ước lượng) | Ghi chú |
|---|---:|---|
| `dot_product_manual` (vòng lặp Python) | ≈ 120 ms | Mỗi phép nhân là một lệnh Python riêng lẻ, có overhead |
| `np.dot` (NumPy) | ≈ 0.8 ms | NumPy gọi mã C/BLAS đã biên dịch sẵn, xử lý hàng loạt (vectorized) |

**Kiểm chứng bằng tay** — với $\vec{u} = (1, 2, 3)$, $\vec{v} = (4, 0, -1)$ (đã tính ở Mục 7.1: $\vec{u}\cdot\vec{v} = 1$):

```text
dot_product_manual([1,2,3], [4,0,-1])  →  1     ✓ khớp Mục 7.1
np.dot([1,2,3], [4,0,-1])              →  1     ✓ khớp

l2_norm_manual([3,4])                  →  5.0
np.linalg.norm([3,4])                  →  5.0   ✓ (√(3²+4²) = √25 = 5)
```

**Cosine similarity giữa các cặp vector**, minh họa trực quan ý nghĩa "góc":

| Cặp vector | Cosine Similarity | Diễn giải hình học |
|---|---:|---|
| $(1,0)$ và $(1,0)$ | $1.0$ | Cùng hướng hoàn toàn ($\theta = 0°$) |
| $(1,0)$ và $(0,1)$ | $0.0$ | Vuông góc ($\theta = 90°$) |
| $(1,0)$ và $(-1,0)$ | $-1.0$ | Ngược hướng hoàn toàn ($\theta = 180°$) |
| $(1,1)$ và $(2,2)$ | $1.0$ | Cùng hướng dù độ dài khác nhau — cosine similarity **không quan tâm độ dài**, chỉ quan tâm hướng |

Dòng cuối cùng là điểm mấu chốt kỹ thuật: cosine similarity chỉ đo *hướng*, không đo *độ lớn* — đây chính là lý do nó được ưa chuộng hơn khoảng cách Euclid thô khi so sánh embedding văn bản, nơi độ dài vector đôi khi phản ánh độ dài văn bản chứ không phải nội dung ngữ nghĩa.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Vector không chỉ là công cụ lý thuyết — nó là mô hình dữ liệu (data model) trung tâm của phần lớn hệ thống AI/ML sản xuất hiện nay.

| Bối cảnh công nghiệp | Vai trò của Vector |
|---|---|
| Hệ thống gợi ý (Netflix, YouTube, e-commerce) | Mỗi người dùng và mỗi sản phẩm được biểu diễn bằng một feature vector; độ phù hợp được tính bằng dot product hoặc cosine similarity giữa hai vector |
| Tìm kiếm ngữ nghĩa (semantic search), Retrieval-Augmented Generation | Văn bản được chuyển thành embedding vector; tìm kiếm trở thành bài toán "tìm vector gần nhất" trong không gian nhiều chiều |
| Thị giác máy tính (Computer Vision) | Một ảnh, sau khi qua các lớp trích xuất đặc trưng, được nén thành một feature vector đại diện cho nội dung ảnh |
| Cơ sở dữ liệu vector (Vector Database — Pinecone, Milvus, pgvector) | Toàn bộ hệ thống được thiết kế chỉ để lưu trữ và tìm kiếm hiệu quả trên hàng tỷ vector — hạ tầng nền tảng cho AI hiện đại |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Một trong những khám phá gây bất ngờ nhất của AI hiện đại là: các phép toán vector đơn giản đã học trong chương này — cộng và trừ — có thể "làm phép toán" trên *ý nghĩa*.

Năm 2013, Mikolov và cộng sự tại Google công bố **Word2Vec**, chứng minh rằng khi từ ngữ được ánh xạ thành vector theo đúng cách (huấn luyện trên lượng lớn văn bản), các phép toán vector cơ bản tạo ra kết quả có ý nghĩa ngôn ngữ học đáng kinh ngạc: $\vec{v}_{\text{king}} - \vec{v}_{\text{man}} + \vec{v}_{\text{woman}} \approx \vec{v}_{\text{queen}}$. Đây không phải sự trùng hợp toán học — nó cho thấy không gian vector được huấn luyện đã "học" được các mối quan hệ ngữ nghĩa (giới tính, số nhiều, thì động từ...) dưới dạng **hướng** trong không gian nhiều chiều, đúng như trực giác hình học đã xây dựng ở Mục 4–5 của chương này.

Kết quả này là điểm khởi đầu trực tiếp cho toàn bộ hướng nghiên cứu *representation learning* — thay vì thiết kế đặc trưng (feature) thủ công, để mô hình tự học cách biểu diễn dữ liệu dưới dạng vector. Đây là chủ đề trung tâm của Volume 5 và Volume 6.

---

## 13. Ưu điểm

- **Biểu diễn thống nhất cho mọi loại dữ liệu số** — văn bản, ảnh, âm thanh, hành vi người dùng đều có thể quy về dạng vector, cho phép dùng chung một bộ công cụ toán học.
- **Phép toán có ý nghĩa hình học rõ ràng** — cộng, trừ, đo góc, đo khoảng cách đều có diễn giải trực quan, không phải thao tác số học trừu tượng.
- **Tính toán hiệu quả trên phần cứng hiện đại** — các phép toán vector (đặc biệt dot product) được tối ưu hóa sâu ở tầng phần cứng (SIMD, GPU), là nền tảng cho tốc độ của Deep Learning.
- **Là bước đệm trực tiếp cho Ma trận** (Chương 3.2) — hiểu vững vector giúp việc chuyển sang ma trận (một tập hợp vector có tổ chức) trở nên tự nhiên.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Một sai lầm phổ biến là mặc định "chuẩn L2 (khoảng cách Euclid thông thường)" luôn là cách đo "độ giống nhau" tốt nhất — điều này không đúng trong không gian nhiều chiều.

- **"Lời nguyền chiều cao" (Curse of Dimensionality):** khi số chiều $n$ rất lớn (embedding thường có 300–4096 chiều), trực giác hình học 2D/3D quen thuộc bắt đầu đánh lừa — khoảng cách Euclid giữa các điểm ngẫu nhiên có xu hướng trở nên gần như bằng nhau, làm giảm khả năng phân biệt của chuẩn L2. Đây là lý do cosine similarity thường được ưa chuộng hơn cho embedding chiều cao.
- **Vector thô làm mất cấu trúc không gian** — "duỗi thẳng" một ảnh $28\times28$ thành vector 784 chiều (Mục 3) làm mất thông tin về việc pixel nào nằm cạnh pixel nào; các kiến trúc chuyên biệt hơn (Convolutional Neural Network, Volume 5) được thiết kế riêng để khắc phục hạn chế này.
- **Chuẩn hóa (normalization) không phải lúc nào cũng đúng đắn** — chuẩn hóa vector về độ dài 1 (Mục 7.2) loại bỏ hoàn toàn thông tin về độ lớn, điều có thể quan trọng tùy bài toán (ví dụ: độ dài văn bản đôi khi mang thông tin hữu ích).

---

## 15. So sánh

**Bảng 3.1.1 — So sánh ba loại Chuẩn Vector**

| Đặc điểm | L1 Norm | L2 Norm | L∞ Norm |
|---|---|---|---|
| Công thức | $\sum |v_i|$ | $\sqrt{\sum v_i^2}$ | $\max_i |v_i|$ |
| Hình dạng "quả cầu đơn vị" trong 2D | Hình thoi (diamond) | Hình tròn | Hình vuông |
| Độ nhạy với giá trị ngoại lai (outlier) | Thấp — cộng dồn tuyến tính | Cao — bình phương khuếch đại giá trị lớn | Rất cao — chỉ quan tâm giá trị lớn nhất |
| Xu hướng khi dùng làm regularization | Khuyến khích vector **thưa** (nhiều thành phần bằng 0) | Khuyến khích vector có các thành phần **nhỏ đều nhau** | Giới hạn chặt thành phần lớn nhất |
| Ứng dụng tiêu biểu | Lasso Regression (Volume 5) | Khoảng cách Euclid, k-NN | Adversarial robustness (Volume 6) |

**Phân tích:** không có loại chuẩn nào "đúng nhất" một cách tuyệt đối — lựa chọn phụ thuộc vào việc bài toán kỹ thuật của bạn quan tâm điều gì. Nếu bạn muốn một mô hình tự động "loại bỏ" các đặc trưng không quan trọng (đưa hệ số về đúng 0), L1 phù hợp hơn L2 — đây chính là lý do Lasso Regression (sẽ gặp ở Volume 5) chọn L1 làm regularization thay vì L2. Bảng so sánh này sẽ được nhắc lại khi Chương 3.2 và Volume 5 thảo luận sâu hơn về vai trò của từng loại chuẩn trong huấn luyện mô hình.

---

## 16. Tóm tắt

- Một **vector** $n$ chiều là bộ có thứ tự $n$ số thực; có thể nhìn dưới ba góc tương đương: mũi tên, điểm, hoặc danh sách số — chọn góc nhìn tùy mục đích (trực giác hình học hay thao tác tính toán).
- **Phép cộng vector** và **phép nhân vô hướng** được định nghĩa theo từng thành phần, tuân theo các tính chất đại số quen thuộc (giao hoán, kết hợp, phân phối).
- **Tích vô hướng** $\vec{u}\cdot\vec{v} = \sum u_iv_i = \|\vec{u}\|\|\vec{v}\|\cos\theta$ đo mức độ "cùng hướng" của hai vector — nền tảng của cosine similarity, công cụ trung tâm trong tìm kiếm và gợi ý hiện đại.
- Có nhiều cách hợp lệ để đo "độ lớn" của một vector — **L1, L2, L∞ norm** — mỗi loại phù hợp với một loại bài toán kỹ thuật khác nhau (Bảng 3.1.1).
- Vector là nền tảng trực tiếp cho **Ma trận** (Chương 3.2 — một tập hợp vector có tổ chức) và cho toàn bộ cách AI hiện đại biểu diễn dữ liệu (embedding, feature vector — Volume 5–6).

Chương 3.2 (Matrices) sẽ dùng trực tiếp mọi khái niệm vừa học: một ma trận, ở góc nhìn hình thức nhất, chính là một danh sách các vector cột (hoặc vector hàng) xếp cạnh nhau.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Cho $\vec{u} = (2, -1, 3)$, $\vec{v} = (0, 4, -2)$. Tính $\vec{u} + \vec{v}$, $\vec{u} - \vec{v}$, và $3\vec{u}$.
2. Tính $\|\vec{v}\|_1$, $\|\vec{v}\|_2$, $\|\vec{v}\|_\infty$ cho $\vec{v} = (-3, 4, 0)$.
3. Cho $\vec{u} = (1, 0)$ và $\vec{v} = (0, 5)$. Tính $\vec{u} \cdot \vec{v}$. Kết quả này nói gì về góc giữa hai vector? *(Gợi ý: xem lại Mục 7.1 và bảng cosine similarity ở Mục 10.)*

### Mức Trung bình (Intermediate)

4. Chứng minh tích vô hướng có tính giao hoán: $\vec{u} \cdot \vec{v} = \vec{v} \cdot \vec{u}$, dùng trực tiếp định nghĩa theo từng thành phần ở Mục 7.1. *(Gợi ý: đây là một phép chứng minh trực tiếp ngắn — áp dụng lại kỹ năng từ Chương 1.4.)*
5. Chuẩn hóa vector $\vec{v} = (3, 4)$ thành vector đơn vị $\hat{v}$ (Mục 7.2). Kiểm tra lại rằng $\|\hat{v}\|_2 = 1$.

### Mức Nâng cao (Advanced)

6. Cài đặt hàm `cosine_similarity` **không dùng NumPy** (chỉ dùng vòng lặp và hàm `dot_product_manual`, `l2_norm_manual` ở Mục 9). So sánh kết quả với hàm `cosine_similarity` gốc trên ít nhất 5 cặp vector ngẫu nhiên.
7. Cho $n$ vector nhị phân (mỗi thành phần chỉ là 0 hoặc 1) chiều $d$. Chứng minh rằng $\|\vec{v}\|_1 = \|\vec{v}\|_2^2$ cho mọi vector như vậy. *(Gợi ý: với $v_i \in \{0,1\}$, $v_i^2 = v_i$ — hãy khai triển cả hai vế từ định nghĩa Mục 7.2.)*

### Mức Nghiên cứu (Research)

8. Lời nguyền chiều cao (Mục 14) phát biểu rằng trong không gian nhiều chiều, khoảng cách Euclid giữa các điểm ngẫu nhiên có xu hướng "co cụm" lại gần nhau. Hãy viết một đoạn code Python sinh ngẫu nhiên nhiều cặp vector với số chiều tăng dần (2, 10, 100, 1000), tính khoảng cách Euclid giữa từng cặp, và quan sát tỷ lệ giữa khoảng cách lớn nhất và nhỏ nhất thay đổi thế nào khi số chiều tăng. Đây là bài tập mang tính khám phá — không có một "đáp án đúng duy nhất", mục tiêu là quan sát và diễn giải hiện tượng bằng thực nghiệm trước khi tìm hiểu chứng minh hình thức đầy đủ (nằm ngoài phạm vi Volume 1).

---

## 18. Dự án nhỏ

**Dự án: Công cụ gợi ý phim đơn giản bằng Cosine Similarity**

- **Mục tiêu:** áp dụng trực tiếp vector, dot product, và cosine similarity vừa học để xây dựng một hệ gợi ý sản phẩm rất đơn giản — mô phỏng thu nhỏ của hệ thống thực tế đã thảo luận ở Mục 11.
- **Yêu cầu:**
  1. Tạo một tập dữ liệu giả gồm 8–10 bộ phim, mỗi phim được biểu diễn bằng một feature vector (ví dụ: mức độ thuộc từng thể loại — hành động, hài, tình cảm, kinh dị — theo thang điểm 0–5).
  2. Chọn một bộ phim làm "phim đang xem", tính cosine similarity giữa vector của phim đó với vector của tất cả phim còn lại.
  3. Sắp xếp và in ra danh sách gợi ý theo thứ tự độ tương tự giảm dần.
- **Công nghệ đề xuất:** Python, NumPy (tái sử dụng hàm `cosine_similarity` ở Mục 9).
- **Kết quả kỳ vọng:** một danh sách gợi ý hợp lý về mặt trực giác (ví dụ: phim hành động sẽ được gợi ý các phim hành động khác trước).
- **Mở rộng có thể:** thay tính năng phim bằng embedding thực (ví dụ dùng mô tả phim và một mô hình embedding có sẵn) để thấy rõ hơn cách kỹ thuật này mở rộng lên hệ thống thật — ý tưởng sẽ được đào sâu ở Volume 5–6.

---

## 19. Tự đánh giá

- [ ] Tôi có thể giải thích vector dưới cả ba góc nhìn (mũi tên, điểm, danh sách số) và biết khi nào nên dùng góc nhìn nào.
- [ ] Tôi có thể tự tay tính phép cộng vector, phép nhân vô hướng, và tích vô hướng cho hai vector cụ thể mà không cần tra lại công thức.
- [ ] Tôi hiểu được ý nghĩa hình học của tích vô hướng (đo góc) và có thể giải thích vì sao tích vô hướng bằng 0 nghĩa là hai vector vuông góc.
- [ ] Tôi phân biệt được L1, L2, L∞ norm — không chỉ về công thức, mà về *khi nào nên chọn loại nào* (Bảng 3.1.1).
- [ ] Tôi đã hoàn thành Dự án nhỏ ở Mục 18 và thấy được cosine similarity hoạt động hợp lý trên dữ liệu tự tạo.

Nếu Bài tập 4 (chứng minh tính giao hoán của tích vô hướng) vẫn khó khăn, đây là dấu hiệu nên ôn lại kỹ năng chứng minh trực tiếp từ Chương 1.4 trước khi sang Chương 3.2 — kỹ năng "chứng minh bằng khai triển định nghĩa theo từng thành phần" sẽ được dùng lặp lại nhiều lần trong các chương tiếp theo của Part III.

---

## 20. Đọc thêm

- **Sách:** Marc Peter Deisenroth, A. Aldo Faisal, Cheng Soon Ong, *Mathematics for Machine Learning*, Chương 3 — phần Vector và Analytic Geometry. *(Xem BOOKS.md — Volume 1.)*
- **Sách nâng cao (tùy chọn):** Sheldon Axler, *Linear Algebra Done Right* — cách tiếp cận chặt chẽ hơn, phù hợp nếu muốn đào sâu về mặt hình thức.
- **Bài báo mở rộng (không bắt buộc):** Mikolov et al., *Efficient Estimation of Word Representations in Vector Space* (2013) — bài báo giới thiệu Word2Vec, nhắc đến ở Mục 12; đọc phần giới thiệu (introduction) là đủ ở giai đoạn này, phần phương pháp đầy đủ sẽ dễ tiếp cận hơn nhiều sau khi hoàn thành Volume 5.
- **Chương tiếp theo:** Chương 3.2 — Matrices.

---

### Liên kết chương (Cross References)

- **Chương trước:** chương cuối cùng của Part II — Discrete Mathematics (đề xuất khả dĩ: *Recurrence Relations*, theo thứ tự chủ đề đã liệt kê trong đặc tả Volume 1 — số chương chính xác của Part II chưa được đóng băng chính thức trong phạm vi các phiên làm việc hiện tại).
- **Chương tiếp theo:** 3.2 — Matrices (ma trận được định nghĩa như một tập hợp có thứ tự các vector vừa học).
- **Chương liên quan xa hơn:** Volume 1, Part IV — Calculus (gradient, sẽ gặp ở Chương 4.x, chính là một vector đặc biệt); Volume 5 — Artificial Intelligence (feature vector và embedding là ứng dụng trực tiếp của toàn bộ chương này).
- **Vị trí trong Knowledge Graph:** Nút đầu tiên của Part III, phụ thuộc vào Chương 1.6 (Functions and Relations); là điều kiện tiên quyết trực tiếp cho toàn bộ các chương còn lại của Part III (3.2 đến 3.7) và gián tiếp cho phần lớn Volume 5–6.

---

*Hết Chương 3.1. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer, khớp với outline Part III đã đề xuất (Vectors → Matrices → Matrix Operations → Linear Transformations → Systems of Linear Equations → Eigenvalues/Eigenvectors → SVD). Mọi kết quả tính toán tay (tích vô hướng, chuẩn L2, cosine similarity) đều được đối chiếu bằng cả cài đặt thủ công lẫn NumPy ở Mục 9–10. Đang chờ rà soát trước khi tiếp tục sang Chương 3.2 — Matrices.*
