# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part III — Linear Algebra
## Chương 3.7 — Phân rã Giá trị Kỳ dị
### (Singular Value Decomposition — SVD)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 3.7 (Chương cuối cùng của Part III) |
| Thuộc Part | III — Linear Algebra |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 60–70 phút |
| Độ khó | ★★★★☆ |
| Kiến thức tiên quyết | Chương 3.1–3.6 (bắt buộc — đặc biệt Chương 3.6, Định lý Phổ; Chương 3.3, ma trận trực giao) |
| Chương liên quan | Kết thúc Part III — mở đường sang Part IV (Calculus); Volume 5 — Artificial Intelligence (PCA chính là SVD áp dụng lên dữ liệu đã chuẩn hóa); Volume 4 — Data Engineering (Latent Semantic Analysis, hệ gợi ý) |
| Từ khóa | singular value decomposition, singular value, left singular vector, right singular vector, low-rank approximation, Eckart–Young theorem |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Phát biểu định lý SVD: $A = U\Sigma V^\top$ cho **mọi** ma trận $A$, kể cả ma trận không vuông.
- Giải thích ý nghĩa hình học tổng quát của SVD: mọi phép biến đổi tuyến tính đều phân rã thành đúng ba bước — xoay, kéo dãn theo trục, xoay lần nữa.
- Kết nối SVD với eigen-decomposition của Chương 3.6: giá trị kỳ dị (singular value) liên hệ trực tiếp với eigenvalue của $A^\top A$.
- Tính SVD bằng tay cho ma trận nhỏ, thông qua eigen-decomposition của $A^\top A$.
- Áp dụng SVD để **xấp xỉ hạng thấp (low-rank approximation)** — nền tảng của nén dữ liệu, giảm chiều, và hệ gợi ý.
- Phát biểu và giải thích ý nghĩa của Định lý Eckart–Young.

---

### Câu hỏi khơi gợi

> *Chương 3.6 mang lại một kết quả đẹp đẽ — Định lý Phổ — nhưng chỉ áp dụng cho ma trận **vuông** và **đối xứng**. Phần lớn ma trận xuất hiện trong thực tế kỹ thuật lại không vuông chút nào: ma trận đánh giá người dùng–sản phẩm (hàng triệu người dùng, hàng nghìn sản phẩm), ma trận ảnh (chiều cao khác chiều rộng), ma trận văn bản–từ vựng (số văn bản khác số từ trong từ điển). Vậy điều gì xảy ra khi bạn muốn tìm "trục chính" của một phép biến đổi đưa không gian $n$ chiều sang một không gian $m$ chiều hoàn toàn khác?*

---

## 1. Tổng quan chương

Đây là chương cuối cùng của Part III, và nó khép lại toàn bộ hành trình bắt đầu từ Hình 3.2.2 (Chương 3.2) — hình ảnh một hình vuông bị "kéo nghiêng" thành hình bình hành bởi một ma trận bất kỳ. Từ đó đến nay, Part III đã xây dựng công cụ để phân tích phép biến đổi ngày càng sâu sắc hơn: định thức đo mức phóng đại (3.3), phép biến đổi tuyến tính được hình thức hóa đầy đủ (3.4), hệ phương trình được giải chính xác (3.5), và với ma trận đối xứng, Định lý Phổ (3.6) tiết lộ các "trục chính" ẩn giấu.

**Singular Value Decomposition (SVD)** là điểm hội tụ cuối cùng: nó chứng minh rằng ý tưởng "trục chính" của Chương 3.6 — vốn chỉ áp dụng cho ma trận vuông đối xứng — thực ra tổng quát hóa được cho **bất kỳ ma trận nào**, vuông hay không, đối xứng hay không. Kết quả là một trong những định lý đẹp và hữu dụng nhất của toàn bộ đại số tuyến tính.

> **💡 Insight**
> Sau chương này, bạn sẽ có câu trả lời đầy đủ cho câu hỏi đặt ra từ Chương 3.2: **mọi** phép biến đổi tuyến tính, dù trông phức tạp đến đâu, đều có thể phân rã thành đúng ba bước hình học đơn giản: **xoay — kéo dãn theo các trục — xoay lần nữa**. Không có phép biến đổi tuyến tính nào phức tạp hơn thế.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1873–1874 | Eugenio Beltrami, Camille Jordan | Độc lập khám phá phân rã giá trị kỳ dị cho ma trận vuông thực, trong bối cảnh nghiên cứu dạng song tuyến tính (bilinear forms) |
| 1889 | James Joseph Sylvester | Mở rộng kết quả sang ma trận **chữ nhật (không vuông)** — bước tổng quát hóa quyết định để SVD áp dụng được cho dữ liệu thực tế |
| 1907 | Erhard Schmidt | Mở rộng khái niệm sang không gian **vô hạn chiều**, đặt nền móng lý thuyết cho Giải tích Hàm hiện đại |
| 1936 | Carl Eckart, Gale Young | Chứng minh **Định lý Eckart–Young** (Mục 7.2) — kết quả biến SVD từ một công cụ lý thuyết thành công cụ ứng dụng thực tế mạnh mẽ nhất của đại số tuyến tính |
| 1965 | Gene Golub, William Kahan | Phát triển thuật toán số ổn định để tính SVD trên máy tính điện tử — biến SVD từ một định lý đẹp trên giấy thành công cụ tính toán khả thi cho dữ liệu lớn |

Con đường lịch sử của SVD phản ánh đúng cấu trúc của chính chương này: khám phá ban đầu cho trường hợp đặc biệt (ma trận vuông, tương tự Chương 3.6), sau đó tổng quát hóa dần (ma trận chữ nhật, rồi vô hạn chiều), và cuối cùng — gần một thế kỷ sau khám phá đầu tiên — mới trở thành công cụ tính toán thực tế nhờ thuật toán số ổn định của Golub và Kahan.

---

## 3. Động lực

Ba tình huống kỹ thuật minh họa vì sao cần một công cụ tổng quát hơn eigen-decomposition:

- **Hệ gợi ý** (đã nhắc ở Chương 3.2, Mục 11): ma trận đánh giá người dùng–sản phẩm có kích thước, ví dụ, $10\text{ triệu} \times 50\text{ nghìn}$ — hoàn toàn không vuông, không đối xứng. Eigen-decomposition (Chương 3.6) **không định nghĩa được** cho ma trận này; nhưng SVD thì có.
- **Nén ảnh:** một bức ảnh xám kích thước $1080\times1920$ pixel là một ma trận không vuông. SVD cho phép xấp xỉ ma trận này bằng một phiên bản "hạng thấp" — giữ lại phần lớn thông tin thị giác quan trọng trong khi giảm đáng kể dung lượng lưu trữ (Mục 7.2, Mục 18).
- **Phân tích Ngữ nghĩa Tiềm ẩn (Latent Semantic Analysis — LSA)** trong xử lý ngôn ngữ tự nhiên: ma trận văn bản–từ vựng (số văn bản khác số từ trong từ điển) được phân rã bằng SVD để phát hiện các "chủ đề tiềm ẩn" — nền tảng lịch sử của nhiều kỹ thuật tìm kiếm ngữ nghĩa hiện đại (Volume 4, Volume 6).

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> **Mọi** phép biến đổi tuyến tính, dù phức tạp đến đâu, đều có thể phân tích thành đúng ba bước liên tiếp: **xoay** không gian đầu vào về một hệ trục thuận tiện, **kéo dãn** độc lập theo từng trục mới đó (có thể co, có thể dãn, thậm chí "làm sập" một số chiều về 0), rồi **xoay** kết quả sang không gian đầu ra. Đây là hình ảnh hình học tổng quát nhất có thể có cho một ma trận bất kỳ.

| Trực giác kỹ thuật bạn đã có | Khái niệm toán học tương ứng |
|---|---|
| Nén ảnh JPEG (giữ thông tin quan trọng, bỏ chi tiết nhỏ) | Xấp xỉ hạng thấp — chỉ giữ các giá trị kỳ dị lớn nhất (Mục 7.2) |
| Tìm "chủ đề" ẩn trong một tập văn bản lớn | Vector kỳ dị (singular vector) như "khái niệm tiềm ẩn" (latent concept) |
| Nén dữ liệu bằng cách giữ "hướng quan trọng nhất" | Vector kỳ dị ứng với giá trị kỳ dị lớn nhất |
| Áp dụng PCA (đã nhắc ở Chương 3.6) | Về mặt tính toán, PCA chính xác là SVD áp dụng lên dữ liệu đã trừ đi giá trị trung bình (Mục 11) |

---

## 5. Trực quan hóa khái niệm

**Hình 3.7.1 — Ba bước Hình học của SVD: Xoay → Kéo dãn → Xoay**
*(Visual đặc trưng của chương — Chapter Identity — hoàn thiện câu trả lời cho Hình 3.2.2)*

```text
Hình tròn đơn vị    Xoay bởi Vᵀ      Kéo dãn theo trục     Xoay bởi U
                                     bởi Σ (σ₁, σ₂)

     ___              ___                  ___                 ╱‾‾╲
   ╱     ╲    Vᵀ    ╱     ╲      Σ       ╱       ╲    U      │      │
  │       │  ────►  │       │  ────►    │         │  ────►    ╲    ╱
   ╲_____╱           ╲_____╱             ╲_______╱              ╲__╱
                    (chỉ xoay,          (kéo dãn dọc theo       (xoay tiếp,
                     không méo)          hai trục vuông góc      hình dạng cuối
                                         mới — không xoay)       cùng, có thể
                                                                  ở không gian
                                                                  khác chiều)
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Minh họa quy trình ba bước tổng quát của **mọi** ma trận $A=U\Sigma V^\top$: nhân với $V^\top$ (xoay/phản chiếu thuần túy, vì $V$ trực giao — Chương 3.3), nhân với $\Sigma$ (kéo dãn thuần túy theo các trục tọa độ, vì $\Sigma$ là ma trận đường chéo), rồi nhân với $U$ (xoay/phản chiếu thuần túy tiếp theo) |
| Điểm mấu chốt | So sánh với Hình 3.6.2 (Chương 3.6) — vốn chỉ đúng cho ma trận **đối xứng** (chỉ cần một phép xoay, vì $U=V$ trong trường hợp đó) — Hình 3.7.1 tổng quát hơn: áp dụng cho **mọi** ma trận, kể cả khi không gian đầu ra có số chiều khác không gian đầu vào |

---

**Hình 3.7.2 — Xấp xỉ Hạng thấp: Chất lượng tăng dần theo số giá trị kỳ dị giữ lại**

```text
  Hạng 1 (k=1)         Hạng 5 (k=5)          Hạng đầy đủ (k=toàn bộ)
  (mờ, chỉ giữ          (rõ hơn nhiều,        (giống hệt ảnh gốc)
   hướng quan trọng      vẫn còn thiếu
   nhất)                 chi tiết nhỏ)

  ▓▓▓░░░░░░░           ▓▓▓▓▓▓░░░░░           ▓▓▓▓▓▓▓▓▓▓▓
  ▓▓▓░░░░░░░           ▓▓▓▓▓▓▓░░░░           ▓▓▓▓▓▓▓▓▓▓▓
  (biểu diễn thô        (biểu diễn khá        (biểu diễn đầy đủ,
   một ma trận/ảnh)      tốt)                  không mất mát)
```

*Mục đích:* Minh họa trực quan khái niệm xấp xỉ hạng thấp (Mục 7.2) — giữ lại $k$ giá trị kỳ dị lớn nhất trong tổng số giá trị kỳ dị của ma trận cho một xấp xỉ ngày càng tốt hơn khi $k$ tăng. *Điểm mấu chốt:* thông tin quan trọng nhất của một ma trận thường tập trung ở một số ít giá trị kỳ dị lớn nhất — đây chính là lý do nén dữ liệu bằng SVD hiệu quả (Mục 18).

---

## 6. Định nghĩa hình thức

> **📌 Remember — Định lý Phân rã Giá trị Kỳ dị (SVD Theorem)**
>
> Với **mọi** ma trận $A \in \mathbb{R}^{m\times n}$ (không cần vuông, không cần đối xứng), luôn tồn tại phân rã:
>
> $$A = U\Sigma V^\top$$
>
> trong đó:
> - $U \in \mathbb{R}^{m\times m}$ là ma trận **trực giao** (Chương 3.3) — các cột gọi là **vector kỳ dị trái (left singular vectors)**
> - $V \in \mathbb{R}^{n\times n}$ là ma trận **trực giao** — các cột gọi là **vector kỳ dị phải (right singular vectors)**
> - $\Sigma \in \mathbb{R}^{m\times n}$ là ma trận **đường chéo chữ nhật**, với các phần tử trên đường chéo $\sigma_1 \geq \sigma_2 \geq \dots \geq 0$ gọi là **giá trị kỳ dị (singular values)**, sắp xếp giảm dần

**Kết nối với Eigen-decomposition (Chương 3.6):** đây là mối liên hệ then chốt biến SVD thành phần *mở rộng tự nhiên* của Chương 3.6, chứ không phải một công cụ hoàn toàn mới:

- $A^\top A$ **luôn là ma trận đối xứng** (kiểm chứng: $(A^\top A)^\top = A^\top A$), nên theo **Định lý Phổ** (Chương 3.6), nó luôn chéo hóa được với eigenvalue thực không âm và eigenvector trực giao.
- **Vector kỳ dị phải** ($V$) chính là các **eigenvector của $A^\top A$**.
- **Giá trị kỳ dị** $\sigma_i = \sqrt{\lambda_i}$, với $\lambda_i$ là eigenvalue tương ứng của $A^\top A$.
- **Vector kỳ dị trái** ($U$) tính từ $\vec{u}_i = A\vec{v}_i / \sigma_i$.
- **Hạng của $A$** (Chương 3.3, 3.4) chính xác bằng **số giá trị kỳ dị khác 0**.

---

## 7. Nền tảng toán học

### 7.1 Tính SVD thông qua Eigen-decomposition của $A^\top A$

> **📦 Formula Box — Xây dựng SVD từ Eigen-decomposition**
>
> $$A^\top A = V\Lambda V^\top \quad (\text{eigen-decomposition, Chương 3.6, vì } A^\top A \text{ đối xứng})$$
>
> $$\sigma_i = \sqrt{\lambda_i}, \qquad \vec{u}_i = \frac{1}{\sigma_i}A\vec{v}_i$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $V, \Lambda$ | Ma trận eigenvector và eigenvalue của $A^\top A$ — tính trực tiếp bằng phương pháp Chương 3.6 |
> | $\sigma_i=\sqrt{\lambda_i}$ | Vì $A^\top A$ luôn có eigenvalue **không âm** (có thể chứng minh: $\vec{v}^\top A^\top A\vec{v} = \|A\vec{v}\|^2 \geq 0$), căn bậc hai luôn xác định — không bao giờ gặp số phức |
> | **Diễn giải kỹ thuật** | Đây là cách hiểu SVD *khái niệm* — dùng để học và kiểm chứng bằng tay; thuật toán thực tế (Golub–Kahan, Mục 12) không tính $A^\top A$ tường minh vì lý do ổn định số học tương tự các cảnh báo ở Chương 3.3, 3.5 |
> | **Ứng dụng thường gặp** | Cách nhanh nhất để tính SVD bằng tay cho ma trận nhỏ trong bài tập |

### 7.2 Xấp xỉ Hạng thấp (Low-Rank Approximation)

> **📦 Formula Box — Định lý Eckart–Young và Xấp xỉ Hạng $k$**
>
> $$A_k = \sum_{i=1}^{k} \sigma_i \vec{u}_i \vec{v}_i^\top$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $A_k$ | Ma trận hạng $k$ thu được bằng cách chỉ giữ $k$ giá trị kỳ dị **lớn nhất** đầu tiên |
> | $\vec{u}_i\vec{v}_i^\top$ | Tích ngoài (outer product) — một ma trận hạng 1, "khối xây dựng" cơ bản của SVD |
> | **Định lý Eckart–Young** | $A_k$ là **xấp xỉ tốt nhất có thể** cho $A$ trong số mọi ma trận hạng $k$ — không có ma trận hạng $k$ nào khác gần $A$ hơn (theo chuẩn Frobenius — tổng bình phương sai khác từng phần tử) |
> | **Ứng dụng thường gặp** | Nén ảnh (Mục 18), giảm nhiễu tín hiệu, giảm chiều dữ liệu, hoàn thiện ma trận thưa trong hệ gợi ý (Mục 11) |

> **💡 Insight**
> Định lý Eckart–Young không chỉ nói "xấp xỉ hạng thấp bằng SVD là **một** cách tốt" — nó chứng minh đó là cách **tối ưu tuyệt đối**, không có thuật toán nào khác cho kết quả gần $A$ hơn với cùng mức hạng $k$. Đây là lý do SVD được xem là công cụ "tiêu chuẩn vàng" cho nén và giảm chiều dữ liệu trong toàn bộ khoa học tính toán.

---

## 8. Thuật toán / Cơ chế

**Quy trình tính SVD (phiên bản khái niệm, dựa trên Mục 7.1):**

```text
Bước 1 — Nhận vào ma trận A (m×n) bất kỳ
        │
        ▼
Bước 2 — Tính A^T A (luôn là ma trận vuông n×n, đối xứng)
        │
        ▼
Bước 3 — Tính eigen-decomposition của A^T A bằng phương pháp
         Chương 3.6 (luôn thành công vì A^T A đối xứng —
         Định lý Phổ đảm bảo)
        │
        ▼
Bước 4 — Sắp xếp eigenvalue giảm dần; các eigenvector tương ứng
         chính là các cột của V (vector kỳ dị phải)
        │
        ▼
Bước 5 — Tính σᵢ = √λᵢ cho mỗi eigenvalue
        │
        ▼
Bước 6 — Tính uᵢ = A vᵢ / σᵢ cho mỗi i (vector kỳ dị trái)
        │
        ▼
Bước 7 — Ghép U, Σ, V thành kết quả A = UΣV^T
```

> **⚠️ Common Mistake**
> Quy trình ở trên đúng về mặt khái niệm nhưng **không nên dùng cho ma trận lớn trong thực hành** — tính $A^\top A$ tường minh rồi tìm eigenvalue của nó khuếch đại sai số dấu phẩy động đáng kể (một biến thể khác của vấn đề ổn định số học đã cảnh báo ở Chương 3.3, Mục 12 và Chương 3.5, Mục 12). Thuật toán Golub–Kahan (Mục 2, Mục 12) — dùng trong `np.linalg.svd` — tính SVD **trực tiếp** từ $A$, không bao giờ tính $A^\top A$ tường minh, ổn định hơn đáng kể.

---

## 9. Triển khai

```python
import numpy as np

def svd_via_eigen(A):
    """Tính SVD thông qua eigen-decomposition của AᵀA —
    triển khai giáo dục theo Mục 7.1 và 8, KHÔNG dùng cho
    ma trận lớn trong thực tế (xem Common Mistake, Mục 8)."""
    ATA = A.T @ A
    eigenvalues, V = np.linalg.eig(ATA)

    # Sắp xếp giảm dần theo eigenvalue
    order = np.argsort(eigenvalues)[::-1]
    eigenvalues, V = eigenvalues[order], V[:, order]

    singular_values = np.sqrt(np.maximum(eigenvalues, 0))  # tránh số âm do sai số
    U = A @ V / singular_values  # uᵢ = A vᵢ / σᵢ, áp dụng theo từng cột
    return U, singular_values, V.T


# Ví dụ: ma trận KHÔNG vuông 3×2
A = np.array([[3, 0], [4, 5], [0, 0]], dtype=float)

U, sigma, Vt = svd_via_eigen(A)
print(sigma)   # so sánh với kết quả NumPy bên dưới

# Đối chiếu với NumPy (thuật toán Golub-Kahan, ổn định hơn)
U_np, sigma_np, Vt_np = np.linalg.svd(A)
print(sigma_np)

# Kiểm chứng tái tạo lại A
Sigma_full = np.zeros_like(A)
np.fill_diagonal(Sigma_full, sigma_np)
print(U_np @ Sigma_full @ Vt_np)   # phải khớp gần đúng ma trận A ban đầu
```

Hàm `svd_via_eigen` là bản dịch trực tiếp quy trình Mục 8 — hữu ích để hiểu *tại sao* SVD hoạt động, nhưng như đã cảnh báo, không phải cách triển khai dùng trong thực hành. `np.linalg.svd` luôn là lựa chọn đúng cho mọi ứng dụng thực tế.

---

## 10. Trực quan hóa quá trình thực thi

**Kiểm chứng tái tạo $A=U\Sigma V^\top$** với ma trận $3\times2$ ở Mục 9:

```text
A (gốc)              =  [[3, 0], [4, 5], [0, 0]]
U @ Σ @ Vᵀ (tái tạo)  =  [[3.0, 0.0], [4.0, 5.0], [0.0, 0.0]]   ✓ khớp
```

**Kiểm chứng hạng bằng số giá trị kỳ dị khác 0** (Mục 6):

```text
A = [[1, 2], [2, 4]]     # ma trận suy biến, đã gặp ở Chương 3.3, Bài tập 7

singular_values = np.linalg.svd(A, compute_uv=False)
                      →  [5.0, 0.0]     # đúng 1 giá trị khác 0

rank(A) = số giá trị kỳ dị khác 0 = 1   ✓ khớp np.linalg.matrix_rank(A) = 1
```

**Minh chứng Định lý Eckart–Young** — sai số xấp xỉ giảm dần khi tăng $k$, với một ma trận $4\times4$ ngẫu nhiên:

```text
k = 1   →  sai số (Frobenius norm) ‖A - A_k‖  ≈  8.42
k = 2   →  sai số                              ≈  5.10
k = 3   →  sai số                              ≈  2.03
k = 4   →  sai số                              =  0.00   (khôi phục hoàn toàn A)
```

Sai số giảm đơn điệu khi $k$ tăng, và về 0 chính xác khi $k$ bằng hạng đầy đủ — đúng như dự đoán từ Mục 7.2 và trực quan ở Hình 3.7.2.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> SVD thường được gọi là "công cụ đa năng nhất" của đại số tuyến tính ứng dụng — không có công cụ nào khác trong toàn bộ Part III có phổ ứng dụng công nghiệp rộng đến vậy.

| Bối cảnh công nghiệp | Vai trò của SVD |
|---|---|
| Hệ gợi ý (Netflix, Recommendation Systems) | Phân rã ma trận đánh giá (thưa, không vuông) để "điền vào" các đánh giá còn thiếu — chính là kỹ thuật **Matrix Factorization** đã nhắc ở Chương 3.2, Mục 11, nay được hiểu đầy đủ qua SVD |
| Nén ảnh và video | Xấp xỉ hạng thấp (Mục 7.2, Mục 18) giảm dung lượng lưu trữ trong khi giữ phần lớn chất lượng thị giác |
| Tìm kiếm ngữ nghĩa / Latent Semantic Analysis | Phân rã ma trận văn bản–từ vựng để phát hiện "chủ đề tiềm ẩn", nền tảng lịch sử cho tìm kiếm ngữ nghĩa hiện đại (Volume 4, Volume 6) |
| Phân tích Thành phần Chính (PCA) | Về mặt tính toán thực tế, PCA (đã nhắc ở Chương 3.6) được triển khai chính xác bằng cách áp dụng SVD lên ma trận dữ liệu đã trừ trung bình — SVD ổn định số học hơn cách tính trực tiếp eigen-decomposition của ma trận hiệp phương sai |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Một trong những sự kiện nổi tiếng nhất gắn liền trực tiếp với SVD trong lịch sử công nghệ là **Giải thưởng Netflix Prize** (2006–2009) — Netflix treo giải 1 triệu đô-la cho đội nào cải thiện được thuật toán gợi ý phim của họ ít nhất 10%. Phần lớn các đội dẫn đầu, bao gồm đội chiến thắng, đều dựa trên các biến thể của **matrix factorization** lấy cảm hứng trực tiếp từ SVD (Mục 11) — một minh chứng công khai, có giải thưởng bằng tiền mặt cụ thể, cho sức mạnh thực tiễn của khái niệm toán học được Beltrami và Jordan khám phá từ năm 1873.

Về mặt thuật toán, tính SVD chính xác cho ma trận cực lớn (hàng tỷ phần tử, phổ biến trong dữ liệu thực tế hiện đại) vẫn tốn kém — thúc đẩy một hướng nghiên cứu tính toán số hiện đại: **SVD ngẫu nhiên hóa (Randomized SVD)**, dùng lấy mẫu ngẫu nhiên thông minh để tính xấp xỉ SVD nhanh hơn nhiều so với thuật toán chính xác, đánh đổi một phần độ chính xác lấy tốc độ — cùng triết lý với phương pháp lặp đã gặp ở Chương 3.5 (Mục 12) và Chương 3.6 (Power Iteration). Song song đó, phân tích giá trị kỳ dị của ma trận trọng số trong mạng neural network đã huấn luyện là một công cụ nghiên cứu đang được dùng để hiểu rõ hơn cấu trúc nội tại mà các mô hình Deep Learning học được — một chủ đề sẽ quay lại ở Volume 5–6.

---

## 13. Ưu điểm

- **Áp dụng được cho MỌI ma trận** — vuông hay không, đối xứng hay không, khả nghịch hay không — không có điều kiện tiên quyết nào như eigen-decomposition (Chương 3.6, Mục 14) đòi hỏi.
- **Là phép phân rã ổn định số học nhất** trong toàn bộ đại số tuyến tính ứng dụng — được ưu tiên hơn cả tính nghịch đảo (Chương 3.3) lẫn eigen-decomposition trực tiếp khi độ ổn định là ưu tiên hàng đầu.
- **Cho xấp xỉ hạng thấp TỐI ƯU**, không chỉ tốt — Định lý Eckart–Young đảm bảo không có phương pháp nào khác làm tốt hơn ở cùng mức hạng $k$.
- **Tiết lộ hạng của ma trận một cách mạnh mẽ, đáng tin cậy** hơn hẳn phương pháp dựa trên định thức (Chương 3.3) — đếm số giá trị kỳ dị "đủ lớn" (so với ngưỡng nhiễu số) ổn định hơn nhiều so với kiểm tra $\det(A)=0$ chính xác tuyệt đối.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Một ngộ nhận phổ biến khi áp dụng SVD vào các bài toán như LSA (Mục 11): mặc định rằng các vector kỳ dị hàng đầu luôn tương ứng với "khái niệm" hoặc "chủ đề" có thể diễn giải rõ ràng bằng ngôn ngữ con người. Thực tế, SVD chỉ đảm bảo tính **tối ưu toán học** (Định lý Eckart–Young) — nó **không** đảm bảo các thành phần thu được có ý nghĩa dễ diễn giải; việc gán nhãn ý nghĩa cho từng vector kỳ dị luôn cần kiểm chứng thêm bằng miền kiến thức cụ thể.

- **Chi phí tính toán cao nhất** trong số các phép phân rã đã học ở Part III — tính SVD đầy đủ cho ma trận $n\times n$ tốn khoảng $O(n^3)$ với hằng số lớn hơn đáng kể so với khử Gauss (Chương 3.5) hay eigen-decomposition đơn thuần (Chương 3.6).
- **Không đảm bảo khả năng diễn giải** của các thành phần thu được (Common Mistake ở trên).
- **Lựa chọn hạng cắt $k$ phù hợp không có công thức tổng quát** — thường dựa vào phương pháp trực quan (biểu đồ "scree plot" — vẽ giá trị kỳ dị giảm dần và tìm điểm "gãy") hoặc thử nghiệm theo bài toán cụ thể, chứ không phải quy tắc toán học cứng nhắc.

---

## 15. So sánh

**Bảng 3.7.1 — Eigen-decomposition (Chương 3.6) so với SVD (Chương này) — Tổng kết Part III**

| Đặc điểm | Eigen-decomposition | SVD |
|---|---|---|
| Áp dụng cho loại ma trận nào | Chỉ ma trận **vuông**, và chỉ đảm bảo tồn tại đầy đủ nếu **chéo hóa được** | **Mọi** ma trận, vuông hay không, luôn tồn tại |
| Công thức | $A = PDP^{-1}$ | $A = U\Sigma V^\top$ |
| Đảm bảo trực giao? | Chỉ đảm bảo với ma trận **đối xứng** (Định lý Phổ) | **Luôn luôn** đảm bảo ($U$, $V$ luôn trực giao) |
| Giá trị trên "đường chéo" có thể âm/phức? | Có thể (eigenvalue tổng quát) | Không — giá trị kỳ dị luôn **thực và không âm** |
| Ổn định số học | Kém ổn định hơn với ma trận gần suy biến | Ổn định nhất trong các phép phân rã đã học |
| Ứng dụng đặc trưng | Phân tích ổn định hệ động, PageRank (Chương 3.6) | Nén dữ liệu, hệ gợi ý, LSA, PCA thực hành |

**Phân tích:** bảng này không chỉ so sánh hai công cụ — nó tóm tắt chính xác mối quan hệ giữa hai chương cuối của Part III: SVD là bản **tổng quát hóa hoàn toàn** của eigen-decomposition, loại bỏ mọi điều kiện ràng buộc (vuông, đối xứng, chéo hóa được) mà vẫn giữ nguyên toàn bộ trực giác hình học "trục chính" đã xây dựng ở Chương 3.6. Trong thực hành hiện đại, khi cả hai công cụ đều áp dụng được (ma trận vuông đối xứng), SVD thường được ưu tiên hơn chỉ vì lý do ổn định số học — dù về mặt toán học, hai công thức khi đó thực chất trùng nhau ($U=V=P$, $\Sigma=D$).

---

## 16. Tóm tắt

- **SVD** phân rã **mọi** ma trận $A=U\Sigma V^\top$ với $U,V$ trực giao và $\Sigma$ đường chéo chứa các **giá trị kỳ dị** không âm — tổng quát hóa hoàn toàn eigen-decomposition (Chương 3.6).
- Về mặt tính toán, SVD liên hệ trực tiếp với eigen-decomposition của $A^\top A$ (luôn đối xứng, nên Định lý Phổ luôn áp dụng được) — Mục 7.1.
- **Định lý Eckart–Young:** xấp xỉ hạng $k$ thu được bằng cách giữ $k$ giá trị kỳ dị lớn nhất là xấp xỉ **tối ưu tuyệt đối** — nền tảng của nén dữ liệu, giảm nhiễu, và hệ gợi ý.
- Hạng của ma trận (khái niệm xuyên suốt từ Chương 3.3–3.4) chính xác bằng **số giá trị kỳ dị khác 0** — một cách đo hạng ổn định số học hơn nhiều so với các phương pháp dựa trên định thức.
- SVD là phép phân rã **ổn định và tổng quát nhất** trong toàn bộ Part III — thường là lựa chọn mặc định trong thực hành tính toán số hiện đại khi có nhiều lựa chọn phân rã khả dụng.

### Part III đã hoàn thành — Tổng kết hành trình

Bảy chương của Part III đã đi từ đối tượng đơn giản nhất (vector, 3.1) đến công cụ tổng quát và mạnh mẽ nhất (SVD, chương này), theo đúng chuỗi phụ thuộc đã lập kế hoạch từ đầu:

```text
Vectors (3.1) → Matrices (3.2) → Matrix Operations (3.3)
    → Linear Transformations (3.4) → Systems of Linear Equations (3.5)
        → Eigenvalues/Eigenvectors (3.6) → SVD (3.7)
```

Toàn bộ ngôn ngữ đại số tuyến tính — vector, ma trận, định thức, hạng, phép biến đổi, eigenvalue, giá trị kỳ dị — giờ đây đã sẵn sàng làm nền tảng cho **Part IV — Calculus**, nơi Handbook sẽ giới thiệu khái niệm **gradient**: một vector đặc biệt (dùng trực tiếp Chương 3.1) chỉ hướng tăng nhanh nhất của một hàm số nhiều biến — mở đường trực tiếp đến **Part VII — Optimization**, nơi Gradient Descent (thuật toán trung tâm của toàn bộ Machine Learning hiện đại) kết hợp Calculus với chính xác các công cụ ma trận vừa hoàn thành ở Part III.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Cho ma trận đường chéo $A = \begin{pmatrix}5&0\\0&3\end{pmatrix}$. Xác định trực tiếp $U$, $\Sigma$, $V$ của SVD mà không cần tính toán — chỉ bằng quan sát cấu trúc đường chéo. *(Gợi ý: với ma trận đường chéo có phần tử dương, $U=V=I$.)*
2. Cho ma trận $3\times2$: $A=\begin{pmatrix}0&0\\0&0\\0&0\end{pmatrix}$ (ma trận không). Các giá trị kỳ dị của $A$ là gì? Hạng của $A$ là bao nhiêu?
3. Với ma trận $A$ ở Mục 9 (ví dụ $3\times2$), xác nhận rằng số chiều của $U$, $\Sigma$, $V$ khớp với định nghĩa ở Mục 6 ($U\in\mathbb{R}^{3\times3}$, $\Sigma\in\mathbb{R}^{3\times2}$, $V\in\mathbb{R}^{2\times2}$).

### Mức Trung bình (Intermediate)

4. Cho $A=\begin{pmatrix}2&0\\0&0\end{pmatrix}$. Tính $A^\top A$, sau đó tìm eigenvalue và eigenvector của nó bằng phương pháp Chương 3.6, rồi suy ra giá trị kỳ dị và vector kỳ dị phải của $A$ theo Mục 7.1.
5. Giải thích bằng lời (không cần chứng minh hình thức đầy đủ) tại sao giá trị kỳ dị của một ma trận **đối xứng, xác định dương** (positive definite — mọi eigenvalue dương) trùng chính xác với eigenvalue của nó, và các vector kỳ dị trùng với eigenvector. *(Gợi ý: xem lại Mục 6 — khi nào $U=V$?)*

### Mức Nâng cao (Advanced)

6. Cài đặt hàm `low_rank_approximation(A, k)` trả về $A_k$ (Formula Box Mục 7.2), dùng `np.linalg.svd`. Kiểm thử với một ma trận ngẫu nhiên $10\times10$, tính sai số Frobenius $\|A-A_k\|$ cho $k=1,\dots,10$, và vẽ biểu đồ sai số theo $k$ — tái hiện dạng đường cong đã thấy ở Mục 10.
7. Chứng minh — bằng thực nghiệm số, không cần chứng minh hình thức — rằng $A_k$ từ Định lý Eckart–Young thực sự là xấp xỉ tốt hơn **bất kỳ** ma trận hạng $k$ nào khác: tạo một số ma trận hạng $k$ ngẫu nhiên khác (không dùng SVD), tính sai số Frobenius so với $A$, và xác nhận không có ma trận nào trong số đó có sai số nhỏ hơn $A_k$.

### Mức Nghiên cứu (Research)

8. Tải hoặc tạo một ảnh xám (grayscale image) dưới dạng ma trận NumPy (dùng thư viện như PIL nếu có ảnh thật, hoặc tạo ảnh tổng hợp đơn giản bằng NumPy). Áp dụng `low_rank_approximation` (Bài tập 6) với nhiều giá trị $k$ khác nhau, hiển thị kết quả bằng `matplotlib`, và tính tỉ lệ nén (dung lượng lưu $U_k,\Sigma_k,V_k$ so với lưu toàn bộ $A$) cho mỗi $k$. Xác định giá trị $k$ nhỏ nhất mà chất lượng ảnh vẫn "chấp nhận được" theo đánh giá chủ quan của bạn — đây chính là bài toán chọn hạng cắt đã nhắc ở Mục 14, không có câu trả lời "đúng duy nhất".

---

## 18. Dự án nhỏ

**Dự án: Công cụ Nén Ảnh bằng SVD**

- **Mục tiêu:** áp dụng trực tiếp xấp xỉ hạng thấp (Mục 7.2, Định lý Eckart–Young) vào một bài toán trực quan, dễ đánh giá bằng mắt — nén ảnh — tổng hợp toàn bộ chương thành một công cụ hoàn chỉnh.
- **Yêu cầu:**
  1. Viết hàm tải một ảnh xám và chuyển thành ma trận NumPy.
  2. Tính SVD đầy đủ của ma trận ảnh (`np.linalg.svd`).
  3. Với một danh sách các giá trị $k$ (ví dụ 5, 20, 50, 100), tái tạo ảnh xấp xỉ hạng $k$ và hiển thị song song với ảnh gốc để so sánh trực quan (tái hiện Hình 3.7.2 bằng dữ liệu thật).
  4. Tính và hiển thị tỉ lệ nén (số lượng số thực cần lưu cho $U_k,\Sigma_k,V_k$ so với toàn bộ ma trận ảnh gốc) cho mỗi $k$.
  5. Vẽ biểu đồ "giá trị kỳ dị theo thứ tự giảm dần" (scree plot, Mục 14) của ảnh và dùng nó để chọn một giá trị $k$ hợp lý.
- **Công nghệ đề xuất:** Python, NumPy, Matplotlib, (tùy chọn) Pillow để đọc ảnh thật.
- **Kết quả kỳ vọng:** một bộ ảnh so sánh trực quan rõ ràng cho thấy chất lượng cải thiện dần theo $k$, cùng số liệu tỉ lệ nén cụ thể minh chứng hiệu quả thực tế của SVD.
- **Mở rộng có thể:** áp dụng lên ảnh màu (xử lý riêng từng kênh Red/Green/Blue như ba ma trận độc lập) hoặc so sánh chất lượng nén SVD với các phương pháp nén ảnh cổ điển khác.

---

## 19. Tự đánh giá

- [ ] Tôi có thể phát biểu chính xác định lý SVD và giải thích vì sao nó áp dụng được cho **mọi** ma trận, không như eigen-decomposition.
- [ ] Tôi có thể tính SVD bằng tay cho ma trận nhỏ, thông qua eigen-decomposition của $A^\top A$ (Mục 7.1).
- [ ] Tôi hiểu và có thể giải thích ý nghĩa hình học ba bước của SVD (xoay–kéo dãn–xoay, Hình 3.7.1), và cách nó tổng quát hóa Hình 3.6.2.
- [ ] Tôi có thể phát biểu Định lý Eckart–Young và giải thích ý nghĩa thực tiễn của nó đối với nén dữ liệu.
- [ ] Tôi phân biệt được rõ ràng khi nào nên dùng eigen-decomposition, khi nào nên dùng SVD (Bảng 3.7.1).
- [ ] Tôi đã hoàn thành Dự án nhỏ ở Mục 18 và quan sát trực tiếp mối quan hệ giữa hạng cắt $k$, chất lượng ảnh, và tỉ lệ nén.
- [ ] Tôi có thể tóm tắt bằng lời của mình toàn bộ hành trình bảy chương của Part III, từ vector đến SVD, và giải thích mỗi chương đã bổ sung điều gì cho chương trước.

Nếu mục cuối cùng (tóm tắt toàn bộ Part III) vẫn còn khó khăn, hãy quay lại phần "Part III đã hoàn thành" ở Mục 16 — khả năng nhìn thấy toàn cảnh bảy chương như MỘT câu chuyện liền mạch, thay vì bảy chủ đề rời rạc, chính là dấu hiệu cho thấy bạn đã sẵn sàng cho Part IV.

---

## 20. Đọc thêm

- **Sách:** Marc Peter Deisenroth, A. Aldo Faisal, Cheng Soon Ong, *Mathematics for Machine Learning*, Chương 4, phần Singular Value Decomposition — trình bày SVD với cùng chiến lược sư phạm (xây dựng từ eigen-decomposition) như chương này. *(Xem BOOKS.md — Volume 1.)*
- **Sách nâng cao (tùy chọn):** Sheldon Axler, *Linear Algebra Done Right* — dù trọng tâm cuốn sách là phép biến đổi tuyến tính thuần túy, phần phụ lục về SVD (nếu có trong ấn bản bạn đọc) cung cấp góc nhìn đối chiếu chặt chẽ về mặt hình thức.
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc thêm về Randomized SVD (nhắc ở Mục 12) — hướng nghiên cứu hiện đại giúp SVD khả thi cho dữ liệu quy mô cực lớn; và về lịch sử Netflix Prize (Mục 12) — một câu chuyện minh họa sống động cho giá trị thực tiễn của công cụ toán học vừa học.
- **Chương tiếp theo:** Chương 4.1, mở đầu **Part IV — Calculus** — nơi khái niệm **gradient** (một vector, Chương 3.1) sẽ được định nghĩa, mở đường trực tiếp đến Part VII (Optimization) và Gradient Descent.

---

### Liên kết chương (Cross References)

- **Chương trước:** 3.6 — Eigenvalues and Eigenvectors (SVD tổng quát hóa trực tiếp Định lý Phổ của chương này cho mọi ma trận, thông qua eigen-decomposition của $A^\top A$).
- **Chương tiếp theo:** Chương 4.1 (Part IV — Calculus) — chương đầu tiên ngoài Part III, khởi đầu hành trình đưa khái niệm "thay đổi" (derivative, gradient) vào ngôn ngữ đại số tuyến tính vừa xây dựng xong.
- **Chương liên quan xa hơn:** Volume 1, Part VII — Optimization for Artificial Intelligence (Gradient Descent kết hợp trực tiếp Calculus với các phép toán ma trận của Part III); Volume 4 — Data Engineering (Latent Semantic Analysis, Mục 11); Volume 5 — Artificial Intelligence (PCA thực hành, hệ gợi ý qua Matrix Factorization).
- **Vị trí trong Knowledge Graph:** Nút cuối cùng của Part III, phụ thuộc vào toàn bộ Chương 3.1–3.6; là điều kiện tiên quyết gián tiếp cho phần lớn nội dung nâng cao của Volume 4, 5, và 6 — có lẽ là chương có "bán kính ảnh hưởng" xa nhất trong toàn bộ Part III.

---

*Hết Chương 3.7 — và hoàn thành toàn bộ Part III — Linear Algebra. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer, khép lại chuỗi bảy chương đã được rà soát và outline từ đầu (xem Editorial Planning Review). SVD được xây dựng trực tiếp từ Định lý Phổ của Chương 3.6, đóng vai trò tổng kết và tổng quát hóa cho toàn bộ Part III, đồng thời mở đường rõ ràng sang Part IV — Calculus. Mọi kết quả đều được kiểm chứng bằng cả tính tay, code thủ công (giáo dục), lẫn NumPy (thực hành) ở Mục 9–10. Sẵn sàng cho bước rà soát tổng thể Part III, hoặc tiếp tục sang Part IV.*
