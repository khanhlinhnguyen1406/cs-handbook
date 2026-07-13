# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 01 — Mathematics for Computer Science
### Part III — Linear Algebra
## Chương 3.5 — Hệ Phương trình Tuyến tính
### (Systems of Linear Equations)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 3.5 |
| Thuộc Part | III — Linear Algebra |
| Thuộc Volume | 01 — Mathematics for Computer Science |
| Thời gian đọc ước tính | 55–65 phút |
| Độ khó | ★★★☆☆ |
| Kiến thức tiên quyết | Chương 3.1–3.4 (bắt buộc — chương này là điểm hội tụ trực tiếp của vector, ma trận, định thức/nghịch đảo/hạng, và span/độc lập tuyến tính) |
| Chương liên quan | 3.6 — Eigenvalues and Eigenvectors (dùng lại kỹ thuật giải hệ phương trình để tìm eigenvector); Volume 1, Part VII — Optimization (đối lập giữa giải chính xác và giải xấp xỉ lặp, Mục 12); Volume 5 — Artificial Intelligence (phương trình chuẩn trong hồi quy tuyến tính) |
| Từ khóa | system of linear equations, augmented matrix, Gaussian elimination, row echelon form, pivot, free variable, consistent system, inconsistent system |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Biểu diễn một hệ phương trình tuyến tính dưới dạng ma trận cô đọng $A\vec{x}=\vec{b}$.
- Xác định chính xác một hệ phương trình thuộc trường hợp nào trong ba trường hợp có thể xảy ra — vô nghiệm, nghiệm duy nhất, vô số nghiệm — dựa trên hạng của $A$ và ma trận mở rộng $[A|b]$.
- Thực hiện phép khử Gauss (Gaussian Elimination) bằng tay cho hệ phương trình nhỏ, đưa về dạng bậc thang (row echelon form) rồi thế ngược (back-substitution).
- Giải thích vì sao khử Gauss được ưa chuộng hơn tính nghịch đảo tường minh trong thực hành tính toán số (hoàn thiện đầy đủ lời hứa còn để ngỏ từ Chương 3.3, Mục 8 và 14).
- Cài đặt thuật toán khử Gauss trong Python và đối chiếu với `np.linalg.solve`.

---

### Câu hỏi khơi gợi

> *Khi huấn luyện một mô hình Machine Learning với hàng triệu tham số nhưng chỉ có hàng nghìn điểm dữ liệu huấn luyện — nghĩa là "phương trình" ít hơn hẳn "ẩn số" — hệ phương trình tương ứng có nghiệm không? Nếu có, nó có bao nhiêu nghiệm, và nghiệm nào trong số đó là "tốt nhất"? Câu trả lời không cần một công cụ toán học mới — nó nằm trọn vẹn trong chính xác những gì Chương 3.4 đã xây dựng: hạng, span, và độc lập tuyến tính.*

---

## 1. Tổng quan chương

Bốn chương trước xây dựng đầy đủ bộ công cụ: vector (3.1), ma trận và phép nhân ma trận (3.2), các phép toán trên ma trận — định thức, nghịch đảo, hạng (3.3), và ý nghĩa chính xác của hạng qua span và độc lập tuyến tính (3.4). Chương này là điểm **hội tụ** của toàn bộ những công cụ đó vào một bài toán cụ thể, có lẽ là bài toán tính toán phổ biến nhất trong toàn bộ khoa học kỹ thuật: giải hệ phương trình tuyến tính $A\vec{x}=\vec{b}$.

Chương này cũng hoàn thành một lời hứa đã để ngỏ từ Chương 3.3, Mục 8: giới thiệu đầy đủ **phép khử Gauss (Gaussian Elimination)** — thuật toán $O(n^3)$ thực sự được dùng trong thực hành, thay thế cho khai triển định thức $O(n!)$ bất khả thi và tính nghịch đảo tường minh kém ổn định số học.

> **💡 Insight**
> Câu hỏi "hệ phương trình này có nghiệm không, và nếu có thì bao nhiêu nghiệm?" nghe có vẻ là một câu hỏi đại số thuần túy — nhưng sau Chương 3.4, bạn sẽ thấy nó thực ra là một câu hỏi **hình học**: bạn đang hỏi liệu vector $\vec{b}$ có nằm trong **span** của các cột ma trận $A$ hay không, và nếu có, các cột đó có **độc lập tuyến tính** đủ để xác định nghiệm duy nhất hay không.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| ~200 TCN | *Cửu chương Toán thuật* (Nine Chapters on the Mathematical Art), Trung Quốc cổ đại | Chương thứ tám của tác phẩm này mô tả một phương pháp giải hệ phương trình tuyến tính **tương đương hoàn toàn** với khử Gauss hiện đại — đi trước Gauss gần 2000 năm |
| Cuối thế kỷ 17 | Isaac Newton | Mô tả một phương pháp khử tương tự trong các ghi chép chưa công bố về giải phương trình đại số |
| ~1810 | Carl Friedrich Gauss | Hệ thống hóa và phổ biến rộng rãi phương pháp khử trong bối cảnh tính toán quỹ đạo thiên văn bằng phương pháp bình phương tối thiểu (least squares) — phương pháp được đặt tên theo ông kể từ đó, dù không phải người phát minh đầu tiên |
| Giữa thế kỷ 20 | James H. Wilkinson | Phân tích chặt chẽ độ ổn định số học (numerical stability) của khử Gauss khi triển khai trên máy tính điện tử dấu phẩy động — đặt nền móng cho lĩnh vực Numerical Linear Algebra hiện đại (liên hệ trực tiếp Chương 3.3, Mục 12) |

Trường hợp *Cửu chương Toán thuật* là một minh chứng thú vị khác (tương tự Grassmann ở Chương 3.1) cho việc các ý tưởng toán học nền tảng thường được khám phá độc lập, nhiều lần, ở nhiều nền văn minh khác nhau, rất lâu trước khi được hình thức hóa và phổ biến rộng rãi dưới tên gọi hiện đại.

---

## 3. Động lực

Ba tình huống kỹ thuật minh họa tầm quan trọng của việc giải hệ phương trình tuyến tính:

- **Hồi quy tuyến tính** (Chương 3.3, Mục 3): tìm hệ số $\beta$ sao cho $X\beta \approx y$ dẫn trực tiếp đến việc giải hệ phương trình tuyến tính (phương trình chuẩn — normal equations).
- **Mô phỏng mạch điện**: định luật Kirchhoff về dòng điện và điện áp trong một mạch điện phức tạp tạo ra một hệ phương trình tuyến tính mà nghiệm chính là dòng điện/điện áp tại từng điểm — phần mềm mô phỏng mạch (SPICE và tương tự) về bản chất là các bộ giải hệ phương trình tuyến tính quy mô lớn.
- **Cân bằng phương trình hóa học**: số nguyên tử mỗi nguyên tố phải bảo toàn ở hai vế phản ứng — một bài toán tưởng chừng thuần hóa học thực chất là một hệ phương trình tuyến tính (Mục 18 sẽ khai thác trực tiếp ví dụ này).

Cả ba tình huống đều cần câu trả lời cho đúng ba câu hỏi: hệ có nghiệm không? nếu có, duy nhất hay vô số? và làm sao tính nghiệm đó một cách hiệu quả, ổn định về mặt số học?

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Giải hệ phương trình tuyến tính giống như tìm **giao điểm** của nhiều đường thẳng (hoặc mặt phẳng, trong không gian nhiều chiều hơn). Khử Gauss là quá trình **đơn giản hóa dần từng bước** — biến các đường "phức tạp, cắt chéo nhau" thành các đường "thẳng đứng, nằm ngang" cho đến khi giao điểm hiện ra rõ ràng, đọc trực tiếp được từ hình vẽ.

| Trực giác kỹ thuật bạn đã có | Khái niệm toán học tương ứng |
|---|---|
| Giải một hệ ràng buộc (constraint solving) trong bài toán tối ưu | Giải hệ phương trình $A\vec{x}=\vec{b}$ |
| Rút gọn dần các biểu thức đại số cho đến khi còn một biến duy nhất | Đưa ma trận về dạng bậc thang (row echelon form) qua khử Gauss |
| Thay ngược giá trị đã biết vào các phương trình còn lại | Thế ngược (back-substitution) |
| Hệ ràng buộc "over-determined" (nhiều ràng buộc hơn biến, có thể mâu thuẫn) | Hệ **vô nghiệm (inconsistent)** — Mục 6 |

---

## 5. Trực quan hóa khái niệm

**Hình 3.5.1 — Ba trường hợp Nghiệm của Hệ 2 Phương trình, 2 Ẩn**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
  Nghiệm DUY NHẤT          VÔ NGHIỆM                VÔ SỐ NGHIỆM
  (hai đường cắt nhau)     (hai đường song song)    (hai đường trùng nhau)

     │  ╱                    │  ╱  ╱                   │
     │ ╱                     │ ╱  ╱                     ╱ (cả hai
   ──┼╱────                ──┼────────                 ╱   phương trình
     ●  ← giao điểm          │ (không bao giờ           là MỘT đường
     │╲                      │  gặp nhau)                duy nhất)
     │ ╲

  rank(A) = rank([A|b])    rank(A) < rank([A|b])    rank(A) = rank([A|b])
  = n (số ẩn)                                        < n (số ẩn)
```

| Trường hợp | Điều kiện (Mục 6) | Ý nghĩa hình học |
|---|---|---|
| Nghiệm duy nhất | $\text{rank}(A) = \text{rank}([A|b]) = n$ | Hai đường (hoặc siêu phẳng) cắt nhau tại đúng một điểm |
| Vô nghiệm | $\text{rank}(A) < \text{rank}([A|b])$ | Hai đường song song, không bao giờ gặp nhau — $\vec{b}$ nằm **ngoài** span của các cột $A$ |
| Vô số nghiệm | $\text{rank}(A) = \text{rank}([A|b]) < n$ | Hai đường trùng nhau — mọi điểm trên đường đó đều là nghiệm |

---

**Hình 3.5.2 — Quá trình Khử Gauss: Ma trận "Bậc thang"**

```text
   Ma trận ban đầu              Sau khử Gauss (Row Echelon Form)

   ┌ 2  1  1 │ 5 ┐              ┌ 2  1   1  │  5  ┐
   │ 4 -1  0 │ 3 │    ──►       │ 0 -3  -2  │ -7  │
   └ 2  2  1 │ 6 ┘              └ 0  0   1  │  1  ┘

   (các hệ số lộn xộn)          (dạng "bậc thang" — mỗi hàng
                                 có nhiều số 0 dẫn đầu hơn
                                 hàng trên nó — đọc nghiệm
                                 từ dưới lên bằng thế ngược)
```

*Mục đích:* Minh họa trực quan mục tiêu của khử Gauss — biến ma trận mở rộng $[A|b]$ ban đầu thành dạng "bậc thang", nơi việc đọc nghiệm trở nên đơn giản (hàng cuối chỉ còn một ẩn, thế ngược dần lên trên). *Điểm mấu chốt:* mỗi bước biến đổi chỉ dùng các **phép toán hàng cơ bản** (Mục 7.1) không làm thay đổi tập nghiệm của hệ — đây là lý do thuật toán hoạt động đúng.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Hệ Phương trình Tuyến tính**
>
> Một hệ gồm $m$ phương trình tuyến tính với $n$ ẩn số được viết cô đọng dưới dạng ma trận:
>
> $$A\vec{x} = \vec{b}, \qquad A \in \mathbb{R}^{m\times n},\ \vec{x} \in \mathbb{R}^n,\ \vec{b} \in \mathbb{R}^m$$
>
> **Ma trận mở rộng (Augmented Matrix)** $[A|b]$ ghép thêm cột $\vec{b}$ vào bên phải $A$ — công cụ thao tác trực tiếp trong khử Gauss.

**Định lý Ba Trường hợp Nghiệm** — kết quả trung tâm của chương, hợp nhất toàn bộ công cụ từ Chương 3.3–3.4:

| Điều kiện trên hạng | Kết luận |
|---|---|
| $\text{rank}(A) < \text{rank}([A|b])$ | Hệ **vô nghiệm (inconsistent)** — $\vec{b}$ nằm ngoài span các cột của $A$ |
| $\text{rank}(A) = \text{rank}([A|b]) = n$ | Hệ có **nghiệm duy nhất** — các cột của $A$ độc lập tuyến tính, đủ để "khóa" nghiệm về đúng một điểm |
| $\text{rank}(A) = \text{rank}([A|b]) < n$ | Hệ có **vô số nghiệm** — tồn tại ít nhất một **biến tự do (free variable)**, có thể nhận giá trị bất kỳ |

Khi $\text{rank}(A) = \text{rank}([A|b])$ (bất kể bằng hay nhỏ hơn $n$), hệ được gọi là **giải được (consistent)**.

**Ba phép toán hàng cơ bản (Elementary Row Operations)** — dùng trong khử Gauss, mỗi phép **không làm thay đổi tập nghiệm** của hệ:

1. Đổi chỗ hai hàng.
2. Nhân một hàng với một số khác 0.
3. Cộng vào một hàng một bội số của hàng khác.

---

## 7. Nền tảng toán học

### 7.1 Khử Gauss (Gaussian Elimination)

> **📦 Formula Box — Quy trình Khử Gauss**
>
> **Bước 1 — Khử xuôi (Forward Elimination):** dùng ba phép toán hàng cơ bản (Mục 6) để biến $[A|b]$ thành **dạng bậc thang (row echelon form)** — mỗi hàng có số "phần tử dẫn đầu khác 0" (gọi là **pivot**) nằm ở cột xa bên trái hơn hàng phía trên nó.
>
> **Bước 2 — Thế ngược (Back-Substitution):** từ hàng cuối cùng (chỉ còn một ẩn), giải ra giá trị của ẩn đó, rồi lần lượt thế ngược lên các hàng phía trên để giải các ẩn còn lại.
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | Pivot | Phần tử khác 0 đầu tiên của mỗi hàng trong dạng bậc thang — dùng làm "trục" để khử các phần tử bên dưới nó về 0 |
> | Free Variable | Cột không có pivot tương ứng — ẩn số này có thể nhận giá trị bất kỳ, dẫn đến vô số nghiệm (Mục 6) |
> | **Diễn giải kỹ thuật** | Vì ba phép toán hàng cơ bản không thay đổi tập nghiệm, dạng bậc thang cuối cùng có **cùng nghiệm hệt** hệ ban đầu — nhưng dễ đọc nghiệm hơn nhiều |
> | **Ứng dụng thường gặp** | Đây chính xác là thuật toán đứng sau `np.linalg.solve` (dù thư viện dùng biến thể tối ưu hơn về ổn định số học — Mục 12) |

### 7.2 Liên hệ với Ma trận Nghịch đảo (Chương 3.3)

Khi $A$ vuông và khả nghịch (điều kiện tương đương: hạng đầy đủ, $\det(A)\neq0$, theo Chương 3.3, Mục 7.3), hệ luôn có **nghiệm duy nhất**, và về mặt lý thuyết, $\vec{x} = A^{-1}\vec{b}$. Tuy nhiên, như đã cảnh báo ở Chương 3.3, Mục 14, đây **chỉ là công thức lý thuyết** — trong thực hành tính toán số, khử Gauss (hoặc các biến thể ổn định hơn, Mục 12) luôn được ưu tiên hơn việc tính $A^{-1}$ tường minh rồi nhân với $\vec{b}$, cả về tốc độ lẫn độ chính xác số học.

---

## 8. Thuật toán / Cơ chế

**Thuật toán Khử Gauss đầy đủ:**

```text
Bước 1 — Ghép A và b thành ma trận mở rộng [A|b]
        │
        ▼
Bước 2 — Với mỗi cột j từ trái sang phải (khử xuôi):
        │
        ▼
Bước 3 —   Tìm hàng có phần tử khác 0 ở cột j (bên dưới
           hàng hiện tại) để làm pivot; nếu không có,
           chuyển sang cột tiếp theo (đây là dấu hiệu free variable)
        │
        ▼
Bước 4 —   Đổi chỗ hàng đó lên vị trí hiện tại nếu cần
        │
        ▼
Bước 5 —   Với mỗi hàng bên dưới pivot: cộng vào hàng đó
           một bội số thích hợp của hàng pivot, sao cho
           phần tử ở cột j của hàng đó trở thành 0
        │
        ▼
Bước 6 — Sau khi khử xuôi xong (dạng bậc thang), thực hiện
         thế ngược từ hàng cuối lên hàng đầu để tính từng ẩn
        │
        ▼
Bước 7 — Kiểm tra: nếu xuất hiện hàng dạng [0...0 | c] với
         c≠0 → hệ vô nghiệm (Mục 6); nếu có cột không pivot
         → tồn tại biến tự do → vô số nghiệm
```

> **💡 Insight**
> Ba vòng lặp lồng nhau trong Bước 2–5 (duyệt cột, duyệt hàng bên dưới, cập nhật từng phần tử) cho thấy trực tiếp độ phức tạp: khử Gauss cho hệ $n$ phương trình, $n$ ẩn tốn $O(n^3)$ — chính xác như đã hứa hẹn từ Chương 3.2 (nhân ma trận) và Chương 3.3 (thay thế cho khai triển định thức $O(n!)$).

---

## 9. Triển khai

```python
import numpy as np

def gaussian_elimination(A, b):
    """Khử Gauss cho hệ Ax=b — triển khai trực tiếp
    thuật toán ở Mục 8 (giả định hệ có nghiệm duy nhất)."""
    n = len(A)
    # Ghép thành ma trận mở rộng [A|b]
    M = [row[:] + [b[i]] for i, row in enumerate(A)]

    # Khử xuôi (Bước 2-5)
    for col in range(n):
        pivot_row = max(range(col, n), key=lambda r: abs(M[r][col]))
        M[col], M[pivot_row] = M[pivot_row], M[col]

        if abs(M[col][col]) < 1e-12:
            raise ValueError("Ma trận suy biến — hệ vô nghiệm hoặc vô số nghiệm.")

        for r in range(col + 1, n):
            factor = M[r][col] / M[col][col]
            for c in range(col, n + 1):
                M[r][c] -= factor * M[col][c]

    # Thế ngược (Bước 6)
    x = [0] * n
    for i in range(n - 1, -1, -1):
        x[i] = (M[i][n] - sum(M[i][j] * x[j] for j in range(i + 1, n))) / M[i][i]
    return x


# Kiểm chứng với ví dụ Hình 3.5.2
A = [[2, 1, 1], [4, -1, 0], [2, 2, 1]]
b = [5, 3, 6]
print(gaussian_elimination(A, b))     # [1.0, 1.0, 2.0]
print(np.linalg.solve(A, b))          # [1. 1. 2.]  ✓ khớp
```

Hàm `gaussian_elimination` chọn pivot có trị tuyệt đối **lớn nhất** trong cột (kỹ thuật gọi là *partial pivoting*) thay vì lấy pivot đầu tiên khác 0 — đây là một cải tiến quan trọng cho ổn định số học (Mục 12), không chỉ để "cho đúng" như thuật toán thô ở Mục 8. Trong thực hành, luôn dùng `np.linalg.solve` — cài đặt trong NumPy còn tinh vi hơn nữa về mặt số học.

---

## 10. Trực quan hóa quá trình thực thi

**Ba trường hợp nghiệm, kiểm chứng bằng hạng:**

```text
# Trường hợp 1 — Nghiệm duy nhất
A = [[1,1],[1,-1]],  b = [4, 0]
rank(A) = 2,  rank([A|b]) = 2,  n = 2  →  nghiệm duy nhất
np.linalg.solve(A, b)  →  [2.0, 2.0]

# Trường hợp 2 — Vô nghiệm
A = [[1,1],[2,2]],  b = [3, 100]      # hai hàng A tỉ lệ, nhưng b thì không
rank(A) = 1,  rank([A|b]) = 2         # 1 < 2  →  vô nghiệm
np.linalg.solve(A, b)  →  numpy.linalg.LinAlgError: Singular matrix

# Trường hợp 3 — Vô số nghiệm
A = [[1,1],[2,2]],  b = [3, 6]        # hai hàng A VÀ b đều tỉ lệ đúng
rank(A) = 1,  rank([A|b]) = 1,  n = 2 # 1 = 1 < 2  →  vô số nghiệm
```

Ba kết quả trên khớp hoàn toàn với bảng điều kiện ở Mục 6 — điểm mấu chốt là Trường hợp 2 và 3 dùng **cùng một ma trận $A$** (hai hàng tỉ lệ nhau, $\det(A)=0$, suy biến — Chương 3.3), nhưng cho **hai kết luận hoàn toàn khác nhau** (vô nghiệm so với vô số nghiệm), tùy thuộc hoàn toàn vào việc $\vec{b}$ có "tình cờ" nằm trong span của các cột $A$ hay không — minh chứng trực tiếp cho Insight ở Mục 1.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Giải hệ phương trình tuyến tính là một trong số ít bài toán toán học có mặt ở *mọi* lĩnh vực kỹ thuật, từ phần mềm khoa học dữ liệu đến mô phỏng vật lý công nghiệp quy mô lớn.

| Bối cảnh công nghiệp | Vai trò của Hệ Phương trình Tuyến tính |
|---|---|
| Phần mềm mô phỏng mạch điện (SPICE và tương tự) | Định luật Kirchhoff sinh ra hệ phương trình tuyến tính khổng lồ; công cụ mô phỏng thực chất là một bộ giải hệ phương trình được tối ưu hóa sâu |
| Phương pháp Phần tử Hữu hạn (Finite Element Method — kỹ thuật xây dựng, hàng không) | Mô phỏng ứng suất, nhiệt độ, dòng chảy chất lỏng trên các cấu trúc phức tạp quy về giải hệ phương trình tuyến tính thưa (sparse) với hàng triệu ẩn số |
| Hồi quy tuyến tính (Linear Regression) | Phương trình chuẩn $(X^\top X)\beta = X^\top y$ — một hệ phương trình tuyến tính theo đúng nghĩa Mục 6 |
| Đồ họa máy tính (Ray Tracing, Inverse Kinematics) | Tính giao điểm tia sáng với bề mặt, hoặc tính góc khớp robot cần thiết để đạt vị trí mong muốn, thường quy về giải (hệ) phương trình tuyến tính cục bộ |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Với hệ phương trình có hàng triệu ẩn số — phổ biến trong mô phỏng kỹ thuật quy mô lớn — khử Gauss $O(n^3)$ trở nên bất khả thi về mặt tính toán, mở ra một hướng nghiên cứu hoàn toàn khác: **phương pháp lặp (iterative methods)**.

Thay vì tìm nghiệm chính xác bằng một chuỗi bước hữu hạn như khử Gauss, các phương pháp lặp (ví dụ **Conjugate Gradient**, đặc biệt hiệu quả với ma trận đối xứng — Chương 3.3, Bảng 3.3.1) bắt đầu từ một nghiệm gần đúng, rồi cải thiện dần qua nhiều vòng lặp cho đến khi đủ chính xác — đánh đổi tính chính xác tuyệt đối lấy khả năng mở rộng lên quy mô cực lớn. Đây chính là cầu nối quan trọng đến **Part VII — Optimization for Artificial Intelligence**: các thuật toán như Gradient Descent, sẽ học ở đó, thực chất cũng là những phương pháp lặp, dù áp dụng cho bài toán tối ưu hóa tổng quát hơn (không chỉ hệ phương trình tuyến tính). Việc lựa chọn "giải chính xác một lần bằng khử Gauss" hay "giải xấp xỉ dần qua nhiều vòng lặp" là một quyết định kỹ thuật nền tảng, tùy thuộc vào quy mô bài toán — chủ đề nghiên cứu vẫn tiếp diễn trong Numerical Linear Algebra hiện đại.

---

## 13. Ưu điểm

- **Cho câu trả lời chính xác, hữu hạn bước** — không như phương pháp lặp (Mục 12), khử Gauss luôn kết thúc sau đúng $O(n^3)$ bước và cho nghiệm chính xác (trong giới hạn sai số dấu phẩy động).
- **Phân loại đầy đủ cả ba trường hợp nghiệm** — không chỉ tìm nghiệm, mà còn tự động phát hiện hệ vô nghiệm hoặc vô số nghiệm (Mục 6, Mục 10).
- **Hợp nhất toàn bộ công cụ từ Chương 3.1–3.4** thành một quy trình tính toán cụ thể, thực dụng — không chỉ là lý thuyết trừu tượng.
- **Nền tảng thuật toán ổn định, đã được nghiên cứu kỹ** (Mục 2, Mục 9 — partial pivoting) qua hơn hai thế kỷ phân tích số học.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Khi hệ có **nhiều ẩn hơn phương trình** (như câu hỏi khơi gợi đầu chương — mô hình có nhiều tham số hơn dữ liệu huấn luyện), hệ gần như chắc chắn rơi vào trường hợp **vô số nghiệm** (Mục 6). Sai lầm phổ biến là nghĩ "vô số nghiệm" nghĩa là bài toán "không giải được" — thực ra, nó có nghĩa là cần thêm một **tiêu chí bổ sung** (ví dụ: chọn nghiệm có độ lớn nhỏ nhất — liên quan trực tiếp đến chuẩn vector, Chương 3.1, Mục 7.2) để chọn ra một nghiệm cụ thể trong vô số nghiệm khả dĩ — đây chính xác là điều các kỹ thuật regularization trong Machine Learning (Volume 5) thực hiện.

- **Độ phức tạp $O(n^3)$ trở nên bất khả thi với hệ cực lớn** (hàng triệu ẩn) — động lực trực tiếp cho phương pháp lặp (Mục 12).
- **Nhạy cảm với sai số dấu phẩy động** khi ma trận gần suy biến (Chương 3.3, Mục 12) — kỹ thuật partial pivoting (Mục 9) giảm thiểu nhưng không loại bỏ hoàn toàn vấn đề này.
- **Chỉ áp dụng trực tiếp cho hệ phương trình tuyến tính** — hệ phương trình phi tuyến (nonlinear) đòi hỏi kỹ thuật hoàn toàn khác (ví dụ tuyến tính hóa cục bộ lặp đi lặp lại), nằm ngoài phạm vi chương này.

---

## 15. So sánh

**Bảng 3.5.1 — Ba Trường hợp Nghiệm: Điều kiện, Hình học, và Ví dụ**

| Trường hợp | Điều kiện Hạng | Hình ảnh Hình học (Hình 3.5.1) | Ví dụ Kỹ thuật |
|---|---|---|---|
| Nghiệm duy nhất | $\text{rank}(A)=\text{rank}([A|b])=n$ | Các siêu phẳng cắt nhau tại đúng một điểm | Hệ có số phương trình = số ẩn, ma trận hệ số khả nghịch (mạch điện xác định đủ) |
| Vô nghiệm | $\text{rank}(A)<\text{rank}([A|b])$ | Các siêu phẳng song song, không giao nhau | Hệ ràng buộc mâu thuẫn nhau (constraint không thể đồng thời thỏa mãn) |
| Vô số nghiệm | $\text{rank}(A)=\text{rank}([A|b])<n$ | Các siêu phẳng trùng nhau một phần (giao nhau theo cả một đường/mặt) | Mô hình có nhiều tham số hơn dữ liệu (underdetermined system, Mục 14) |

**Phân tích:** bảng này là bản tổng kết cô đọng nhất của toàn chương — mọi hệ phương trình tuyến tính, dù phức tạp đến đâu, luôn rơi vào đúng một trong ba hàng của bảng trên, và việc xác định hàng nào chỉ cần tính hai giá trị hạng. Đây là một trong những kết quả "đóng" (closed-form classification) gọn gàng nhất trong toàn bộ đại số tuyến tính — không có trường hợp thứ tư nào có thể xảy ra.

---

## 16. Tóm tắt

- Một **hệ phương trình tuyến tính** được viết cô đọng dưới dạng $A\vec{x}=\vec{b}$; **ma trận mở rộng** $[A|b]$ là công cụ thao tác trực tiếp.
- Đúng **ba trường hợp** có thể xảy ra — vô nghiệm, nghiệm duy nhất, vô số nghiệm — xác định hoàn toàn bởi so sánh $\text{rank}(A)$, $\text{rank}([A|b])$, và số ẩn $n$ (Mục 6, Bảng 3.5.1).
- **Khử Gauss** (khử xuôi + thế ngược) là thuật toán $O(n^3)$ thực dụng để giải hệ, thay thế hoàn toàn nhu cầu tính nghịch đảo tường minh (Mục 7.2) — hoàn thành lời hứa từ Chương 3.3.
- Với hệ cực lớn, khử Gauss trở nên bất khả thi, dẫn đến các **phương pháp lặp** (Mục 12) — cầu nối trực tiếp đến Part VII (Optimization, Gradient Descent).
- Chương này hợp nhất **mọi** khái niệm đã học trong Part III tính đến nay: vector, ma trận, định thức, nghịch đảo, hạng, span, độc lập tuyến tính — tất cả cùng phục vụ một câu hỏi duy nhất: hệ này có nghiệm không, và nếu có, làm sao tìm nó hiệu quả và ổn định.

Chương 3.6 (Eigenvalues and Eigenvectors) sẽ dùng lại trực tiếp kỹ thuật giải hệ phương trình tuyến tính vừa học — tìm eigenvector, về bản chất, chính là giải một hệ phương trình tuyến tính đặc biệt dạng $(A-\lambda I)\vec{v}=\vec{0}$.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Viết hệ phương trình sau dưới dạng ma trận $A\vec{x}=\vec{b}$: $\begin{cases}2x+3y=7\\ x-y=1\end{cases}$
2. Giải hệ ở Bài tập 1 bằng khử Gauss thủ công (khử xuôi rồi thế ngược), trình bày rõ từng bước.
3. Cho hệ $\begin{cases}x+y=2\\ 2x+2y=5\end{cases}$. Tính $\text{rank}(A)$ và $\text{rank}([A|b])$. Hệ này thuộc trường hợp nào trong ba trường hợp ở Bảng 3.5.1?

### Mức Trung bình (Intermediate)

4. Cho hệ $\begin{cases}x+2y-z=3\\ 2x+4y-2z=6\end{cases}$ (2 phương trình, 3 ẩn). Chứng minh hệ này có vô số nghiệm bằng cách tính hạng, sau đó biểu diễn tập nghiệm theo một biến tự do (chọn $z$ làm biến tự do, biểu diễn $x, y$ theo $z$).
5. Giải thích bằng ngôn ngữ hình học (Hình 3.5.1) tại sao một hệ 3 phương trình, 2 ẩn (nhiều phương trình hơn ẩn) thường vô nghiệm, trừ khi có sự "trùng hợp" đặc biệt. *(Gợi ý: liên hệ với khái niệm span trong không gian 2 chiều từ ba đường thẳng.)*

### Mức Nâng cao (Advanced)

6. Sửa hàm `gaussian_elimination` (Mục 9) để tự động phát hiện và báo cáo chính xác cả ba trường hợp ở Bảng 3.5.1 (hiện tại hàm giả định luôn có nghiệm duy nhất), thay vì chỉ báo lỗi chung chung "ma trận suy biến".
7. Cân bằng phương trình hóa học sau bằng cách thiết lập và giải hệ phương trình tuyến tính (số nguyên tử mỗi nguyên tố phải bằng nhau hai vế): $\text{C}_3\text{H}_8 + \text{O}_2 \to \text{CO}_2 + \text{H}_2\text{O}$. *(Gợi ý: gọi hệ số cân bằng là $a,b,c,d$; viết một phương trình tuyến tính cho mỗi nguyên tố C, H, O.)*

### Mức Nghiên cứu (Research)

8. So sánh thời gian chạy của `gaussian_elimination` (Mục 9) với `np.linalg.solve` cho các hệ vuông kích thước $n=10, 50, 100, 300$ (dùng ma trận và vector ngẫu nhiên, đảm bảo khả nghịch). Kết quả có khớp với dự đoán $O(n^3)$ ở Mục 8 không? Đồng thời, thử nghiệm với một ma trận **gần suy biến** (tương tự Chương 3.3, Bài tập 8) và quan sát xem `np.linalg.solve` có báo lỗi hoặc cảnh báo gì đặc biệt không so với ma trận thông thường. Đây là bài tập mang tính khám phá thực nghiệm, kết nối trực tiếp Mục 12 và 14.

---

## 18. Dự án nhỏ

**Dự án: Bộ Cân bằng Phương trình Hóa học Tự động**

- **Mục tiêu:** áp dụng toàn bộ chương — thiết lập hệ phương trình tuyến tính từ một bài toán thực tế thuộc lĩnh vực khác (hóa học), giải bằng khử Gauss, và diễn giải kết quả — minh họa tính phổ quát của công cụ toán học vừa học.
- **Yêu cầu:**
  1. Viết một hàm nhận vào công thức các chất phản ứng và sản phẩm (dạng đơn giản hóa, ví dụ dictionary đếm số nguyên tử mỗi nguyên tố trong mỗi phân tử).
  2. Tự động thiết lập ma trận hệ số $A$ (mỗi hàng là một nguyên tố, mỗi cột là một chất) từ dữ liệu đầu vào.
  3. Giải hệ (lưu ý: đây thường là hệ **vô số nghiệm** một chiều — Mục 6 — cần chuẩn hóa nghiệm về số nguyên nhỏ nhất có thể, một bước xử lý hậu kỳ ngoài phạm vi khử Gauss thuần túy).
  4. Kiểm thử với ít nhất ba phản ứng hóa học khác nhau, bao gồm ví dụ ở Bài tập 7.
- **Công nghệ đề xuất:** Python, NumPy hoặc `gaussian_elimination` tự cài đặt (Mục 9).
- **Kết quả kỳ vọng:** hệ số cân bằng chính xác, kiểm chứng bằng tay cho ít nhất một phản ứng.
- **Mở rộng có thể:** mở rộng để xử lý phản ứng có nhiều nghiệm cân bằng độc lập (hệ có hạng còn nhỏ hơn, nhiều biến tự do hơn) — trường hợp hiếm nhưng có thật trong hóa học hữu cơ phức tạp.

---

## 19. Tự đánh giá

- [ ] Tôi có thể viết một hệ phương trình tuyến tính dưới dạng ma trận $A\vec{x}=\vec{b}$ và thiết lập ma trận mở rộng $[A|b]$.
- [ ] Tôi có thể thực hiện khử Gauss bằng tay cho hệ 2–3 phương trình, bao gồm cả khử xuôi lẫn thế ngược.
- [ ] Tôi có thể xác định một hệ phương trình thuộc trường hợp nào trong ba trường hợp (Bảng 3.5.1) chỉ bằng cách tính và so sánh hạng, không cần giải toàn bộ hệ.
- [ ] Tôi hiểu và có thể giải thích bằng lời vì sao khử Gauss được ưa chuộng hơn tính nghịch đảo tường minh trong thực hành.
- [ ] Tôi phân biệt được ý nghĩa của "vô số nghiệm" — không phải "không giải được", mà là cần thêm tiêu chí bổ sung để chọn nghiệm cụ thể (Mục 14, Common Mistake).
- [ ] Tôi đã hoàn thành Dự án nhỏ ở Mục 18 và cân bằng chính xác được ít nhất một phương trình hóa học bằng công cụ tự xây dựng.

Nếu Bài tập 5 (giải thích hình học vì sao hệ có nhiều phương trình hơn ẩn thường vô nghiệm) vẫn còn mơ hồ, hãy quay lại Hình 3.4.2 (Chương 3.4) và Hình 3.5.1 — trực giác "span" là chìa khóa hợp nhất cả hai hình ảnh đó.

---

## 20. Đọc thêm

- **Sách:** Marc Peter Deisenroth, A. Aldo Faisal, Cheng Soon Ong, *Mathematics for Machine Learning*, Chương 2, phần Hệ Phương trình Tuyến tính và Khử Gauss. *(Xem BOOKS.md — Volume 1.)*
- **Sách nâng cao (tùy chọn):** Sheldon Axler, *Linear Algebra Done Right* — góc nhìn giải hệ phương trình qua ngôn ngữ ánh xạ tuyến tính, nhất quán với Chương 3.4.
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc thêm về phương pháp Conjugate Gradient (nhắc ở Mục 12) — công cụ trung tâm khi giải hệ phương trình tuyến tính đối xứng quy mô cực lớn, thường gặp lại ở Volume 4 (Data Engineering) và Volume 7 (Research Methodology, mô phỏng số).
- **Chương tiếp theo:** Chương 3.6 — Eigenvalues and Eigenvectors.

---

### Liên kết chương (Cross References)

- **Chương trước:** 3.4 — Linear Transformations (span và độc lập tuyến tính, dùng trực tiếp để phát biểu Định lý Ba Trường hợp Nghiệm ở Mục 6).
- **Chương tiếp theo:** 3.6 — Eigenvalues and Eigenvectors (tìm eigenvector, về bản chất, là giải hệ phương trình tuyến tính đặc biệt $(A-\lambda I)\vec{v}=\vec{0}$, dùng trực tiếp kỹ thuật của chương này).
- **Chương liên quan xa hơn:** Volume 1, Part VII — Optimization for Artificial Intelligence (đối lập giữa phương pháp giải chính xác một bước và phương pháp lặp — Mục 12 — là tiền đề trực tiếp cho Gradient Descent); Volume 5 — Artificial Intelligence (phương trình chuẩn trong hồi quy tuyến tính, Mục 3 và 11).
- **Vị trí trong Knowledge Graph:** Nút thứ năm của Part III, phụ thuộc trực tiếp vào toàn bộ Chương 3.1–3.4; là điều kiện tiên quyết trực tiếp cho Chương 3.6, và là cầu nối khái niệm sang Part VII.

---

*Hết Chương 3.5. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer, tiếp nối trực tiếp Chương 3.4 theo outline Part III đã đề xuất (bổ sung — xem Editorial Planning Review, Mục 6). Chương này hoàn thành lời hứa về thuật toán khử Gauss đã để ngỏ từ Chương 3.2–3.3, và hợp nhất toàn bộ công cụ của Part III tính đến nay vào một quy trình giải toán cụ thể, có kiểm chứng bằng cả tính tay, code thủ công, lẫn NumPy ở Mục 9–10. Đang chờ rà soát trước khi tiếp tục sang Chương 3.6 — Eigenvalues and Eigenvectors.*
