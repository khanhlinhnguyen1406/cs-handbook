# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 03 — Algorithms and Data Structures
### Part III — Algorithm Design Paradigms
## Chương 19 — Quy hoạch Động II: Kỹ thuật Nâng cao
### (Dynamic Programming II: Advanced Techniques)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 19 |
| Thuộc Part | III — Algorithm Design Paradigms |
| Thuộc Volume | 03 — Algorithms and Data Structures |
| Thời gian đọc ước tính | 65–75 phút |
| Độ khó | ★★★★☆ |
| Kiến thức tiên quyết | Chương 18 — Dynamic Programming I (Optimal Substructure, Overlapping Subproblems, Memoization, Tabulation); Volume 3, Part II — Tree, Array |
| Chương liên quan | 18 — Dynamic Programming I (nền tảng bắt buộc); 20 — Backtracking (đối chiếu: khi nào nên dùng DP thay vì quay lui thuần túy); Volume 5/6 — Reinforcement Learning, Sequence Models (mở rộng tư duy Bellman Equation) |
| Từ khóa | edit distance, matrix chain multiplication, bitmask dp, tree dp, rolling array, space optimization |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Thiết lập và triển khai công thức truy hồi cho hai bài toán DP trên chuỗi nâng cao: Edit Distance và Matrix Chain Multiplication.
- Áp dụng kỹ thuật **Bitmask DP** để biểu diễn trạng thái là một tập con, giải các bài toán mà không gian trạng thái vốn có bản chất tổ hợp.
- Nhận diện cấu trúc **DP trên cây (Tree DP)**, nơi bài toán con tương ứng với các cây con của một cây cho trước.
- Áp dụng kỹ thuật **tối ưu không gian (rolling array)** để giảm độ phức tạp bộ nhớ từ $O(n \times m)$ xuống $O(m)$ khi công thức truy hồi chỉ phụ thuộc vào hàng liền kề.
- Nhận diện được khi nào một bài toán cần biểu diễn trạng thái phức tạp hơn một chỉ số đơn giản (như đã thấy ở Chương 18), và cách xây dựng biểu diễn trạng thái phù hợp.

---

### Câu hỏi khơi gợi

> *Khi bạn gõ nhầm một từ và trình soạn thảo văn bản gợi ý "có phải bạn muốn gõ...", làm sao máy tính biết được từ nào trong hàng chục nghìn từ trong từ điển là "gần đúng nhất" với những gì bạn vừa gõ sai? Và tại sao bài toán tưởng chừng chỉ liên quan đến ngôn ngữ này lại có thể giải bằng chính công thức toán học tương tự với bài toán nhân một chuỗi ma trận theo thứ tự tối ưu?*

---

## 1. Tổng quan chương

Chương 18 đã trang bị nền tảng của Dynamic Programming: hai điều kiện (Optimal Substructure, Overlapping Subproblems), hai kỹ thuật cài đặt (Memoization, Tabulation), và hai ví dụ kinh điển trên bảng 2 chiều đơn giản (0/1 Knapsack, Longest Common Subsequence). Chương này mở rộng bộ công cụ đó theo bốn hướng nâng cao, mỗi hướng giải quyết một dạng bài toán mà cấu trúc trạng thái phức tạp hơn đáng kể so với Chương 18.

Chương này có bốn mục tiêu song song. Thứ nhất, **DP trên chuỗi nâng cao**: Edit Distance (đo độ "khác biệt" giữa hai chuỗi bằng số phép biến đổi tối thiểu) và Matrix Chain Multiplication (tìm thứ tự nhân ma trận tối ưu) — cả hai đều dùng bảng 2 chiều như Chương 18, nhưng với công thức truy hồi phức tạp hơn. Thứ hai, **Bitmask DP**: một kỹ thuật biểu diễn trạng thái bằng một số nguyên, trong đó mỗi bit đại diện cho việc một phần tử có "thuộc tập hợp hiện tại" hay không — hồi tưởng lại trực tiếp lý thuyết tập hợp đã học ở Volume 1. Thứ ba, **DP trên cây**: mở rộng tư duy DP từ chuỗi/mảng sang cấu trúc cây, nơi bài toán con là các cây con. Thứ tư, **tối ưu không gian**: một kỹ thuật cài đặt (không phải một bài toán mới) giúp giảm đáng kể bộ nhớ sử dụng, giải quyết trực tiếp câu hỏi mở đã đặt ra ở Chương 18, Mục 12.

> **💡 Insight**
> Điểm chung xuyên suốt bốn kỹ thuật của chương này không phải là "công thức mới" — mà là **cách biểu diễn trạng thái (state) trở nên tinh vi hơn**. Ở Chương 18, trạng thái chỉ là một hoặc hai chỉ số đơn giản ($i$, hoặc $(i,j)$). Ở chương này, trạng thái có thể là một khoảng $(i,j)$ với ý nghĩa hoàn toàn khác (Matrix Chain), một tập con được mã hóa nhị phân (Bitmask), hoặc một nút trong cây (Tree DP). Kỹ năng cốt lõi cần rèn luyện: **nhận diện đúng "trạng thái" cần biểu diễn** trước khi nghĩ đến công thức truy hồi.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1965 | Vladimir Levenshtein | Định nghĩa **Khoảng cách Levenshtein (Levenshtein Distance)** — chính là Edit Distance — làm nền tảng lý thuyết mã hóa sửa lỗi (error-correcting codes) |
| 1968 | Robert W. Floyd (cùng nhiều đóng góp khác về DP) | Góp phần định hình các kỹ thuật DP tổng quát trên cấu trúc bảng, bao gồm các bài toán liên quan đến chuỗi phép toán tối ưu (tiền thân của Matrix Chain Multiplication) |
| Thập niên 1950–1960 | S. Godbole và các nhà nghiên cứu vận trù học (operations research) | Hình thức hóa bài toán Matrix Chain Multiplication như một ví dụ ứng dụng thực tế của Dynamic Programming trong tính toán khoa học |
| Thập niên 1970–1980 | Cộng đồng nghiên cứu tối ưu hóa tổ hợp | Phát triển kỹ thuật Bitmask DP như một công cụ chuẩn cho các bài toán có không gian trạng thái nhỏ nhưng mang bản chất tổ hợp (ví dụ Traveling Salesman Problem, sẽ gặp ở Chương 21) |

> **🔬 Research Connection**
> Edit Distance không chỉ là một bài toán học thuật — nó là nền tảng của **lý thuyết mã hóa sửa lỗi (error-correcting codes)**, một lĩnh vực cực kỳ quan trọng trong truyền thông số (viễn thông, lưu trữ dữ liệu). Khi dữ liệu bị nhiễu trong quá trình truyền, khoảng cách Levenshtein (và các biến thể của nó, như khoảng cách Hamming) giúp xác định thông điệp gốc có khả năng cao nhất đã được gửi đi, dựa trên thông điệp nhiễu nhận được.

---

## 3. Động lực

Hãy xét bài toán kiểm tra chính tả: người dùng gõ nhầm từ "recieve" (đúng ra là "receive"). Làm sao hệ thống định lượng được "recieve" và "receive" gần nhau đến mức nào, để gợi ý đúng từ thay thế trong số hàng nghìn từ có thể?

Câu trả lời: đếm số phép **chèn (insert)**, **xóa (delete)**, hoặc **thay thế (substitute)** ký tự tối thiểu để biến chuỗi này thành chuỗi kia — đây chính là bài toán **Edit Distance**. Với "recieve" → "receive", chỉ cần một phép hoán đổi vị trí hai ký tự "ie" thành "ei" (thực chất là một phép xóa + một phép chèn, hoặc tùy định nghĩa biến thể, một phép thay thế) — khoảng cách rất nhỏ, nên hệ thống tự tin gợi ý "receive".

Bài toán này, dù thuộc lĩnh vực xử lý ngôn ngữ hoàn toàn khác với Matrix Chain Multiplication (một bài toán đại số tuyến tính), lại chia sẻ **cùng một cấu trúc toán học**: cả hai đều là bài toán tối ưu hóa trên một **khoảng** (interval) — Edit Distance trên khoảng của hai chuỗi, Matrix Chain trên khoảng chỉ số ma trận cần nhân. Đây chính là động lực cốt lõi của phần đầu chương: nhận ra rằng nhiều bài toán tưởng chừng khác biệt hoàn toàn thực chất chia sẻ chung một khuôn mẫu DP.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Nếu Chương 18 dạy bạn "ghi sổ tay" cho các bài toán con được đánh số bằng một hoặc hai chỉ số đơn giản, chương này dạy bạn cách **thiết kế lại chính cuốn sổ tay đó** để phù hợp với hình dạng thực sự của bài toán: đôi khi cuốn sổ cần được đánh số theo **khoảng** (như một cuốn lịch ghi "từ ngày $i$ đến ngày $j$"), đôi khi cần đánh số theo **tập hợp** (như một cuốn sổ ghi "đã đi qua đúng những thành phố nào trong danh sách"), và đôi khi cuốn sổ cần có hình dạng của chính **cấu trúc cây** đang được xét.

| Trực giác đời thường | Khái niệm thuật toán tương ứng |
|---|---|
| Sổ tay ghi theo "từ ngày $i$ đến ngày $j$" | DP trên khoảng (interval DP) — Matrix Chain Multiplication |
| Sổ tay ghi theo "đã đi qua đúng tập thành phố nào" | Bitmask DP — trạng thái là một tập con |
| Sổ tay có hình dạng giống cây gia phả | Tree DP — bài toán con tương ứng với cây con |
| Chỉ cần giữ lại trang sổ "hôm qua" và "hôm nay", không cần giữ mọi trang cũ | Rolling Array — tối ưu không gian |

---

## 5. Trực quan hóa khái niệm

**Hình 19.1 — Bảng Edit Distance giữa "HORSE" và "ROS"**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
        ""   R    O    S
    ""   0   1    2    3
    H    1   1    2    3
    O    2   2    1    2
    R    3   2    2    2
    S    4   3    3    2
    E    5   4    4    3
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Minh họa cấu trúc bảng Edit Distance — tương tự bảng LCS ở Chương 18 (Hình 18.2) về hình dạng, nhưng công thức truy hồi khác (Mục 7.1) vì phép toán cho phép là chèn/xóa/thay thế, không chỉ "bỏ qua" |
| Điểm mấu chốt | Giá trị góc dưới phải ($3$) là số phép biến đổi tối thiểu để biến "HORSE" thành "ROS" — so sánh trực tiếp với bảng LCS để thấy hai bài toán tuy khác mục tiêu nhưng cùng cấu trúc bảng |

---

**Hình 19.2 — Không gian trạng thái Bitmask cho bài toán TSP rút gọn (4 thành phố)**

```text
Với 4 thành phố {A, B, C, D}, mỗi trạng thái được biểu diễn bằng
một số nhị phân 4-bit, bit thứ i = 1 nghĩa là đã ghé thăm thành phố đó:

  0000 = {}           (chưa thăm thành phố nào)
  0001 = {A}          0010 = {B}          0100 = {C}          1000 = {D}
  0011 = {A,B}        0101 = {A,C}        1001 = {A,D}        ...
  0111 = {A,B,C}      1011 = {A,B,D}      ...
  1111 = {A,B,C,D}    (đã thăm tất cả)

Trạng thái DP: dp[mask][i] = chi phí tối thiểu để thăm đúng tập
"mask", kết thúc tại thành phố i.
```

*Mục đích:* minh họa trực tiếp mối liên hệ giữa Bitmask DP và Lý thuyết Tập hợp (Volume 1, Chương 1.5) — mỗi số nguyên từ $0$ đến $2^n - 1$ mã hóa chính xác một tập con, đúng như thuật toán sinh Tập lũy thừa đã học. *Điểm mấu chốt:* đây là lý do độ phức tạp không gian trạng thái của Bitmask DP luôn có yếu tố $2^n$ — chỉ khả thi khi $n$ đủ nhỏ (thường dưới 20–25).

---

## 6. Định nghĩa hình thức

> **📌 Remember — Bốn kỹ thuật nâng cao của chương này**
>
> - **Interval DP (DP trên khoảng):** trạng thái được biểu diễn bằng một cặp chỉ số $(i, j)$ đại diện cho một **khoảng con liên tục** của dữ liệu gốc (chuỗi, mảng, dãy ma trận). Công thức truy hồi thường xét mọi cách "chia đôi" khoảng tại một điểm $k$ ở giữa.
> - **Bitmask DP:** trạng thái được biểu diễn bằng một số nguyên $mask$ trong khoảng $[0, 2^n)$, trong đó bit thứ $i$ của $mask$ cho biết phần tử thứ $i$ có thuộc tập hợp hiện tại hay không.
> - **Tree DP:** trạng thái được biểu diễn bằng một nút của cây, và công thức truy hồi kết hợp kết quả từ các **cây con trực tiếp** của nút đó.
> - **Rolling Array:** một kỹ thuật cài đặt (không phải một dạng bài toán mới) trong đó bảng DP nhiều chiều được thu gọn, chỉ giữ lại số hàng/cột tối thiểu cần thiết theo công thức truy hồi, thường giảm độ phức tạp không gian từ $O(n \times m)$ xuống $O(m)$ hoặc $O(1)$.

---

## 7. Nền tảng toán học

### 7.1 Công thức truy hồi Edit Distance

Cho hai chuỗi $X$ (độ dài $m$), $Y$ (độ dài $n$). Gọi $D(i,j)$ là số phép biến đổi tối thiểu để biến $i$ ký tự đầu của $X$ thành $j$ ký tự đầu của $Y$.

> **📦 Formula Box — Công thức truy hồi Edit Distance**
>
> $$
> D(i,j) =
> \begin{cases}
> i & \text{nếu } j = 0 \quad \text{(xóa hết } i \text{ ký tự)} \\
> j & \text{nếu } i = 0 \quad \text{(chèn hết } j \text{ ký tự)} \\
> D(i-1, j-1) & \text{nếu } X_i = Y_j \quad \text{(không cần biến đổi)} \\
> 1 + \min\big(D(i-1,j),\ D(i,j-1),\ D(i-1,j-1)\big) & \text{nếu } X_i \neq Y_j
> \end{cases}
> $$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $D(i-1, j)$ | Phương án **xóa** ký tự cuối của $X$ |
> | $D(i, j-1)$ | Phương án **chèn** ký tự cuối của $Y$ vào $X$ |
> | $D(i-1, j-1)$ (trong trường hợp $X_i \neq Y_j$) | Phương án **thay thế** ký tự cuối của $X$ bằng ký tự cuối của $Y$ |
> | **Diễn giải kỹ thuật** | Ba phương án tương ứng chính xác với ba phép biến đổi cơ bản được phép; công thức chọn phương án tốn ít phép biến đổi nhất, cộng thêm 1 cho chính phép biến đổi vừa thực hiện |
> | **Ứng dụng thường gặp** | Kiểm tra chính tả, so khớp DNA gần đúng (fuzzy matching), tính điểm tương đồng văn bản |

### 7.2 Công thức truy hồi Matrix Chain Multiplication

Cho chuỗi ma trận $A_1, A_2, \dots, A_n$ với kích thước tương ứng $p_0 \times p_1, p_1 \times p_2, \dots, p_{n-1} \times p_n$. Gọi $M(i,j)$ là số phép nhân vô hướng (scalar multiplication) tối thiểu để tính tích $A_i \times A_{i+1} \times \dots \times A_j$.

> **📦 Formula Box — Công thức truy hồi Matrix Chain Multiplication**
>
> $$
> M(i,j) =
> \begin{cases}
> 0 & \text{nếu } i = j \\
> \min\limits_{i \leq k < j} \big( M(i,k) + M(k+1,j) + p_{i-1} \cdot p_k \cdot p_j \big) & \text{nếu } i < j
> \end{cases}
> $$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $k$ | Vị trí "chia đôi" chuỗi ma trận: nhân nhóm từ $i$ đến $k$ trước, nhóm từ $k+1$ đến $j$ sau, rồi nhân hai kết quả với nhau |
> | $M(i,k) + M(k+1,j)$ | Chi phí đệ quy của hai nhóm con |
> | $p_{i-1} \cdot p_k \cdot p_j$ | Chi phí của phép nhân cuối cùng — nhân hai ma trận kết quả kích thước $p_{i-1} \times p_k$ và $p_k \times p_j$ |
> | **Diễn giải kỹ thuật** | Đây là ví dụ điển hình của **Interval DP**: với mỗi khoảng $(i,j)$, ta thử **mọi** điểm chia $k$ có thể và chọn điểm chia cho tổng chi phí thấp nhất — khác với LCS/Edit Distance (Chương 18, Mục 7.1 chương này), nơi mỗi ô chỉ phụ thuộc vào tối đa 3 ô lân cận cố định |
> | **Độ phức tạp** | $O(n^3)$ — vì có $O(n^2)$ khoảng $(i,j)$, và với mỗi khoảng cần thử $O(n)$ điểm chia $k$ |

---

## 8. Thuật toán / Cơ chế

### 8.1 Interval DP cho Matrix Chain Multiplication

```text
Bước 1 — Khởi tạo M(i,i) = 0 với mọi i (một ma trận đơn không
           cần phép nhân nào)
        │
        ▼
Bước 2 — Với mỗi độ dài khoảng L từ 2 đến n (quan trọng: phải
           duyệt theo độ dài khoảng tăng dần, để khi tính M(i,j)
           thì mọi M(i,k) và M(k+1,j) với khoảng ngắn hơn đã có sẵn)
        │
        ▼
Bước 3 —   Với mỗi vị trí bắt đầu i (suy ra j = i + L - 1):
        │
        ▼
Bước 4 —     Thử mọi điểm chia k từ i đến j-1, áp dụng công thức
             ở Mục 7.2, giữ lại giá trị nhỏ nhất
        │
        ▼
Bước 5 — Kết quả cuối cùng: M(1, n)
```

> **⚠️ Common Mistake**
> Thứ tự điền bảng cho Interval DP **không thể** đơn giản là "hàng theo hàng" hay "cột theo cột" như bảng LCS ở Chương 18 — vì $M(i,j)$ phụ thuộc vào các khoảng **ngắn hơn nằm bên trong nó** ($M(i,k)$ và $M(k+1,j)$ đều là khoảng con của $(i,j)$). Việc bắt buộc phải điền bảng theo **độ dài khoảng tăng dần** là điểm dễ gây lỗi nhất khi mới học Interval DP.

### 8.2 Bitmask DP cho bài toán "Traveling Salesman rút gọn" (minh họa nguyên lý)

```text
Bước 1 — Trạng thái dp[mask][i]: chi phí tối thiểu để thăm đúng
           tập thành phố "mask", kết thúc tại thành phố i
        │
        ▼
Bước 2 — Base case: dp[{start}][start] = 0
        │
        ▼
Bước 3 — Với mỗi trạng thái (mask, i) đã biết chi phí:
        │
        ▼
Bước 4 —   Với mỗi thành phố j chưa thuộc mask:
        │
        ▼
Bước 5 —     Cập nhật dp[mask | (1<<j)][j] bằng cách xét đi từ
             i đến j — giữ lại chi phí nhỏ nhất trong số các
             cách đến (mask | (1<<j), j)
        │
        ▼
Bước 6 — Kết quả: min trên mọi i của dp[tập đầy đủ][i] + chi phí
           quay lại điểm xuất phát
```

### 8.3 Tree DP (giới thiệu nguyên lý qua bài toán "Tập độc lập lớn nhất trên cây")

Bài toán: cho một cây, chọn tập con các nút sao cho không có hai nút nào được chọn là cha–con của nhau, tối đa hóa số lượng nút được chọn.

```text
Bước 1 — Với mỗi nút v, định nghĩa hai trạng thái:
             include(v): số nút tối đa chọn được trong cây con
                          gốc v, với điều kiện v ĐƯỢC chọn
             exclude(v): số nút tối đa chọn được trong cây con
                          gốc v, với điều kiện v KHÔNG được chọn
        │
        ▼
Bước 2 — Base case: nút lá → include(lá) = 1, exclude(lá) = 0
        │
        ▼
Bước 3 — Với nút v có các con c1, c2, ..., ck (đã tính xong
           include/exclude cho từng con — đệ quy post-order):
        │
        ▼
Bước 4 —   include(v) = 1 + Σ exclude(ci)  (nếu chọn v, mọi con
             đều KHÔNG được chọn)
        │
        ▼
Bước 5 —   exclude(v) = Σ max(include(ci), exclude(ci))  (nếu
             không chọn v, mỗi con tự do chọn phương án tốt hơn)
        │
        ▼
Bước 6 — Kết quả cuối: max(include(gốc), exclude(gốc))
```

---

## 9. Triển khai

```python
def edit_distance(X, Y):
    """Tính khoảng cách Levenshtein giữa X và Y bằng Tabulation."""
    m, n = len(X), len(Y)
    D = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(m + 1):
        D[i][0] = i
    for j in range(n + 1):
        D[0][j] = j

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if X[i - 1] == Y[j - 1]:
                D[i][j] = D[i - 1][j - 1]
            else:
                D[i][j] = 1 + min(
                    D[i - 1][j],       # Xóa
                    D[i][j - 1],       # Chèn
                    D[i - 1][j - 1],   # Thay thế
                )
    return D[m][n]


def matrix_chain_order(p):
    """p: danh sách kích thước, len(p) = n + 1 cho n ma trận.
    Trả về số phép nhân vô hướng tối thiểu — minh họa Interval DP."""
    n = len(p) - 1
    M = [[0] * (n + 1) for _ in range(n + 1)]

    for length in range(2, n + 1):          # Duyệt theo ĐỘ DÀI khoảng tăng dần
        for i in range(1, n - length + 2):
            j = i + length - 1
            M[i][j] = float('inf')
            for k in range(i, j):
                cost = M[i][k] + M[k + 1][j] + p[i - 1] * p[k] * p[j]
                M[i][j] = min(M[i][j], cost)

    return M[1][n]


def tsp_bitmask(dist):
    """Traveling Salesman bằng Bitmask DP.
    dist: ma trận khoảng cách vuông n x n. Trả về chi phí tối thiểu
    của chu trình thăm hết mọi thành phố, bắt đầu và kết thúc tại 0."""
    n = len(dist)
    FULL = (1 << n) - 1
    dp = [[float('inf')] * n for _ in range(1 << n)]
    dp[1][0] = 0                              # Chỉ thăm thành phố 0

    for mask in range(1 << n):
        for i in range(n):
            if dp[mask][i] == float('inf') or not (mask & (1 << i)):
                continue
            for j in range(n):
                if mask & (1 << j):            # j đã thăm rồi
                    continue
                new_mask = mask | (1 << j)
                new_cost = dp[mask][i] + dist[i][j]
                if new_cost < dp[new_mask][j]:
                    dp[new_mask][j] = new_cost

    return min(dp[FULL][i] + dist[i][0] for i in range(n) if dp[FULL][i] != float('inf'))


def max_independent_set_tree(tree, root):
    """tree: dict, key là nút, value là danh sách con trực tiếp.
    Trả về kích thước tối đa của tập độc lập trên cây — minh họa Tree DP."""

    def dfs(node):
        include = 1
        exclude = 0
        for child in tree.get(node, []):
            child_include, child_exclude = dfs(child)
            include += child_exclude
            exclude += max(child_include, child_exclude)
        return include, exclude

    inc, exc = dfs(root)
    return max(inc, exc)
```

Bốn hàm minh họa đúng bốn kỹ thuật ở Mục 6–8: `edit_distance` (interval DP đơn giản, tương tự Chương 18 nhưng công thức phức tạp hơn), `matrix_chain_order` (interval DP đầy đủ với duyệt theo độ dài), `tsp_bitmask` (bitmask DP), `max_independent_set_tree` (tree DP dùng đệ quy post-order).

---

## 10. Trực quan hóa quá trình thực thi

**Kết quả `edit_distance("HORSE", "ROS")` khớp với bảng ở Hình 19.1: kết quả là 3.**

**Vết thực thi rút gọn của `matrix_chain_order([30, 35, 15, 5, 10, 20, 25])`** (6 ma trận):

| Khoảng $(i,j)$ | $M(i,j)$ tối thiểu | Điểm chia $k$ tối ưu |
|---|---:|---:|
| $(1,2)$ | 15.750 | 1 |
| $(2,3)$ | 2.625 | 2 |
| $(3,4)$ | 750 | 3 |
| $(1,3)$ | 7.875 | 1 |
| $(2,4)$ | 4.375 | 3 |
| $(1,6)$ (toàn bộ) | **15.125** | 3 |

Kết quả $M(1,6) = 15.125$ phép nhân vô hướng — đây là ví dụ kinh điển trong CLRS (Further Reading), minh họa rằng thứ tự nhân ma trận (dù kết quả cuối cùng giống nhau về mặt toán học) có thể tạo ra chênh lệch chi phí tính toán rất lớn nếu chọn thứ tự không tối ưu.

**So sánh độ phức tạp không gian trước và sau khi áp dụng Rolling Array cho Edit Distance:**

| Cách cài đặt | Độ phức tạp không gian |
|---|---|
| Bảng đầy đủ (Mục 9) | $O(m \times n)$ |
| Rolling Array (chỉ giữ 2 hàng) | $O(n)$ |

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Các kỹ thuật DP nâng cao trong chương này xuất hiện trong nhiều hệ thống công nghiệp có yêu cầu tối ưu hóa phức tạp hơn các bài toán cơ bản ở Chương 18.

| Bối cảnh công nghiệp | Vai trò của kỹ thuật DP nâng cao |
|---|---|
| Trình kiểm tra chính tả (Spell Checker), công cụ tìm kiếm mờ (fuzzy search) | Edit Distance là nền tảng để tính "khoảng cách" giữa từ gõ sai và từ điển, xếp hạng các gợi ý theo độ tương đồng |
| Thư viện đại số tuyến tính hiệu năng cao (NumPy, thư viện BLAS) | Nguyên lý Matrix Chain Multiplication (chọn thứ tự nhân tối ưu) được áp dụng khi trình biên dịch/thư viện tối ưu hóa biểu thức nhân nhiều ma trận liên tiếp |
| Định tuyến logistics, giao hàng (Vehicle Routing Problem, biến thể của TSP) | Bitmask DP là kỹ thuật chuẩn cho các bài toán định tuyến quy mô nhỏ (dưới ~20 điểm giao hàng), trước khi phải chuyển sang các thuật toán xấp xỉ cho quy mô lớn hơn (Volume 3, Part VII) |
| Trình biên dịch (Compiler) — tối ưu hóa cây cú pháp (AST) | Tree DP được dùng để tối ưu hóa cách sinh mã máy từ cây cú pháp trừu tượng, chọn cách kết hợp các nút con tối ưu về chi phí thực thi |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Bitmask DP minh họa rõ ràng một giới hạn thực tiễn quan trọng: dù về mặt lý thuyết nó giải đúng bài toán TSP, độ phức tạp $O(2^n \times n^2)$ khiến nó chỉ khả thi với $n$ dưới khoảng 20–25 thành phố. Đây chính là một minh chứng cụ thể, bằng con số, cho ranh giới giữa "có thể giải bằng thuật toán chính xác" và "cần chuyển sang thuật toán xấp xỉ hoặc heuristic" — chủ đề trung tâm của Volume 3, Part VII, nơi TSP sẽ được xem xét lại dưới góc độ NP-hard.

**Câu hỏi mở** để suy ngẫm: Tree DP ở Mục 8.3 giả định cây có cấu trúc cố định, đã biết trước khi bắt đầu tính toán. Điều gì sẽ xảy ra nếu cấu trúc cây liên tục thay đổi (ví dụ, thêm/xóa nút) và bạn cần cập nhật lại kết quả DP mà không tính lại từ đầu? Đây là bài toán "Dynamic Trees" hoặc "DP on Trees with Updates" — một hướng nghiên cứu tích cực trong cấu trúc dữ liệu nâng cao, thường được giới thiệu ở các khóa học thuật toán cạnh tranh (competitive programming) trình độ cao.

---

## 13. Ưu điểm

- **Mở rộng phạm vi ứng dụng của DP đáng kể** — từ bảng 2 chiều đơn giản (Chương 18) sang các cấu trúc trạng thái phong phú hơn nhiều (khoảng, tập hợp, cây), cho phép giải quyết một lớp bài toán rộng hơn hẳn.
- **Interval DP tổng quát hóa tốt** cho nhiều bài toán có cấu trúc "chia khoảng tại một điểm" — không chỉ Matrix Chain Multiplication mà còn nhiều bài toán tối ưu hóa chuỗi khác.
- **Bitmask DP giải chính xác các bài toán tổ hợp nhỏ** mà Brute Force (Chương 13) không khả thi ($O(n!)$) nhưng bản thân bài toán chưa đến mức cần thuật toán xấp xỉ.
- **Rolling Array giảm đáng kể chi phí bộ nhớ** mà không ảnh hưởng đến độ phức tạp thời gian — một cải tiến "miễn phí" khi công thức truy hồi cho phép.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Một lỗi phổ biến khi mới học Interval DP là cố điền bảng theo thứ tự hàng/cột như Chương 18, dẫn đến truy cập vào các ô **chưa được tính** (vì $M(i,j)$ phụ thuộc vào khoảng con bên trong nó, không phải khoảng "trước đó" theo chỉ số hàng/cột thông thường).

- **Bitmask DP có giới hạn quy mô nghiêm ngặt** — độ phức tạp không gian $O(2^n)$ khiến kỹ thuật này hoàn toàn không khả thi với $n$ vượt quá khoảng 25, do bộ nhớ và thời gian đều tăng theo hàm mũ.
- **Tree DP đòi hỏi xử lý đúng thứ tự đệ quy (post-order)** — nếu tính sai thứ tự (ví dụ tính nút cha trước khi mọi nút con đã xong), kết quả sẽ sai mà không có dấu hiệu lỗi rõ ràng (không có exception, chỉ là kết quả sai).
- **Rolling Array làm mất khả năng truy vết (traceback)** — nếu chỉ giữ lại hàng hiện tại và hàng trước, ta mất thông tin để tái tạo lại **chuỗi** lời giải cụ thể (ví dụ chuỗi Edit Distance, hoặc thứ tự nhân ma trận cụ thể), chỉ còn giữ được **giá trị tối ưu**. Muốn vừa tối ưu không gian vừa truy vết được cần kỹ thuật phức tạp hơn (nằm ngoài phạm vi chương này).
- **Độ phức tạp $O(n^3)$ của Matrix Chain Multiplication có thể vẫn chậm** với $n$ rất lớn (hàng nghìn ma trận) — dù đã cải thiện đáng kể so với duyệt toàn bộ cách đặt dấu ngoặc có thể ($O(4^n / n^{1.5})$, liên quan đến Số Catalan).

---

## 15. So sánh

**Bảng 19.1 — Bốn kỹ thuật DP nâng cao của chương**

| Kỹ thuật | Biểu diễn trạng thái | Độ phức tạp điển hình | Ví dụ |
|---|---|---|---|
| Interval DP | Khoảng $(i, j)$ | $O(n^2)$ đến $O(n^3)$ | Matrix Chain Multiplication, Edit Distance |
| Bitmask DP | Số nguyên $mask \in [0, 2^n)$ | $O(2^n \times n)$ hoặc $O(2^n \times n^2)$ | Traveling Salesman (quy mô nhỏ) |
| Tree DP | Nút của cây | Thường $O(n)$ hoặc $O(n \log n)$ | Tập độc lập lớn nhất trên cây |
| Rolling Array | (Kỹ thuật cài đặt, không phải bài toán mới) | Không đổi về thời gian; giảm không gian | Áp dụng được cho hầu hết DP có bảng 2D |

**Bảng 19.2 — Đối chiếu độ phức tạp: Chương 18 so với Chương 19**

| Tiêu chí | Chương 18 (nền tảng) | Chương 19 (nâng cao) |
|---|---|---|
| Số chiều trạng thái điển hình | 1–2 (chỉ số đơn giản) | 2 (khoảng), hoặc kết hợp (mask + chỉ số), hoặc theo cấu trúc cây |
| Thứ tự điền bảng | Đơn giản (hàng/cột tăng dần) | Phức tạp hơn (theo độ dài khoảng, theo thứ tự bit, theo post-order cây) |
| Độ phức tạp không gian điển hình | $O(n \times m)$ | Có thể $O(2^n \times n)$ (Bitmask) — cao hơn hẳn |

**Phân tích:** Chương này không giới thiệu một "paradigm mới" tách biệt khỏi Chương 18 — nó là sự **mở rộng tự nhiên** của cùng một tư duy (Optimal Substructure + Overlapping Subproblems), chỉ khác ở việc không gian trạng thái trở nên phong phú và phức tạp hơn. Kỹ năng quan trọng nhất xuyên suốt cả hai chương: luôn bắt đầu bằng câu hỏi "trạng thái của bài toán con là gì?" trước khi nghĩ đến công thức truy hồi.

---

## 16. Tóm tắt

- **Interval DP** dùng trạng thái $(i,j)$ đại diện cho một khoảng con, với công thức truy hồi thường xét mọi điểm chia $k$ ở giữa — minh họa qua Matrix Chain Multiplication ($O(n^3)$) và Edit Distance (biến thể đơn giản hơn của cùng ý tưởng).
- **Bitmask DP** dùng một số nguyên để mã hóa một tập con làm trạng thái, tận dụng trực tiếp lý thuyết tập hợp (Volume 1) — khả thi khi $n$ đủ nhỏ (thường dưới 25).
- **Tree DP** mở rộng DP sang cấu trúc cây, với trạng thái gắn liền từng nút, thường cần hai (hoặc nhiều) biến trạng thái song song (như `include`/`exclude`) và tính theo thứ tự post-order.
- **Rolling Array** là kỹ thuật cài đặt giảm độ phức tạp không gian từ $O(n \times m)$ xuống $O(m)$, đánh đổi bằng việc mất khả năng truy vết lời giải cụ thể.
- Bốn kỹ thuật này không phải các paradigm tách biệt — chúng đều là sự mở rộng của cùng nguyên lý DP đã học ở Chương 18, chỉ khác nhau ở cách biểu diễn trạng thái phù hợp với hình dạng cụ thể của từng bài toán.

Với Chương 19, Part III đã hoàn thành trọn vẹn "bộ ba chuẩn bị bài toán" (Chương 14–16) và "bộ đôi chiến lược ra quyết định" (Chương 17–19: Greedy, Dynamic Programming). Chương 20 (Backtracking) sẽ quay lại với một câu hỏi gần gũi hơn với Brute Force (Chương 13): thay vì luôn xét toàn bộ không gian khả dĩ, làm sao **cắt tỉa (prune)** một cách thông minh ngay trong quá trình duyệt, khi phát hiện một nhánh chắc chắn không dẫn đến lời giải hợp lệ.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Tính `edit_distance("kitten", "sitting")` bằng tay, xây dựng đầy đủ bảng $D$ theo định dạng Hình 19.1.
2. Với chuỗi ma trận kích thước $p = [10, 20, 30]$ (2 ma trận: $10 \times 20$ và $20 \times 30$), tính $M(1,2)$ bằng công thức ở Mục 7.2 — chỉ có một cách duy nhất để nhân hai ma trận này, hãy xác nhận công thức cho ra đúng kết quả đó.
3. Với 3 thành phố và ma trận khoảng cách đối xứng đơn giản, liệt kê toàn bộ các trạng thái $(mask, i)$ có thể có theo định dạng Hình 19.2.

### Mức Trung bình (Intermediate)

4. Cài đặt phiên bản Rolling Array cho `edit_distance` — chỉ giữ lại hai hàng (hàng hiện tại và hàng trước) thay vì toàn bộ bảng $D$. Xác nhận kết quả cuối cùng không đổi so với phiên bản đầy đủ.
5. Với cây có gốc là `A`, con của `A` là `B, C`, con của `B` là `D, E`, hãy mô phỏng từng bước `max_independent_set_tree` — tính `include`/`exclude` cho từng nút theo đúng thứ tự post-order (D, E trước B; B, C trước A).

### Mức Nâng cao (Advanced)

6. Chứng minh (bằng lời) tại sao Interval DP cho Matrix Chain Multiplication **bắt buộc** phải điền bảng theo thứ tự độ dài khoảng tăng dần, bằng cách chỉ ra một ví dụ cụ thể nếu điền theo thứ tự "hàng tăng dần" thông thường (như Chương 18), một ô sẽ được tính trước khi giá trị nó phụ thuộc vào đã sẵn sàng.
7. Mở rộng công thức Edit Distance ở Mục 7.1 để hỗ trợ thêm phép toán thứ tư: **hoán đổi hai ký tự liền kề** (transposition, ví dụ "ab" → "ba" tính là 1 phép biến đổi thay vì 2). Đây chính là **khoảng cách Damerau-Levenshtein**. Thiết lập công thức truy hồi mới (gợi ý: cần thêm một điều kiện kiểm tra $X_i = Y_{j-1}$ và $X_{i-1} = Y_j$).

### Mức Nghiên cứu (Research)

8. Bitmask DP giải TSP với độ phức tạp $O(2^n \times n^2)$ — vượt trội so với Brute Force $O(n!)$ (Chương 13), nhưng vẫn là hàm mũ. Tìm hiểu (không cần chứng minh) tại sao, tính đến hiểu biết hiện tại của Computer Science, **chưa có thuật toán nào giải TSP chính xác trong thời gian đa thức** — liên hệ trực tiếp đến câu hỏi P vs NP đã nêu ở Chương 13, Mục 12. Viết một đoạn ngắn (200–300 từ) giải thích, theo cách hiểu của bạn, tại sao việc cải thiện từ $O(n!)$ xuống $O(2^n \times n^2)$ vẫn được xem là một "chiến thắng" quan trọng trong thực hành, dù cả hai vẫn thuộc nhóm độ phức tạp hàm mũ.

---

## 18. Dự án nhỏ

**Dự án: Công cụ gợi ý sửa lỗi chính tả bằng Edit Distance**

- **Mục tiêu:** xây dựng một chương trình Python nhận vào một từ gõ sai và một danh sách từ điển, gợi ý ra $k$ từ gần đúng nhất theo khoảng cách Edit Distance.
- **Yêu cầu:**
  - Cài đặt `edit_distance` theo đúng đặc tả Mục 9, kèm phiên bản Rolling Array (Bài tập 4) để tối ưu bộ nhớ khi từ điển lớn.
  - Đọc một danh sách từ điển (ví dụ vài nghìn từ tiếng Anh phổ biến).
  - Với một từ gõ sai đầu vào, tính Edit Distance đến từng từ trong từ điển, trả về $k$ từ có khoảng cách nhỏ nhất.
  - Đo thời gian chạy khi từ điển tăng dần từ 100 đến 10.000 từ, nhận xét về khả năng mở rộng (scalability) của cách tiếp cận này.
- **Công nghệ đề xuất:** Python, xử lý file văn bản.
- **Kết quả mong đợi:** chương trình gợi ý đúng các từ hợp lý cho ít nhất 10 trường hợp gõ sai thử nghiệm.
- **Hướng mở rộng:** thử tích hợp khoảng cách Damerau-Levenshtein (Bài tập 7) và so sánh chất lượng gợi ý với phiên bản Edit Distance chuẩn.

---

## 19. Tự đánh giá

- [ ] Tôi có thể thiết lập công thức truy hồi cho Edit Distance và Matrix Chain Multiplication mà không cần nhìn lại Mục 7, và giải thích ý nghĩa của từng thành phần trong công thức.
- [ ] Tôi hiểu rõ tại sao Interval DP phải điền bảng theo thứ tự độ dài khoảng tăng dần, chứ không phải theo hàng/cột như Chương 18.
- [ ] Tôi có thể giải thích nguyên lý Bitmask DP bằng cách liên hệ trực tiếp đến lý thuyết tập hợp (Volume 1) — cụ thể là cách một số nguyên mã hóa một tập con.
- [ ] Tôi có thể tự thực hiện Tree DP cho một cây nhỏ (dưới 10 nút) bằng tay, tính đúng `include`/`exclude` theo thứ tự post-order.
- [ ] Tôi hiểu sự đánh đổi của Rolling Array: tiết kiệm bộ nhớ nhưng mất khả năng truy vết lời giải cụ thể, và biết khi nào đánh đổi này chấp nhận được.

Nếu Bài tập 6 (giải thích thứ tự điền bảng Interval DP) vẫn còn mơ hồ, hãy quay lại thử tự tay điền bảng $M$ cho ví dụ ở Mục 10 theo đúng thứ tự "hàng tăng dần" sai, và quan sát cụ thể ô nào bị truy cập trước khi có giá trị hợp lệ — trải nghiệm trực tiếp lỗi này thường hiệu quả hơn đọc giải thích bằng lời.

---

## 20. Đọc thêm

- **Sách:** Thomas H. Cormen, Charles E. Leiserson, Ronald L. Rivest, Clifford Stein, *Introduction to Algorithms* (CLRS) — Chương về Matrix Chain Multiplication (ví dụ số liệu ở Mục 10 lấy trực tiếp từ đây) và các bài toán DP nâng cao khác. *(Xem BOOKS.md — Volume 3.)*
- **Bài báo gốc:** Vladimir Levenshtein, "Binary codes capable of correcting deletions, insertions, and reversals" (1966) — nguồn gốc của Edit Distance. *(Xem PAPERS.md.)*
- **Sách:** Steven Skiena, *The Algorithm Design Manual* — phần catalog các bài toán Bitmask DP và Tree DP thường gặp trong thực hành.
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc tổng quan không kỹ thuật về "Dynamic Trees" và cấu trúc dữ liệu Link-Cut Tree, chuẩn bị nền tảng cho các khóa học thuật toán nâng cao ngoài phạm vi Handbook này.
- **Chương tiếp theo:** Chương 20 — Backtracking.

---

### Liên kết chương (Cross References)

- **Chương trước:** Chương 18 — Dynamic Programming I (nền tảng bắt buộc: Optimal Substructure, Overlapping Subproblems, Memoization, Tabulation).
- **Chương tiếp theo:** Chương 20 — Backtracking (quay lại tư duy gần với Brute Force, Chương 13, nhưng bổ sung khả năng cắt tỉa (pruning) — một góc nhìn khác so với "ghi nhớ để tránh tính lại" của DP).
- **Chương liên quan xa hơn:** Chương 21 — Branch and Bound (áp dụng Bitmask DP-style state cho bài toán tối ưu hóa như TSP, nhưng với chiến lược cắt tỉa dựa trên cận thay vì ghi nhớ đầy đủ); Volume 3, Part VII — NP-Completeness (TSP, giới hạn của Bitmask DP, Mục 12); Volume 5/6 — Reinforcement Learning, Sequence-to-Sequence Models (Edit Distance liên hệ đến các phép đo tương đồng chuỗi trong NLP hiện đại).
- **Vị trí trong Knowledge Graph:** Nút thứ bảy của Volume 3, Part III, phụ thuộc trực tiếp và hoàn toàn vào Chương 18; khép lại cặp chương Dynamic Programming trước khi chuyển sang Chương 20–21 (Backtracking, Branch and Bound) — hai kỹ thuật xử lý bài toán tổ hợp bằng cách cắt tỉa không gian tìm kiếm thay vì ghi nhớ.

---

*Hết Chương 19. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`, khớp với outline đã thống nhất cho Volume 3, Part III. Bốn kỹ thuật nâng cao (Interval DP, Bitmask DP, Tree DP, Rolling Array) được trình bày với công thức toán học đầy đủ, code minh họa chạy được, và số liệu cụ thể (ví dụ Matrix Chain Multiplication từ CLRS). Chương khép lại cặp Dynamic Programming I–II, hoàn thành nhóm "chiến lược ra quyết định" của Part III cùng với Chương 17. Đang chờ rà soát trước khi tiếp tục sang Chương 20 — Backtracking.*
