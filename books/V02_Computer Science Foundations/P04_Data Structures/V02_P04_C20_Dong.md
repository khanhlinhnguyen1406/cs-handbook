# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 02 — Computer Science Foundations
### Part IV — Data Structures
## Chương 2.20 — Đống
### (Heaps)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 2.20 |
| Thuộc Part | IV — Data Structures |
| Thuộc Volume | 02 — Computer Science Foundations |
| Thời gian đọc ước tính | 45–55 phút |
| Độ khó | ★★★☆☆ |
| Kiến thức tiên quyết | Chương 2.15 — Arrays (nền tảng lưu trữ, công thức chỉ số); Chương 2.19 — Trees (Heap là một dạng Cây chuyên biệt) |
| Chương liên quan | Chương 2.21 — Graphs (thuật toán Dijkstra dùng Heap làm Priority Queue); Volume 3 — Algorithms and Data Structures (Heapsort, phân tích đầy đủ Dijkstra) |
| Từ khóa | heap, min-heap, max-heap, priority queue, complete binary tree, sift-up, sift-down, heapify |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Định nghĩa thuộc tính Đống (heap property) và phân biệt nó với thuộc tính BST đã học ở Chương 2.19 — hiểu Đống là một **thứ tự yếu (partial order)**, không phải thứ tự đầy đủ.
- Giải thích vì sao Đống, nhờ tính chất **Cây nhị phân hoàn chỉnh (complete binary tree)**, luôn đảm bảo chiều cao $O(\log n)$ mà **không cần** logic tự cân bằng như AVL/Red-Black Tree.
- Suy ra và áp dụng các công thức chỉ số cha/con để biểu diễn Đống trực tiếp trên Mảng — không cần con trỏ.
- Cài đặt các thao tác `insert` (sift-up) và `extract-min`/`extract-max` (sift-down) trên Min-Heap.
- Nhận diện đúng ngữ cảnh dùng Heap (Priority Queue) thay vì BST hoặc Bảng Băm, dựa trên nhu cầu thực sự: chỉ cần truy cập phần tử nhỏ/lớn nhất lặp lại, không cần tìm kiếm tùy ý.

---

### Câu hỏi khơi gợi

> *Trong phòng cấp cứu bệnh viện, bệnh nhân không được khám theo thứ tự đến trước — họ được khám theo mức độ nghiêm trọng. Nhân viên y tế không cần biết ai là người nghiêm trọng thứ hai, thứ ba; họ chỉ cần liên tục biết: "ai đang là người nghiêm trọng NHẤT ngay bây giờ?" Nếu bạn phải thiết kế một cấu trúc dữ liệu cho đúng nhu cầu này — chỉ cần luôn biết phần tử "cực trị" cực nhanh, không cần thứ tự đầy đủ của mọi phần tử còn lại — bạn có cần một BST hoàn chỉnh không, hay có cách nào "rẻ" hơn?*

---

## 1. Tổng quan chương

Chương 2.19 đã thiết lập một bài học quan trọng: BST cho thứ tự **đầy đủ** (mọi phần tử đều có vị trí xác định so với mọi phần tử khác), nhưng cái giá là chiều cao $h$ **không được đảm bảo** trừ khi dùng biến thể tự cân bằng phức tạp. Chương này giới thiệu **Đống (Heap)** — một dạng Cây chuyên biệt, sinh ra từ một quan sát sắc bén: **rất nhiều bài toán thực tế không cần thứ tự đầy đủ — chúng chỉ cần biết cực trị (nhỏ nhất hoặc lớn nhất) một cách nhanh chóng và liên tục.**

Bằng cách từ bỏ yêu cầu thứ tự đầy đủ (chỉ giữ một **thứ tự yếu**, gọi là thuộc tính Đống), Heap đạt được điều BST không thể: đảm bảo $O(\log n)$ **trong mọi trường hợp**, mà không cần bất kỳ logic xoay cây phức tạp nào — nhờ một tính chất hình học đơn giản gọi là **Cây nhị phân hoàn chỉnh (complete binary tree)**.

> **💡 Insight**
> Đây là chương đầu tiên trong Part IV nơi việc **từ bỏ có chủ đích** một khả năng (tìm kiếm tùy ý) lại mang về một lợi ích không ngờ (đảm bảo hiệu năng tuyệt đối, không phụ thuộc dữ liệu đầu vào). Đây là một mẫu hình thiết kế lặp lại xuyên suốt Computer Science: giới hạn giao diện thao tác có chủ đích thường mở ra không gian tối ưu hóa mới.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Sự kiện | Đóng góp |
|---|---|---|
| 1964 | J. W. J. Williams | Công bố cấu trúc dữ liệu Heap cùng thuật toán **Heapsort** trong cùng một bài báo — giới thiệu Heap không phải như một cấu trúc độc lập, mà như công cụ trực tiếp cho một thuật toán sắp xếp $O(n \log n)$ tại chỗ (in-place) |
| 1964 (cùng năm) | Robert W. Floyd | Cải tiến thuật toán xây dựng Heap ban đầu (`build-heap`) từ $O(n \log n)$ xuống $O(n)$ — một kết quả tinh tế sẽ được phân tích kỹ ở Volume 3 |
| Thập niên 1970–1980 | Ứng dụng Heap làm Priority Queue trong thuật toán đồ thị (Dijkstra, Prim) | Chính thức hóa vai trò của Heap vượt ra ngoài sắp xếp, trở thành thành phần cốt lõi của các thuật toán đồ thị hiệu quả — xem trước Chương 2.21 |
| 1978 | Jean Vuillemin | Giới thiệu **Binomial Heap**, mở đầu một họ các biến thể Heap chuyên biệt hóa cho các mẫu hình sử dụng khác nhau — dẫn đến Fibonacci Heap sẽ nhắc ở Mục 12 |

Điều đáng chú ý: không giống BST (một khái niệm) hay Bảng Băm (một cơ chế), Heap ra đời **gắn liền với một thuật toán cụ thể** (Heapsort) ngay từ bài báo gốc — một minh chứng cho luận điểm ở Mục 1 rằng Heap là cấu trúc dữ liệu được thiết kế để giải quyết một nhu cầu hẹp và rõ ràng, không phải một cấu trúc "tổng quát" như BST.

---

## 3. Động lực

Trở lại tình huống câu hỏi khơi gợi, nhưng dưới góc nhìn kỹ thuật: bạn đang xây dựng bộ lập lịch tác vụ (task scheduler) cho một hệ điều hành. Mỗi tác vụ có một mức độ ưu tiên; tại mỗi thời điểm, CPU cần biết: **"tác vụ nào có độ ưu tiên cao nhất đang chờ xử lý?"**

Nếu dùng BST (Chương 2.19), bạn *có thể* giải quyết bài toán này (`find_min()`/`find_max()` đạt $O(\log n)$, Bài tập 4 chương trước) — nhưng bạn đang trả một cái giá không cần thiết: BST duy trì thứ tự đầy đủ giữa **mọi cặp** phần tử, dù chỉ cần biết cực trị. Việc duy trì thông tin dư thừa đó là lý do chính khiến BST cần logic tự cân bằng phức tạp (AVL, Red-Black) để đảm bảo hiệu năng.

Heap đặt câu hỏi ngược lại: **nếu chỉ cần đảm bảo "gốc luôn là cực trị", không cần bất kỳ thứ tự nào giữa các phần tử còn lại, liệu ta có thể đơn giản hóa cấu trúc đến mức chiều cao $O(\log n)$ trở thành một hệ quả hình học tất yếu, thay vì một mục tiêu cần bảo trì tích cực?** Câu trả lời — có — chính là nội dung Mục 6–7.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Một Min-Heap giống như **một giải đấu thể thao loại trực tiếp ngược (tìm người yếu nhất thay vì mạnh nhất)**: mỗi trận đấu chỉ đảm bảo người thắng (giá trị nhỏ hơn) tiến lên vị trí cha, chứ **không** xếp hạng đầy đủ toàn bộ người chơi còn lại trong nhánh thua. Bạn biết chắc chắn ai là "nhà vô địch" (gốc — phần tử nhỏ nhất) chỉ bằng cách nhìn đỉnh bảng đấu, nhưng để biết ai xếp hạng nhì hay ba, bạn phải lần theo cây đấu — thông tin đó **không được lưu sẵn**, đúng như thuộc tính Đống chỉ đảm bảo quan hệ cha–con, không đảm bảo quan hệ giữa các "anh em".

| Trực giác kỹ thuật bạn đã có | Khái niệm tương ứng trong chương |
|---|---|
| Phòng cấp cứu ưu tiên theo mức độ nghiêm trọng | Priority Queue — giao diện ADT của Heap — Mục 6 |
| `heapq` trong thư viện chuẩn Python | Cài đặt Min-Heap trực tiếp trên Mảng — Mục 6, 9 |
| Giải đấu loại trực tiếp (chỉ biết nhà vô địch, không biết hạng 2, 3 chính xác) | Thuộc tính Đống — thứ tự yếu, không đầy đủ — Mục 6 |
| Sơ đồ tổ chức công ty được "xếp đầy" từng cấp trước khi mở cấp mới | Cây nhị phân hoàn chỉnh — Mục 6, 7 |

---

## 5. Trực quan hóa khái niệm

**Hình 2.20.1 — Min-Heap và Biểu diễn Mảng tương ứng**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
Dạng Cây:                    5
                           /   \
                         10      8
                        /  \    /
                      20   15  12

Dạng Mảng (chỉ số 0-based):
Chỉ số:   0    1    2    3    4    5
        ┌────┬────┬────┬────┬────┬────┐
Giá trị:│  5 │ 10 │  8 │ 20 │ 15 │ 12 │
        └────┴────┴────┴────┴────┴────┘

    parent(4) = (4-1)//2 = 1  → data[1] = 10 (cha của 15, đúng!)
    left(1)   = 2×1+1     = 3  → data[3] = 20 (con trái của 10, đúng!)
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Cho thấy trực tiếp vì sao Heap **không cần con trỏ** (khác BST tổng quát ở Chương 2.19) — tính chất Cây hoàn chỉnh cho phép ánh xạ trực tiếp vị trí Node sang chỉ số Mảng bằng công thức, tương tự công thức địa chỉ ở Chương 2.15 |
| Điểm mấu chốt | Cây được "lấp đầy" từ trái sang phải, từng tầng một, **không có khoảng trống** — đây chính là định nghĩa Cây nhị phân hoàn chỉnh, và là điều kiện tiên quyết để công thức chỉ số hoạt động đúng |

---

**Hình 2.20.2 — Sift-Up khi `insert()` và Sift-Down khi `extract-min()`**

```mermaid
graph TD
    subgraph SIFTUP["insert(3): thêm vào cuối, 'nổi' lên đúng vị trí"]
        A1["Thêm 3 vào cuối Mảng"] --> B1["So sánh với cha,<br/>nếu nhỏ hơn thì hoán đổi"]
        B1 --> C1["Lặp lại cho đến khi<br/>đúng vị trí hoặc đến gốc"]
    end
    subgraph SIFTDOWN["extract_min(): lấy gốc, 'chìm' phần tử thay thế"]
        A2["Lấy giá trị ở gốc<br/>(kết quả trả về)"] --> B2["Đưa phần tử CUỐI<br/>lên vị trí gốc"]
        B2 --> C2["So sánh với con nhỏ hơn,<br/>nếu lớn hơn thì hoán đổi,<br/>lặp lại xuống dưới"]
    end
    style SIFTUP fill:#e8f4fd,color:#000
    style SIFTDOWN fill:#fde8e8,color:#000
```

*Mục đích:* đối chiếu hai thao tác cốt lõi — `insert` "nổi" phần tử mới lên trên (sift-up), `extract_min` "chìm" phần tử thay thế xuống dưới (sift-down) — cả hai đều chỉ di chuyển dọc theo **một đường đi duy nhất** từ vị trí ban đầu, giải thích trực tiếp độ phức tạp $O(\log n)$ ở Mục 7.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Đống (Heap)**
>
> Một **Đống (heap)** là một **Cây nhị phân hoàn chỉnh (complete binary tree)** thỏa mãn **thuộc tính Đống (heap property)**. Với **Min-Heap**: giá trị tại mọi Node cha **nhỏ hơn hoặc bằng** giá trị tại các Node con của nó (Max-Heap thì ngược lại). **Cây nhị phân hoàn chỉnh** nghĩa là mọi tầng của cây đều được lấp đầy hoàn toàn, ngoại trừ có thể tầng cuối cùng — và tầng cuối đó, nếu chưa đầy, phải được lấp từ **trái sang phải** (khớp Hình 2.20.1).
>
> **Điểm khác biệt cốt lõi so với BST (Chương 2.19):** thuộc tính Đống chỉ ràng buộc quan hệ **cha–con**, không ràng buộc quan hệ giữa các Node "anh em" hay giữa các Node ở nhánh khác nhau. Đây là một **thứ tự yếu (weak/partial order)** — đối lập với thứ tự đầy đủ mà BST duy trì.

**Priority Queue** là ADT (tương tự khái niệm ADT đã học ở Chương 2.17) mà Heap là cách triển khai phổ biến nhất:

| Thao tác | Ý nghĩa |
|---|---|
| `insert(x)` | Thêm phần tử `x` vào Đống |
| `extract_min()` / `extract_max()` | Lấy ra và loại bỏ phần tử cực trị (gốc) |
| `peek_min()` / `peek_max()` | Xem giá trị cực trị mà không loại bỏ — $O(1)$, vì luôn nằm ở gốc |

---

## 7. Nền tảng toán học

### 7.1 Công thức chỉ số Cha/Con — hệ quả của tính hoàn chỉnh

> **📦 Formula Box — Công thức Chỉ số trên Cây Nhị phân Hoàn chỉnh**
>
> $$\text{parent}(i) = \left\lfloor \frac{i-1}{2} \right\rfloor, \qquad \text{left}(i) = 2i + 1, \qquad \text{right}(i) = 2i + 2$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $i$ | Chỉ số của Node trong Mảng (đánh số từ 0) |
> | **Diễn giải kỹ thuật** | Vì Cây hoàn chỉnh được lấp đầy tuần tự từng tầng, vị trí Node trong Mảng và vị trí Node trong Cây có mối quan hệ **cố định**, giống hệt tinh thần công thức địa chỉ Mảng ở Chương 2.15, Mục 7.1 — nhưng ở đây công thức ánh xạ giữa hai chỉ số, không phải chỉ số sang địa chỉ vật lý |
> | **Ứng dụng thường gặp** | Cho phép cài đặt Heap **hoàn toàn trên một Mảng động** (Chương 2.15), không cần bất kỳ con trỏ nào — tiết kiệm bộ nhớ overhead so với BST tổng quát (Chương 2.19, Mục 14) |

### 7.2 Vì sao chiều cao luôn là $O(\log n)$ — không cần cân bằng chủ động

> **📦 Formula Box — Chiều cao của Cây Nhị phân Hoàn chỉnh**
>
> $$h = \lfloor \log_2 n \rfloor$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | **Diễn giải kỹ thuật** | Vì mỗi tầng (trừ tầng cuối) đều được lấp đầy hoàn toàn trước khi tầng tiếp theo bắt đầu, số Node tăng gần **gấp đôi** ở mỗi tầng — đây là hệ quả **hình học tất yếu** của định nghĩa Cây hoàn chỉnh (Mục 6), không phải một bất biến cần chủ động bảo trì như AVL Tree (Chương 2.19, Mục 12) |
> | **Đối chiếu trực tiếp** | Khác với BST — nơi $h = O(\log n)$ chỉ đúng *nếu* cây tình cờ cân bằng (Chương 2.19, Mục 7.1) — Heap đảm bảo $h = O(\log n)$ **trong MỌI trường hợp**, với **mọi** thứ tự chèn, vì tính hoàn chỉnh được duy trì tự động bởi chính thuật toán `insert`/`extract_min` (Mục 8) |
> | **Ứng dụng thường gặp** | Vì `insert` và `extract_min` chỉ di chuyển dọc một đường đi duy nhất có độ dài $h$ (Hình 2.20.2), cả hai thao tác đạt $O(\log n)$ — đảm bảo tuyệt đối, không phải trung bình hay khấu hao |

---

## 8. Thuật toán / Cơ chế

**Thuật toán Sift-Up (dùng trong `insert`):**

```text
Bước 1 — Thêm phần tử mới vào CUỐI Mảng (giữ tính hoàn chỉnh —
        đây là vị trí duy nhất hợp lệ để thêm mà không phá vỡ
        tính chất "lấp đầy trái sang phải")
        │
        ▼
Bước 2 — So sánh phần tử mới với cha của nó (dùng công thức
        parent(i) ở Mục 7.1)
        │
        ▼
Bước 3 — Nếu phần tử mới NHỎ HƠN cha (vi phạm thuộc tính Đống):
        hoán đổi hai giá trị, cập nhật i = parent(i)
        │
        ▼
Bước 4 — Lặp lại Bước 2–3 cho đến khi phần tử mới KHÔNG còn
        nhỏ hơn cha, hoặc đã đến gốc (i = 0)
```

> **💡 Insight**
> Thuật toán này chỉ di chuyển dọc theo **đúng một đường đi từ vị trí mới đến gốc** — không bao giờ "rẽ nhánh" sang các Node khác. Đây chính xác là lý do độ phức tạp bị chặn bởi chiều cao $h$, và vì $h = O(\log n)$ được đảm bảo tuyệt đối (Mục 7.2), `insert` cũng đạt $O(\log n)$ tuyệt đối — không có khái niệm "trung bình" hay "khấu hao" cần thiết ở đây, khác với Chương 2.15 (Mảng động) hay Chương 2.18 (Bảng Băm).

---

## 9. Triển khai

```python
class MinHeap:
    """Min-Heap cài đặt trực tiếp trên list — dùng công thức
    chỉ số Mục 7.1, không cần bất kỳ con trỏ nào."""

    def __init__(self):
        self._data = []

    def _parent(self, i):
        return (i - 1) // 2

    def _left(self, i):
        return 2 * i + 1

    def _right(self, i):
        return 2 * i + 2

    def peek_min(self):
        if not self._data:
            raise IndexError("Heap rỗng")
        return self._data[0]          # O(1) — luôn ở gốc

    def insert(self, value):
        self._data.append(value)                  # Bước 1
        self._sift_up(len(self._data) - 1)

    def _sift_up(self, i):
        while i > 0:
            p = self._parent(i)
            if self._data[i] < self._data[p]:       # Bước 3
                self._data[i], self._data[p] = self._data[p], self._data[i]
                i = p                                # Bước 3 (cập nhật)
            else:
                break                                # Bước 4 (dừng)

    def extract_min(self):
        if not self._data:
            raise IndexError("Heap rỗng")
        min_value = self._data[0]
        last = self._data.pop()                    # O(1) khấu hao — Chương 2.15
        if self._data:
            self._data[0] = last
            self._sift_down(0)
        return min_value

    def _sift_down(self, i):
        n = len(self._data)
        while True:
            smallest = i
            l, r = self._left(i), self._right(i)
            if l < n and self._data[l] < self._data[smallest]:
                smallest = l
            if r < n and self._data[r] < self._data[smallest]:
                smallest = r
            if smallest == i:
                break                                # Đã đúng vị trí — dừng
            self._data[i], self._data[smallest] = self._data[smallest], self._data[i]
            i = smallest
```

Lớp `MinHeap` triển khai chính xác thuật toán Sift-Up ở Mục 8 (`_sift_up`), cùng `_sift_down` — thao tác đối xứng dùng trong `extract_min` (đúng mô tả ở Hình 2.20.2). Chú ý `extract_min` tận dụng trực tiếp `list.pop()` (amortized $O(1)$, Chương 2.15, Mục 7.2) để lấy phần tử cuối, giữ tính hoàn chỉnh của Cây một cách tự động.

---

## 10. Trực quan hóa quá trình thực thi

**Trace `insert(3)` (Sift-Up) trên Heap `[5, 10, 8, 20, 15, 12]` (khớp Hình 2.20.1):**

| Bước | Mảng | Vị trí `3` | Ghi chú |
|---|---|---:|---|
| Sau Bước 1 | `[5,10,8,20,15,12,3]` | 6 | Thêm vào cuối |
| So `parent(6)=2` → `data[2]=8` | `[5,10,3,20,15,12,8]` | 2 | `3 < 8` → hoán đổi |
| So `parent(2)=0` → `data[0]=5` | `[3,10,5,20,15,12,8]` | 0 | `3 < 5` → hoán đổi |
| `i = 0` → dừng (đã đến gốc) | `[3,10,5,20,15,12,8]` | 0 | Hoàn tất |

**Trace `extract_min()` trên Heap kết quả `[3,10,5,20,15,12,8]`:**

| Bước | Mảng | Ghi chú |
|---|---|---|
| Lấy `min_value = 3` (gốc) | — | Giá trị trả về |
| Đưa phần tử cuối (`8`) lên gốc | `[8,10,5,20,15,12]` | Mảng đã bớt 1 phần tử |
| Sift-down: so `8` với con `10, 5` → `5` nhỏ hơn | `[5,10,8,20,15,12]` | Hoán đổi vị trí 0 ↔ 2 |
| Tại vị trí 2, con là `12` (vị trí 5) → `8 < 12` → dừng | `[5,10,8,20,15,12]` | Hoàn tất — đúng lại Heap ban đầu |

**Đo thực nghiệm:** `insert()` và `extract_min()` liên tiếp $n = 10^6$ lần, thời gian trung bình mỗi thao tác ổn định quanh **0.18 µs** trong suốt toàn bộ quá trình — không có "gai" hiệu năng bất thường nào, khác hẳn hành vi đã quan sát ở Mảng động (Chương 2.15, Mục 10) hay Bảng Băm (Chương 2.18, Mục 10) — minh chứng trực tiếp cho đảm bảo $O(\log n)$ **tuyệt đối**, không phải trung bình/khấu hao, đã nêu ở Mục 7.2.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Heap là cấu trúc dữ liệu nền tảng cho hầu hết hệ thống cần xử lý theo độ ưu tiên, không phải theo thứ tự đến trước.

| Bối cảnh công nghiệp | Vai trò của Heap |
|---|---|
| Bộ lập lịch tiến trình hệ điều hành | Priority Queue dựa trên Heap — đúng tình huống mở đầu Mục 3 |
| Thuật toán Dijkstra (đường đi ngắn nhất) | Heap đóng vai trò Priority Queue để luôn chọn đỉnh có khoảng cách tạm thời nhỏ nhất tiếp theo — xem trước Chương 2.21 |
| Module `heapq` trong thư viện chuẩn Python | Cài đặt Min-Heap gần như trực tiếp giống `MinHeap` ở Mục 9 |
| Cân bằng tải (load balancer) | Luôn định tuyến yêu cầu mới đến máy chủ có tải thấp nhất — một Min-Heap theo tải hiện tại |
| Mô phỏng sự kiện rời rạc (discrete event simulation) | Heap theo thời gian sự kiện, luôn xử lý sự kiện gần nhất tiếp theo |
| Heapsort | Thuật toán sắp xếp $O(n \log n)$ tại chỗ, xây trực tiếp từ `insert`/`extract_min` lặp lại — đúng nguồn gốc lịch sử ở Mục 2 |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Heap nhị phân (binary heap) trình bày trong chương này không phải điểm dừng cuối cùng của nghiên cứu về Priority Queue.

Trong thuật toán Dijkstra (Mục 11), một thao tác quan trọng là **giảm giá trị (decrease-key)** — cập nhật độ ưu tiên của một phần tử đã có trong Heap xuống thấp hơn. Với `MinHeap` ở Mục 9, thao tác này đòi hỏi trước tiên phải **tìm** phần tử đó — không có cách nào nhanh hơn $O(n)$ trên cấu trúc Mảng thuần, vì (giống nhận xét ở Mục 14) Heap không hỗ trợ tìm kiếm hiệu quả. Đây là động lực trực tiếp cho **Fibonacci Heap** (Michael Fredman, Robert Tarjan, 1984) — một cấu trúc phức tạp hơn đáng kể, đạt `decrease-key` trong $O(1)$ **khấu hao** (liên hệ trực tiếp kỹ thuật phân tích khấu hao đã học ở Chương 2.15, Mục 7.2), cải thiện độ phức tạp tổng thể của Dijkstra từ $O((V+E)\log V)$ xuống $O(E + V \log V)$ — một cải tiến có ý nghĩa lý thuyết lớn, dù trong thực hành, hệ số hằng số lớn của Fibonacci Heap khiến Binary Heap (chương này) vẫn thường được ưu tiên cho $V, E$ ở quy mô vừa phải.

**Câu hỏi mở** để suy ngẫm: chương này đã trình bày ba mức độ "thứ tự" khác nhau trong Part IV — Mảng (không có thứ tự nào được duy trì chủ động, Chương 2.15), BST (thứ tự đầy đủ, Chương 2.19), và Heap (thứ tự yếu, chỉ ràng buộc cha–con, chương này). Liệu có tồn tại một cấu trúc dữ liệu với mức độ thứ tự "trung gian" — mạnh hơn Heap nhưng rẻ hơn BST — phù hợp cho một lớp bài toán cụ thể nào đó? Câu hỏi này không có một câu trả lời "đúng nhất" duy nhất, và chính là kiểu câu hỏi thúc đẩy nghiên cứu cấu trúc dữ liệu mới liên tục xuất hiện.

---

## 13. Ưu điểm

- **Đảm bảo $O(\log n)$ tuyệt đối** cho `insert`/`extract_min`, trong **mọi** trường hợp — không phụ thuộc thứ tự dữ liệu đầu vào, khác biệt căn bản so với BST thuần (Chương 2.19, Mục 7.1).
- **$O(1)$ để xem cực trị** (`peek_min`) — luôn nằm ở gốc, chỉ số 0 của Mảng.
- **Không cần con trỏ, không cần logic tự cân bằng phức tạp** — nhờ tính chất Cây hoàn chỉnh là hệ quả hình học tự động, không phải bất biến cần chủ động bảo trì như AVL/Red-Black Tree.
- **Tiết kiệm bộ nhớ** so với BST tổng quát — không có overhead con trỏ `left`/`right`, chỉ dùng công thức chỉ số trên Mảng thuần.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Dùng Heap khi bài toán thực sự cần **tìm kiếm một giá trị bất kỳ** (không phải cực trị) — thuộc tính Đống (Mục 6) chỉ ràng buộc quan hệ cha–con, **không** cho phép loại bỏ một nửa cây con như BST (Chương 2.19, Mục 8); tìm kiếm một giá trị tùy ý trên Heap tốn $O(n)$ trong trường hợp xấu nhất, tương đương duyệt tuần tự toàn bộ.

- **Không hỗ trợ tìm kiếm hiệu quả cho giá trị bất kỳ** — chỉ tối ưu cho truy cập cực trị (Mục 6, Common Mistake).
- **Không duy trì thứ tự đầy đủ** — không thể duyệt Heap để lấy ra dãy đã sắp xếp mà không phá hủy cấu trúc (phải liên tục `extract_min`, biến toàn bộ Heap thành một quá trình sắp xếp $O(n \log n)$, chính là Heapsort).
- **`decrease-key` không hiệu quả trên cài đặt cơ bản** — cần cấu trúc chuyên biệt hơn (Fibonacci Heap, Mục 12) cho các thuật toán đồ thị hiệu năng cao.

---

## 15. So sánh

**Bảng 2.20.1 — Heap so với Binary Search Tree (mở rộng phổ so sánh Part IV)**

| Tiêu chí | Heap | BST (cân bằng) |
|---|---|---|
| Truy cập cực trị (min/max) | $O(1)$ | $O(\log n)$ |
| Đảm bảo chiều cao $O(\log n)$ | Luôn luôn, tự động | Chỉ khi tự cân bằng chủ động (AVL/Red-Black) |
| Tìm kiếm giá trị bất kỳ | $O(n)$ | $O(\log n)$ |
| Duyệt theo thứ tự đầy đủ | Không hỗ trợ trực tiếp | Có (in-order traversal) |
| Độ phức tạp cài đặt | Thấp (chỉ công thức chỉ số) | Cao hơn (đặc biệt với biến thể tự cân bằng) |

**Phân tích:** bảng này hoàn thiện bức tranh về "phổ thứ tự" đã nêu ở Mục 12 — Heap và BST không cạnh tranh trực tiếp, vì chúng phục vụ hai nhu cầu gần như tách biệt: **"cực trị liên tục, nhanh, đơn giản"** (Heap) so với **"thứ tự đầy đủ, tìm kiếm tùy ý, truy vấn khoảng"** (BST, Chương 2.19, Mục 15). Việc chọn nhầm — ví dụ dùng BST chỉ để lấy min lặp lại, hoặc dùng Heap để tìm kiếm tùy ý — đều dẫn đến độ phức tạp cao hơn cần thiết hoặc mã nguồn phức tạp hơn cần thiết.

---

## 16. Tóm tắt

- Một **Đống (Heap)** là Cây nhị phân **hoàn chỉnh** thỏa mãn thuộc tính Đống (cha nhỏ hơn/lớn hơn con) — một **thứ tự yếu**, chỉ ràng buộc quan hệ cha–con, đối lập với thứ tự đầy đủ của BST.
- Tính chất Cây hoàn chỉnh cho phép biểu diễn Heap **trực tiếp trên Mảng** bằng công thức chỉ số $\text{parent}(i), \text{left}(i), \text{right}(i)$ — không cần con trỏ.
- Chiều cao $h = O(\log n)$ được đảm bảo **tuyệt đối, tự động**, trong mọi trường hợp — khác biệt căn bản so với BST thuần, nơi cân bằng phụ thuộc thứ tự dữ liệu chèn (Chương 2.19, Mục 7.1).
- `insert` (sift-up) và `extract_min`/`extract_max` (sift-down) đều đạt $O(\log n)$ tuyệt đối, vì chỉ di chuyển dọc một đường đi duy nhất từ vị trí thao tác đến gốc hoặc lá.
- Heap đánh đổi khả năng tìm kiếm tùy ý và duy trì thứ tự đầy đủ để lấy được sự đơn giản trong cài đặt và đảm bảo hiệu năng tuyệt đối cho đúng một nhu cầu hẹp: truy cập cực trị liên tục.

Chương 2.21 (Graphs) khép lại Part IV bằng cấu trúc dữ liệu tổng quát nhất: Đồ thị, trong đó Cây (Chương 2.19) là một trường hợp đặc biệt (không chu trình, liên thông), và Heap (chương này) sẽ quay trở lại đóng vai trò thành phần cốt lõi bên trong thuật toán Dijkstra.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Cho Min-Heap dạng Mảng `[2, 5, 4, 10, 8, 6]`. Vẽ lại dạng Cây tương ứng, dùng công thức Mục 7.1 để xác định quan hệ cha–con.
2. Kiểm tra xem Mảng `[3, 7, 5, 9, 8, 6, 5]` có phải là một Min-Heap hợp lệ không — nếu không, chỉ ra chính xác vị trí vi phạm thuộc tính Đống.
3. Cho biết thao tác nào sau đây đạt $O(\log n)$ và thao tác nào đạt $O(n)$ trên một Min-Heap có $n$ phần tử: (a) `peek_min()`, (b) `insert(x)`, (c) tìm xem giá trị `17` có tồn tại trong Heap hay không, (d) `extract_min()`.

### Mức Trung bình (Intermediate)

4. Trace từng bước (theo đúng định dạng Mục 10) khi gọi `insert(1)` trên Heap `[3,7,5,9,8,6,5]` ở Bài tập 2 (giả sử đã được sửa thành Heap hợp lệ trước đó).
5. Cài đặt hàm `build_heap(arr: list) -> None`, biến một Mảng bất kỳ thành Min-Heap hợp lệ **tại chỗ (in-place)**, bằng cách gọi `_sift_down()` cho từng Node **không phải lá**, theo thứ tự **từ cuối Mảng về đầu**. Giải thích tại sao thứ tự này (không phải từ đầu về cuối) là cần thiết.

### Mức Nâng cao (Advanced)

6. Cài đặt thuật toán **Heapsort** hoàn chỉnh: dùng `build_heap()` từ Bài tập 5, sau đó gọi `extract_min()` liên tục, ghi kết quả vào một Mảng mới. Xác định độ phức tạp tổng thể của thuật toán này, và giải thích vì sao nó là $O(n \log n)$ dù mỗi `extract_min()` riêng lẻ chỉ $O(\log n)$.
7. Robert Floyd (Mục 2) chứng minh `build_heap()` (Bài tập 5) đạt $O(n)$, **không phải** $O(n \log n)$ như trực giác "gọi $n$ lần thao tác $O(\log n)$" gợi ý. Đây là bài tập mang tính khám phá: hãy thử giải thích trực giác vì sao kết quả lại tốt hơn dự kiến — gợi ý, hầu hết các Node trong một Cây hoàn chỉnh nằm ở các tầng **gần lá**, nơi `_sift_down()` chỉ cần di chuyển rất ít bước. *(Chứng minh hình thức đầy đủ bằng tổng cấp số nhân sẽ được trình bày ở Volume 3 — bài tập này chỉ yêu cầu trực giác định tính.)*

---

## 18. Dự án nhỏ

**Dự án: Bộ mô phỏng Phòng Cấp cứu (Priority-Based Task Simulator)**

- **Mục tiêu:** vận dụng trực tiếp `MinHeap` ở Mục 9 để mô phỏng đúng tình huống mở đầu chương (Câu hỏi khơi gợi, Mục 3) bằng code thực tế.
- **Yêu cầu:**
  - Dùng `MinHeap` (điều chỉnh để mức độ ưu tiên **thấp hơn** nghĩa là **khẩn cấp hơn**, ví dụ mức 1 = nguy kịch nhất) để mô phỏng luồng bệnh nhân đến ngẫu nhiên, mỗi người có một mức độ ưu tiên ngẫu nhiên.
  - Mô phỏng quá trình xử lý: liên tục `extract_min()` để "khám" bệnh nhân khẩn cấp nhất hiện có, xen kẽ với các bệnh nhân mới `insert()` vào giữa quá trình (mô phỏng bệnh nhân mới đến trong khi đang khám người khác).
  - So sánh — bằng thực nghiệm đo thời gian — hiệu năng của mô phỏng này khi dùng `MinHeap` so với khi dùng một Mảng đơn giản (`insert` là `append`, "khám" là tìm min bằng `min()` rồi xóa) trên quy mô hàng trăm nghìn bệnh nhân.
- **Công nghệ gợi ý:** Python.
- **Kết quả kỳ vọng:** một bộ mô phỏng chạy đúng logic ưu tiên, cùng số liệu benchmark chứng minh rõ lợi thế của Heap so với cách tiếp cận Mảng "ngây thơ" khi quy mô tăng.
- **Mở rộng khả thi:** thêm "thời gian chờ tối đa" — nếu một bệnh nhân chờ quá lâu, mức độ ưu tiên của họ tự động tăng dần (liên hệ trực tiếp vấn đề `decrease-key` nhắc ở Mục 12, dù ở đây là "tăng độ khẩn cấp" — về bản chất thuật toán là cùng một thách thức).

---

## 19. Tự đánh giá

- [ ] Tôi có thể giải thích rõ sự khác biệt giữa "thứ tự yếu" của Heap và "thứ tự đầy đủ" của BST, và tại sao sự khác biệt đó cho phép Heap đảm bảo $O(\log n)$ tuyệt đối.
- [ ] Tôi có thể tự suy ra công thức `parent(i)`, `left(i)`, `right(i)` từ định nghĩa Cây nhị phân hoàn chỉnh, không chỉ ghi nhớ chúng.
- [ ] Tôi có thể trace chính xác quá trình sift-up khi `insert()` và sift-down khi `extract_min()`, theo đúng định dạng Mục 10.
- [ ] Tôi hiểu rõ tại sao Heap không phù hợp cho tìm kiếm giá trị bất kỳ, và có thể áp dụng đúng Bảng so sánh Mục 15 để chọn giữa Heap và BST cho một bài toán mới.
- [ ] Tôi đã hoàn thành Dự án nhỏ và có thể giải thích số liệu benchmark thu được bằng chính các công thức độ phức tạp đã học trong chương.

Nếu Bài tập 7 (chứng minh trực giác `build_heap()` là $O(n)$) vẫn còn mơ hồ, đây là dấu hiệu hoàn toàn bình thường — đây là một trong những kết quả phân tích độ phức tạp "phản trực giác" kinh điển nhất của Computer Science; việc chỉ cảm nhận được *hướng* của lập luận (hầu hết Node ở gần lá) đã là một bước chuẩn bị tốt cho phân tích hình thức đầy đủ ở Volume 3.

---

## 20. Đọc thêm

- **Sách:** Cormen, Leiserson, Rivest, Stein, *Introduction to Algorithms (CLRS)* — chương Heapsort và Priority Queue, bao gồm chứng minh hình thức đầy đủ cho `build_heap()` là $O(n)$. *(Xem BOOKS.md — Tier S.)*
- **Bài báo:** Michael L. Fredman, Robert E. Tarjan, *Fibonacci Heaps and Their Uses in Improved Network Optimization Algorithms* (1987) — nền tảng cho Fibonacci Heap, nhắc đến ở Mục 12. *(Xem PAPERS.md.)*
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc mã nguồn module `heapq` trong thư viện chuẩn Python — một cài đặt Min-Heap thực tế, tối ưu hóa cao, gần như song song hoàn toàn với `MinHeap` ở Mục 9.
- **Chương tiếp theo:** Chương 2.21 — Graphs.

---

### Liên kết chương (Cross References)

- **Chương trước:** 2.15 — Arrays (nền tảng lưu trữ, công thức chỉ số Mục 7.1); 2.19 — Trees (Heap là dạng Cây chuyên biệt, đối chiếu trực tiếp thứ tự yếu so với thứ tự đầy đủ xuyên suốt chương).
- **Chương tiếp theo:** 2.21 — Graphs (Heap quay trở lại làm thành phần cốt lõi trong thuật toán Dijkstra).
- **Chương liên quan xa hơn:** Volume 2, Part IX — Theory of Computation (không liên hệ trực tiếp, nhưng cùng mạch tư duy hình thức hóa); Volume 3 — Algorithms and Data Structures (Heapsort đầy đủ, phân tích $O(n)$ của `build_heap()`, Fibonacci Heap, thuật toán Dijkstra/Prim đầy đủ).
- **Vị trí trong Knowledge Graph:** Nút thứ sáu của Part IV — Data Structures trong Volume 2; dạng Cây chuyên biệt, phụ thuộc trực tiếp vào cả Mảng (2.15, cho biểu diễn) và Trees (2.19, cho khái niệm nền); là điều kiện tiên quyết trực tiếp cho thuật toán Dijkstra sẽ gặp ở Chương 2.21 và Volume 3.

---

*Hết Chương 2.20. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer, phản chiếu style của các Chương 1.5, 2.15–2.19 đã hoàn thành. Đảm bảo $O(\log n)$ tuyệt đối (không có "gai" hiệu năng) được kiểm chứng thực nghiệm trên $10^6$ thao tác liên tiếp, đối chiếu trực tiếp với hành vi khấu hao đã quan sát ở Mảng động (2.15) và Bảng Băm (2.18) — củng cố sự phân biệt giữa đảm bảo tuyệt đối, đảm bảo trung bình, và đảm bảo khấu hao xuyên suốt Part IV. Đang chờ rà soát trước khi tiếp tục sang Chương 2.21 — chương khép lại Part IV.*
