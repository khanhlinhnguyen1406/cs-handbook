# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 02 — Computer Science Foundations
### Part V — Computer Organization & Architecture
## Chương 2.27 — Cache Memory

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 2.27 |
| Thuộc Part | V — Computer Organization & Architecture |
| Thuộc Volume | 02 — Computer Science Foundations |
| Thời gian đọc ước tính | 55–65 phút |
| Độ khó | ★★★★☆ |
| Kiến thức tiên quyết | Chương 2.26 — Memory Hierarchy (đặc biệt Mục 6–8, Formula Box $T_{\text{avg}}$); Volume 2, Part II — Data Representation (Binary Number System) |
| Chương liên quan | 2.28 — Input/Output Systems (chương cuối Part V) |
| Từ khóa | Cache Line, Cache Block, Tag, Index, Offset, Direct-Mapped, Fully Associative, Set-Associative, LRU, AMAT, Miss Penalty |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Giải thích cách một địa chỉ bộ nhớ được **phân tách** thành ba trường Tag, Index, Offset để tra cứu Cache.
- Mô tả và so sánh ba kiểu **Cache Mapping**: Direct-Mapped, Fully Associative, Set-Associative.
- Áp dụng công thức **AMAT (Average Memory Access Time)** — mở rộng trực tiếp từ Chương 2.26 — để đánh giá định lượng hiệu năng một thiết kế Cache.
- Định nghĩa chính sách thay thế **LRU (Least Recently Used)** và giải thích vì sao nó khai thác trực tiếp Temporal Locality.
- Phân biệt ba nguyên nhân gây **Cache Miss**: Compulsory, Capacity, Conflict.

---

### Câu hỏi khơi gợi

> *Chương 2.26 nói rằng CPU "kiểm tra tầng gần trước". Nhưng "kiểm tra" nghĩa là gì, về mặt kỹ thuật? Cache không thể quét qua toàn bộ nội dung của nó mỗi lần CPU cần một byte — việc đó sẽ chậm hơn cả việc xuống thẳng Main Memory. Vậy làm sao Cache biết ngay lập tức, chỉ trong một hoặc vài chu kỳ, rằng dữ liệu cần tìm có đang ở đó hay không?*

---

## 1. Tổng quan chương

Chương 2.26 trình bày Memory Hierarchy ở mức nguyên lý chung, áp dụng cho mọi cặp tầng. Chương này **chuyên biệt hóa** nguyên lý đó cho cặp tầng quan trọng nhất đối với hiệu năng CPU hằng ngày: **Cache và Main Memory**. Đây cũng là chương đòi hỏi tư duy kỹ thuật sâu nhất trong Part V, vì câu trả lời cho câu hỏi khơi gợi ở trên — làm sao tra cứu Cache trong thời gian gần như tức thì — chính là nội dung cốt lõi của toàn chương.

> **💡 Insight**
> Cơ chế ở chương này thực chất là một **cấu trúc dữ liệu dạng Hash Table hiện thực hoàn toàn bằng phần cứng** — nếu bạn đã quen thuộc với Hash Table (sẽ học chính thức ở Volume 3), phần lớn trực giác ở đây sẽ rất tự nhiên: Index đóng vai trò gần giống "hash bucket", còn Tag đóng vai trò xác nhận "đúng object cần tìm, không phải một object khác rơi vào cùng bucket".

---

## 2. Bối cảnh lịch sử

| Thời điểm | Sự kiện | Ý nghĩa |
|---|---|---|
| 1965 | Maurice Wilkes đề xuất khái niệm Cache (đã nêu ở Chương 2.26, Mục 2) | Ý tưởng ban đầu: một tầng nhớ nhỏ, nhanh, "trong suốt" với phần mềm |
| Cuối 1980s | Cache trở thành thành phần tiêu chuẩn trong hầu hết CPU thương mại | Đáp ứng trực tiếp hiện tượng Memory Wall (Chương 2.26, Mục 12) |
| Từ 1990s → nay | Kiến trúc Cache đa tầng: **L1, L2, L3** | Áp dụng chính triết lý Memory Hierarchy (Chương 2.26) một lần nữa, ngay *bên trong* tầng Cache: L1 nhỏ nhất/nhanh nhất, L3 lớn nhất/chậm nhất trong số các tầng Cache — một ví dụ đẹp cho thấy "phân tầng" là nguyên lý đệ quy, không chỉ áp dụng một lần |

---

## 3. Động lực

Ở Chương 2.26 (Mục 10), Hit Rate được xác định bằng cách mô phỏng và đếm trực tiếp — một cách tiếp cận phù hợp để minh họa khái niệm, nhưng **không phải cách Cache thật hoạt động**. Cache thật không thể "quét toàn bộ nội dung đang lưu" mỗi lần CPU cần một byte — với hàng nghìn hoặc hàng triệu mục có thể lưu trong Cache, việc quét tuần tự sẽ chậm hơn cả việc xuống thẳng Main Memory, phá vỡ hoàn toàn mục đích tồn tại của Cache.

Vấn đề kỹ thuật cốt lõi, do đó, là: **thiết kế một cơ chế tra cứu gần như tức thì (O(1), sẽ học khái niệm Big-O đầy đủ ở Volume 3)**, đủ nhanh để không làm mất đi lợi ích tốc độ mà Cache hứa hẹn. Đây chính xác là bài toán mà Cache Mapping (Mục 6) giải quyết.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Hãy tưởng tượng một tủ hồ sơ với đúng **100 ngăn kéo được đánh số 0–99**. Thay vì tìm kiếm tuần tự qua từng ngăn, bạn có một quy tắc cố định: *"Hồ sơ có mã số kết thúc bằng hai chữ số nào, đặt vào đúng ngăn kéo mang số đó."* Muốn tìm hồ sơ mã `4527`, bạn đi thẳng đến ngăn kéo số `27` — không cần tìm kiếm. Nhưng khi mở ngăn kéo ra, bạn vẫn cần **kiểm tra nhãn** trên hồ sơ để chắc chắn đó đúng là hồ sơ `4527`, chứ không phải một hồ sơ khác cũng có đuôi `27` (ví dụ `1927`) đã được đặt vào cùng ngăn trước đó.

| Trực giác trong analogy | Khái niệm Cache tương ứng |
|---|---|
| Số ngăn kéo (0–99) | **Index** — xác định *vị trí* cần kiểm tra |
| Nhãn dán trên hồ sơ để xác nhận đúng hồ sơ | **Tag** — xác nhận *đúng dữ liệu* đang tìm |
| Toàn bộ nội dung hồ sơ bên trong | **Cache Block / Cache Line** — khối dữ liệu thực sự |
| Đi thẳng đến đúng ngăn kéo, không tìm kiếm tuần tự | Tra cứu Cache trong O(1) — giải quyết đúng vấn đề nêu ở Mục 3 |

---

## 5. Trực quan hóa khái niệm

**Hình 2.27.1 — Phân tách một địa chỉ bộ nhớ thành Tag / Index / Offset**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
   Địa chỉ bộ nhớ (32 bit, ví dụ):

   ┌─────────────────────┬───────────────┬─────────────┐
   │         Tag          │     Index      │   Offset    │
   │   (xác nhận đúng      │  (chọn ngăn    │ (vị trí byte│
   │    dữ liệu, Mục 4)     │  kéo — Mục 4)  │  trong khối)│
   └─────────────────────┴───────────────┴─────────────┘
         20 bit                8 bit            4 bit
                                  ↓
                          Chọn 1 trong 2^8 = 256
                          "ngăn kéo" (Cache Set)
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Cho thấy trực quan bước đầu tiên và quan trọng nhất của mọi thao tác Cache: một địa chỉ 32-bit *tự nó chứa sẵn* thông tin cần thiết để định vị ngay lập tức "ngăn kéo" nào cần kiểm tra |
| Điểm mấu chốt | **Offset** (4 bit ở ví dụ này) không dùng để tìm khối, mà để xác định **byte cụ thể nào bên trong khối** — vì Cache luôn nạp/lưu theo từng **khối (block)** liền kề, khai thác Spatial Locality đã học ở Chương 2.26, Mục 8, chứ không bao giờ theo từng byte riêng lẻ |

---

**Hình 2.27.2 — Ba kiểu Cache Mapping**

```text
Direct-Mapped:        mỗi khối bộ nhớ CHỈ có đúng 1 "ngăn kéo"
                       duy nhất nó có thể vào

Fully Associative:     mỗi khối bộ nhớ có thể vào BẤT KỲ ngăn
                       kéo nào đang trống — không có Index,
                       phải kiểm tra Tag của MỌI ngăn kéo

Set-Associative (N-way): kết hợp cả hai — Index chọn ra một
                       NHÓM gồm N ngăn kéo, rồi kiểm tra Tag
                       trong N ngăn đó
```

*Mục đích:* đặt ba lựa chọn thiết kế cạnh nhau để so sánh trực tiếp (phân tích đầy đủ ở Mục 15). *Điểm mấu chốt:* đây là một trade-off giữa **tốc độ tra cứu** (Direct-Mapped nhanh nhất, vì chỉ kiểm tra đúng 1 vị trí) và **tính linh hoạt** (Fully Associative linh hoạt nhất, giảm thiểu Conflict Miss — Mục 14, nhưng chậm hơn và tốn phần cứng hơn để kiểm tra song song nhiều Tag).

---

## 6. Định nghĩa hình thức

> **📌 Remember — Cache Line (Cache Block)**
>
> Đơn vị dữ liệu nhỏ nhất mà Cache nạp vào hoặc loại bỏ tại một thời điểm — không bao giờ là một byte đơn lẻ. Kích thước phổ biến trong thực tế là 64 byte. Khi xảy ra Miss tại địa chỉ X, Cache nạp **toàn bộ khối chứa X**, chủ động khai thác Spatial Locality (Chương 2.26, Hình 2.26.2).

> **📌 Remember — Ba trường của địa chỉ khi tra cứu Cache**
>
> | Trường | Vai trò |
> |---|---|
> | **Offset** | Xác định byte cụ thể *bên trong* Cache Line |
> | **Index** | Xác định *Cache Set* (nhóm ngăn kéo) cần kiểm tra — tính trực tiếp bằng phép chia lấy dư (Mục 8) |
> | **Tag** | Phần còn lại của địa chỉ, dùng để **xác nhận** dữ liệu tại vị trí đó đúng là dữ liệu đang cần tìm, không phải một khối khác từng dùng chung vị trí đó trước đây |

**Ba kiểu Cache Mapping** (đã minh họa ở Hình 2.27.2):

| Kiểu | Số vị trí có thể chứa một khối | Chi phí phần cứng khi tra cứu |
|---|---|---|
| **Direct-Mapped** | Đúng 1 vị trí duy nhất | Thấp nhất — chỉ so sánh 1 Tag |
| **Fully Associative** | Bất kỳ vị trí trống nào | Cao nhất — phải so sánh Tag của mọi vị trí song song |
| **Set-Associative (N-way)** | N vị trí trong cùng một Set | Trung bình — so sánh N Tag song song |

> **📌 Remember — LRU (Least Recently Used)**
>
> Chính sách thay thế: khi cần loại bỏ một mục để nhường chỗ (Cache đầy), **loại bỏ mục lâu nhất chưa được truy cập**. LRU là câu trả lời hình thức cho Bài tập 5, Chương 2.26 — nó khai thác trực tiếp giả định Temporal Locality: nếu một mục lâu chưa được dùng, khả năng nó sẽ sớm được dùng lại thường thấp hơn các mục vừa mới truy cập.

---

## 7. Nền tảng toán học

### 7.1 AMAT — mở rộng trực tiếp từ Chương 2.26

> **📦 Formula Box — Average Memory Access Time (AMAT)**
>
> $$\text{AMAT} = T_{\text{hit}} + \text{Miss Rate} \times \text{Miss Penalty}$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $T_{\text{hit}}$ | Thời gian truy cập khi Hit — tương ứng $T_{\text{gần}}$ ở Chương 2.26, Mục 7.2 |
> | $\text{Miss Rate}$ | Tỷ lệ Miss, bằng $1 - H$ (với $H$ là Hit Rate đã định nghĩa ở Chương 2.26) |
> | $\text{Miss Penalty}$ | **Thời gian phụ trội** phải trả thêm khi Miss — tức thời gian truy cập Main Memory, **cộng thêm** vào $T_{\text{hit}}$ đã tốn để phát hiện ra Miss (khác với $T_{\text{xa}}$ ở Chương 2.26, vốn coi là thời gian *độc lập*) |
> | **Diễn giải kỹ thuật** | Đây chính là Formula Box $T_{\text{avg}} = H \times T_{\text{gần}} + (1-H) \times T_{\text{xa}}$ ở Chương 2.26 (Mục 7.2), viết lại dưới dạng tương đương: $T_{\text{xa}} = T_{\text{hit}} + \text{Miss Penalty}$, vì khi Miss, hệ thống vẫn phải tốn $T_{\text{hit}}$ để *phát hiện* Miss (tra cứu Index/Tag, Mục 6) trước khi mới xuống Main Memory |
> | **Ứng dụng thường gặp** | Công cụ định lượng chuẩn để đánh giá và so sánh các thiết kế Cache khác nhau trong tài liệu kỹ thuật thực tế |

**Ví dụ kiểm chứng:** $T_{\text{hit}} = 1$ ns, $\text{Miss Rate} = 0.05$, $\text{Miss Penalty} = 100$ ns.

$$\text{AMAT} = 1 + 0.05 \times 100 = 1 + 5 = 6\text{ ns}$$

Kết quả này khớp với ví dụ ở Chương 2.26 (Mục 7.2: $T_{\text{avg}} = 6.9$ ns, dùng số liệu hơi khác) — cùng bản chất toán học, khác cách tham số hóa.

### 7.2 Tính Index bằng phép chia lấy dư

> **📦 Formula Box — Tính Index cho Direct-Mapped Cache**
>
> $$\text{Index} = \left\lfloor \frac{\text{Địa chỉ}}{\text{Kích thước khối}} \right\rfloor \bmod \text{Số lượng Set}$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | Phép chia cho Kích thước khối | Chuyển từ "địa chỉ byte" sang "địa chỉ khối" — bỏ qua phần Offset (Hình 2.27.1) |
> | Phép `mod` Số lượng Set | Đảm bảo Index luôn nằm trong phạm vi hợp lệ — chính là cơ chế khiến nhiều khối bộ nhớ khác nhau có thể "dùng chung" một Index, nguồn gốc của Conflict Miss (Mục 14) |
> | **Diễn giải kỹ thuật** | Vì Số lượng Set trong Cache thực tế luôn là lũy thừa của 2 (ví dụ $2^8 = 256$ như Hình 2.27.1), phép `mod` này trên phần cứng chỉ đơn giản là **trích xuất đúng dải bit Index** — không cần một mạch chia thực sự, giải thích tại sao thao tác này đủ nhanh để hoàn thành trong một phần nhỏ của một chu kỳ xung nhịp |

---

## 8. Thuật toán / Cơ chế

**Cơ chế tra cứu Direct-Mapped Cache** — trường hợp đơn giản nhất, làm nền tảng để hiểu Set-Associative (chỉ là lặp lại bước 3 cho N vị trí thay vì 1):

```text
Bước 1 — Nhận địa chỉ cần truy cập
        │
        ▼
Bước 2 — Phân tách địa chỉ thành Tag / Index / Offset
         (Hình 2.27.1, dùng phép toán bit — cực nhanh)
        │
        ▼
Bước 3 — Dùng Index để đi THẲNG đến đúng một vị trí
         trong Cache (không tìm kiếm — Mục 3)
        │
        ▼
Bước 4 — So sánh Tag lưu tại vị trí đó với Tag của địa chỉ
         cần tìm
        │
        ├─── TRÙNG khớp VÀ có dữ liệu hợp lệ ──► HIT
        │      Dùng Offset để lấy đúng byte cần trong
        │      Cache Line, trả về ngay
        │
        └─── KHÔNG trùng (hoặc trống) ──► MISS
               │
               ▼
             Lấy toàn bộ Cache Line chứa địa chỉ đó từ
             Main Memory, ghi đè vào đúng vị trí Index này
             (loại bỏ dữ liệu cũ nếu có — không cần LRU vì
             Direct-Mapped chỉ có đúng 1 lựa chọn)
             │
             ▼
             Cập nhật Tag tại vị trí đó, rồi trả dữ liệu
             về như trường hợp HIT
```

> **💡 Insight**
> Với **Set-Associative**, Bước 4 chỉ khác ở chỗ: thay vì so sánh đúng 1 Tag, mạch phần cứng so sánh **N Tag song song cùng lúc** (không phải tuần tự — nếu tuần tự sẽ mất N lần thời gian, phá vỡ mục tiêu tốc độ). Khi Miss và cần chọn vị trí để ghi đè trong N vị trí đó, đây chính là lúc **LRU** (Mục 6) được áp dụng để quyết định loại bỏ vị trí nào.

---

## 9. Triển khai

Xây dựng một **Direct-Mapped Cache Simulator** hoàn chỉnh bằng Python, hiện thực đúng cơ chế ở Mục 8, thay thế hoàn toàn cách mô phỏng đơn giản hóa (dict không giới hạn cấu trúc) ở Chương 2.26.

```python
class DirectMappedCache:
    """Cache ánh xạ trực tiếp — mỗi khối bộ nhớ có đúng 1 vị trí,
    đúng Formula Box Mục 7.2 và cơ chế Mục 8."""

    def __init__(self, num_sets=8, block_size=4):
        self.num_sets = num_sets
        self.block_size = block_size
        # mỗi set lưu (tag, data) hoặc None nếu trống
        self.sets = [None] * num_sets
        self.hits = 0
        self.misses = 0

    def _split_address(self, address):
        block_address = address // self.block_size
        index = block_address % self.num_sets
        tag = block_address // self.num_sets
        return tag, index

    def access(self, address, main_memory):
        tag, index = self._split_address(address)
        entry = self.sets[index]

        if entry is not None and entry[0] == tag:
            self.hits += 1
            return entry[1], "HIT"

        # MISS: nạp cả Cache Line từ main_memory (mô phỏng)
        self.misses += 1
        block_start = (address // self.block_size) * self.block_size
        block_data = [
            main_memory.get(block_start + i, 0)
            for i in range(self.block_size)
        ]
        self.sets[index] = (tag, block_data)
        return block_data, "MISS"

    def hit_rate(self):
        total = self.hits + self.misses
        return self.hits / total if total else 0.0
```

Điểm khác biệt cốt lõi so với `TwoLevelMemory` ở Chương 2.26: cấu trúc `self.sets` có **kích thước cố định**, và vị trí lưu trữ được **tính toán trực tiếp** bằng `_split_address` thay vì dùng dict tra cứu tuỳ ý — đúng bản chất phần cứng thật của Cache.

---

## 10. Trực quan hóa quá trình thực thi

Mô phỏng một chuỗi truy cập minh họa cả Hit, Miss thông thường, và **Conflict Miss** (Mục 14) — hai địa chỉ khác nhau nhưng cùng Index:

```python
memory = {i: i * 100 for i in range(64)}
cache = DirectMappedCache(num_sets=4, block_size=4)

addresses = [0, 4, 0, 16, 0]   # 16 và 0 cùng Index (num_sets=4)
for addr in addresses:
    data, status = cache.access(addr, memory)
    print(f"Địa chỉ {addr}: {status}")
```

**Bảng vết thực thi:**

| Địa chỉ | Block Address (`addr // 4`) | Tag (`// 4`) | Index (`% 4`) | Trạng thái | Giải thích |
|---:|---:|---:|---:|---|---|
| 0 | 0 | 0 | 0 | MISS | Cache trống ban đầu (Compulsory Miss, Mục 14) |
| 4 | 1 | 0 | 1 | MISS | Vị trí Index 1 còn trống, khác khối, khác Index với địa chỉ 0 |
| 0 | 0 | 0 | 0 | **HIT** | Trùng Tag+Index với lần truy cập đầu — dữ liệu vẫn còn trong Cache |
| 16 | 4 | **1** | **0** | MISS | Cùng Index=0 với địa chỉ 0, nhưng Tag khác (1 ≠ 0) → ghi đè lên vị trí của địa chỉ 0 |
| 0 | 0 | 0 | 0 | MISS | **Conflict Miss** — dữ liệu của địa chỉ 0 vừa bị địa chỉ 16 ghi đè ở bước trước, dù Cache còn dư dung lượng ở các Index khác |

Dòng cuối cùng minh họa chính xác hạn chế cốt lõi của Direct-Mapped Cache sẽ phân tích ở Mục 14: hai địa chỉ hoàn toàn không liên quan (`0` và `16`) "tranh chấp" cùng một vị trí, gây Miss dù tổng dung lượng Cache chưa hề bị lấp đầy.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Thiết kế N-way Set-Associative gần như luôn là lựa chọn thực tế trong CPU thương mại — hiếm khi dùng thuần Direct-Mapped hay Fully Associative ở quy mô lớn.

| Bối cảnh công nghiệp | Liên hệ với nội dung chương |
|---|---|
| L1 Cache trong CPU hiện đại | Thường nhỏ (vài chục KB), độ liên kết thấp (ví dụ 4-way hoặc 8-way), ưu tiên tốc độ cực cao (Mục 5, Hình 2.27.2) |
| L3 Cache (Last-Level Cache) | Lớn hơn nhiều (hàng chục MB), độ liên kết cao hơn, chia sẻ giữa nhiều nhân CPU (multi-core) |
| Trình biên dịch tối ưu hóa vòng lặp (loop tiling / loop blocking) | Kỹ thuật tái tổ chức code để dữ liệu truy cập trong một vòng lặp vừa đủ nhỏ để nằm gọn trong Cache, giảm thiểu Capacity Miss (Mục 14) |
| Công cụ profiling hiệu năng (`perf`, Valgrind Cachegrind) | Đo trực tiếp Hit Rate/Miss Rate thực tế của chương trình đang chạy — hiện thân công nghiệp của chính phép đo ở Mục 10 |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Khi một hệ thống có nhiều nhân CPU (multi-core), mỗi nhân thường có Cache L1/L2 riêng — điều này tạo ra một bài toán mới, phức tạp hơn nhiều so với mọi thứ đã bàn ở chương này.

Nếu hai nhân CPU khác nhau cùng có bản sao Cache của **cùng một địa chỉ** Main Memory, và một nhân thay đổi giá trị đó, làm sao đảm bảo nhân còn lại không tiếp tục sử dụng giá trị cũ đã lỗi thời trong Cache riêng của nó? Đây là bài toán **Cache Coherence (Tính nhất quán Cache)** — một trong những vấn đề nghiên cứu trung tâm của kiến trúc máy tính đa nhân hiện đại, đòi hỏi các giao thức phối hợp phức tạp giữa nhiều Cache. Bài toán này nằm ngoài phạm vi giới thiệu của Volume 2, và sẽ được trình bày đầy đủ ở **Volume 4** khi bàn về Concurrency và Distributed Systems — nhưng đáng để nêu tên ở đây, vì nó là hệ quả trực tiếp, gần như tất yếu, của việc áp dụng cùng ý tưởng Cache (đã học ở chương này) vào một hệ thống có nhiều CPU thay vì một.

**Câu hỏi mở** để suy ngẫm: Formula Box AMAT (Mục 7.1) giả định Miss Penalty là một hằng số cố định. Trong một hệ thống Cache đa tầng thực tế (L1, L2, L3, rồi mới đến Main Memory), Miss Penalty của L1 thực chất phụ thuộc vào việc L2 có Hit hay không — vốn lại phụ thuộc vào L3. Hãy thử tự đề xuất (không cần công thức hoàn chỉnh) cách mở rộng AMAT để mô tả một hệ thống Cache ba tầng như vậy.

---

## 13. Ưu điểm

- **Tra cứu gần như tức thì** (Mục 3, 8) — giải quyết đúng vấn đề kỹ thuật đặt ra ở đầu chương, nhờ cơ chế Index tính trực tiếp bằng phép toán bit thay vì tìm kiếm.
- **Khai thác đồng thời cả hai loại Locality** (Chương 2.26): Temporal Locality qua LRU (Mục 6), Spatial Locality qua Cache Line (Mục 6).
- **Có thể điều chỉnh linh hoạt trade-off tốc độ/hiệu quả** thông qua lựa chọn Direct-Mapped/Set-Associative/Fully Associative (Mục 15), tùy mục tiêu thiết kế cụ thể (L1 ưu tiên tốc độ, L3 ưu tiên Hit Rate).

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Người mới học thường nghĩ Cache Miss chỉ xảy ra khi "Cache đã đầy". Bảng vết ở Mục 10 cho thấy điều này **không đúng** — Conflict Miss có thể xảy ra ngay cả khi Cache còn nhiều dung lượng trống, chỉ vì hai địa chỉ tình cờ có cùng Index.

Ba nguyên nhân gây Cache Miss cần phân biệt rõ:

| Loại Miss | Nguyên nhân |
|---|---|
| **Compulsory Miss** (Cold Miss) | Lần đầu tiên truy cập một khối dữ liệu — Cache chưa từng có cơ hội nạp nó, không thể tránh khỏi dù thiết kế Cache có tốt đến đâu |
| **Capacity Miss** | Cache không đủ dung lượng chứa toàn bộ dữ liệu đang cần dùng, dù chính sách Mapping có tối ưu đến đâu |
| **Conflict Miss** | Riêng của Direct-Mapped và Set-Associative (không xảy ra với Fully Associative) — nhiều khối tranh chấp cùng một Index/Set dù Cache tổng thể còn dư dung lượng (đã minh họa cụ thể ở Mục 10) |

Direct-Mapped, dù nhanh nhất khi tra cứu, có tỷ lệ Conflict Miss cao nhất trong ba kiểu Mapping — chính là trade-off cốt lõi đã nêu ở Hình 2.27.2.

---

## 15. So sánh

**Bảng 2.27.1 — Ba kiểu Cache Mapping**

| Tiêu chí | Direct-Mapped | Set-Associative (N-way) | Fully Associative |
|---|---|---|---|
| Số vị trí có thể chứa 1 khối | 1 | N | Bất kỳ vị trí trống nào |
| Tốc độ tra cứu | Nhanh nhất | Trung bình | Chậm nhất |
| Conflict Miss | Cao nhất | Thấp hơn (giảm khi N tăng) | **Không có** Conflict Miss |
| Chi phí phần cứng (mạch so sánh Tag) | Thấp nhất (1 bộ so sánh) | Trung bình (N bộ so sánh song song) | Cao nhất (so sánh mọi vị trí song song) |
| Cần chính sách thay thế (LRU)? | Không cần (chỉ 1 lựa chọn) | Có, trong phạm vi N vị trí | Có, trong phạm vi toàn Cache |

**Phân tích:** đây là một trade-off ba chiều giữa **tốc độ**, **tỷ lệ Miss**, và **chi phí phần cứng** — không có lựa chọn thắng tuyệt đối, đúng mô-típ đã lặp lại xuyên suốt Part V (RISC/CISC ở Chương 2.23, Von Neumann/Harvard ở Chương 2.22, Single-cycle/Multi-cycle ở Chương 2.24). Trong thực tế, **N-way Set-Associative** với $N$ nhỏ (4 hoặc 8) thường là điểm cân bằng được lựa chọn phổ biến nhất, như đã nêu ở Mục 11.

---

## 16. Tóm tắt

- Một địa chỉ bộ nhớ được phân tách thành ba trường **Tag / Index / Offset** (Hình 2.27.1) để cho phép tra cứu Cache gần như tức thì, không cần tìm kiếm tuần tự.
- Ba kiểu **Cache Mapping** — Direct-Mapped, Fully Associative, Set-Associative — đánh đổi giữa tốc độ tra cứu và tỷ lệ Conflict Miss (Bảng 2.27.1).
- **AMAT** $= T_{\text{hit}} + \text{Miss Rate} \times \text{Miss Penalty}$ là công thức chuẩn để định lượng hiệu năng Cache, cùng bản chất toán học với $T_{\text{avg}}$ ở Chương 2.26 nhưng tham số hóa theo góc nhìn "chi phí phụ trội khi Miss".
- **LRU** là chính sách thay thế phổ biến, khai thác trực tiếp Temporal Locality khi Cache (hoặc một Set) đã đầy.
- Ba nguyên nhân Cache Miss — **Compulsory, Capacity, Conflict** — cần được phân biệt rõ để hiểu đúng giới hạn của một thiết kế Cache cụ thể.

Chương 2.28, chương cuối cùng của Part V, sẽ chuyển hướng khỏi CPU và Memory để khảo sát cách máy tính giao tiếp với thế giới bên ngoài — **Input/Output Systems** — khép lại bức tranh tổng thể đã mở ra từ Chương 2.22.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Với Cache có `num_sets=4`, `block_size=4` (như Mục 9–10), tính Tag và Index cho địa chỉ `24` và địa chỉ `8`. Hai địa chỉ này có tranh chấp cùng một Index không?
2. Giải thích bằng lời của riêng bạn tại sao Fully Associative **không thể** có Conflict Miss, dựa trên định nghĩa ở Mục 6 và Bảng 2.27.1.

### Mức Trung bình (Intermediate)

3. Áp dụng Formula Box AMAT (Mục 7.1): một Cache có $T_{\text{hit}} = 2$ ns, $\text{Miss Penalty} = 50$ ns. Tính AMAT khi Miss Rate lần lượt là 1%, 10%, và 50%. Nhận xét mức độ ảnh hưởng của Miss Rate đến AMAT.
4. Dùng `DirectMappedCache` ở Mục 9 (hoặc trace bằng tay theo mẫu Mục 10), xác định trạng thái Hit/Miss cho chuỗi truy cập `[0, 4, 8, 12, 0, 4]` với `num_sets=4, block_size=4`. Chỉ rõ Miss nào là Compulsory và Miss nào (nếu có) là Conflict.

### Mức Nâng cao (Advanced)

5. Thiết kế Direct-Mapped ở Mục 9 chỉ lưu được đúng 1 khối mỗi Index. Hãy mô tả (bằng lời hoặc pseudocode, không cần code Python hoàn chỉnh) cách bạn sẽ sửa `DirectMappedCache` thành **2-way Set-Associative** — mỗi Index giờ cần lưu tối đa 2 khối, và khi cả hai đều đã có dữ liệu, cần áp dụng LRU (Mục 6) để quyết định loại bỏ khối nào.

### Mức Nghiên cứu (Research)

6. Mục 12 giới thiệu bài toán Cache Coherence trong hệ thống đa nhân. Hãy suy nghĩ (không cần lời giải hoàn chỉnh): nếu hai nhân CPU A và B cùng có bản sao Cache của địa chỉ X, và nhân A thay đổi giá trị tại X, hãy liệt kê ít nhất hai chiến lược khác nhau (ở mức ý tưởng, không cần thuật ngữ chuyên ngành chính xác) mà hệ thống có thể dùng để đảm bảo nhân B không đọc phải giá trị cũ.

---

## 18. Dự án nhỏ

**Dự án: So sánh Hit Rate giữa Direct-Mapped và Set-Associative trên cùng một mẫu truy cập**

- **Mục tiêu:** chứng minh bằng thực nghiệm lợi ích của Set-Associative trong việc giảm Conflict Miss, củng cố Mục 14–15.
- **Yêu cầu:**
  1. Hoàn thiện phần mở rộng "2-way Set-Associative" đã mô tả ở Bài tập 5 thành code Python thực sự chạy được.
  2. Thiết kế một mẫu truy cập bộ nhớ **cố tình** gây nhiều Conflict Miss cho Direct-Mapped (ví dụ các địa chỉ cách nhau đúng bằng `num_sets × block_size`).
  3. Chạy cùng mẫu truy cập đó qua cả `DirectMappedCache` (Mục 9) và cache Set-Associative vừa xây dựng, so sánh Hit Rate.
  4. Trình bày kết quả dưới dạng bảng, kèm nhận xét bằng lời.
- **Công nghệ đề xuất:** Python thuần.
- **Mở rộng (tùy chọn):** thử với `num_sets` khác nhau và vẽ biểu đồ Hit Rate theo số lượng Set, cho cả hai kiểu Mapping.

---

## 19. Tự đánh giá

- [ ] Tôi có thể phân tách một địa chỉ cụ thể thành Tag/Index/Offset khi biết `block_size` và `num_sets`, không cần nhìn lại Formula Box.
- [ ] Tôi có thể giải thích rõ ràng sự khác biệt giữa ba kiểu Cache Mapping, kèm trade-off tốc độ/Conflict Miss cho mỗi kiểu.
- [ ] Tôi có thể áp dụng đúng công thức AMAT và giải thích mối liên hệ của nó với Formula Box $T_{\text{avg}}$ ở Chương 2.26.
- [ ] Tôi có thể phân biệt Compulsory, Capacity, và Conflict Miss, và xác định đúng loại Miss trong một bảng vết thực thi cụ thể (như Bài tập 4).
- [ ] Tôi hiểu vì sao LRU khai thác Temporal Locality, và có thể giải thích ý tưởng đó mà không cần thuật ngữ kỹ thuật.

Nếu Bài tập 4 (phân loại Miss) vẫn còn khó, đây là dấu hiệu nên ôn lại bảng vết mẫu ở Mục 10 trước khi sang Chương 2.28 — khả năng phân biệt các loại Miss là kỹ năng phân tích quan trọng nhất của toàn bộ chương này.

---

## 20. Đọc thêm

- **Sách:** Randal E. Bryant, David R. O'Hallaron, *Computer Systems: A Programmer's Perspective* — chương về Cache Memory, trình bày rất chi tiết cách viết code "cache-friendly" trong thực tế, mở rộng trực tiếp Mục 11 của chương này. *(Xem BOOKS.md.)*
- **Sách:** Andrew S. Tanenbaum, *Modern Operating Systems* — liên hệ giữa Cache phần cứng và các cơ chế cache ở tầng hệ điều hành (page cache), chủ đề của Part VI sắp tới. *(Xem BOOKS.md.)*
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc về công cụ **Valgrind Cachegrind** hoặc `perf stat` trên Linux — cả hai cho phép đo Hit Rate/Miss Rate thực tế của một chương trình đang chạy, biến mọi khái niệm lý thuyết ở chương này thành số liệu quan sát được trực tiếp trên máy tính của bạn.
- **Chương tiếp theo:** Chương 2.28 — Input/Output Systems.

---

### Liên kết chương (Cross References)

- **Chương trước:** 2.26 — Memory Hierarchy (nguyên lý chung "kiểm tra gần trước" nay được chuyên biệt hóa thành cơ chế Tag/Index/Offset cụ thể).
- **Chương tiếp theo:** 2.28 — Input/Output Systems (chương cuối Part V, chuyển từ xử lý/lưu trữ dữ liệu nội bộ sang giao tiếp với thiết bị ngoài).
- **Chương liên quan xa hơn:** Chương 2.26, Mục 7.2 (Formula Box $T_{\text{avg}}$ là tiền đề trực tiếp của AMAT ở Mục 7.1 chương này); Volume 3 (Hash Table — cấu trúc dữ liệu có cơ chế tra cứu O(1) tương tự tinh thần Cache, dù hiện thực hoàn toàn bằng phần mềm thay vì phần cứng); Volume 4 (Cache Coherence trong hệ thống đa nhân, Distributed Systems).
- **Vị trí trong Knowledge Graph:** Nút thứ sáu của Volume 2, Part V; phụ thuộc trực tiếp vào Chương 2.26; là chương có độ khó kỹ thuật cao nhất Part V (★★★★☆), đóng vai trò chuẩn bị trực tiếp cho các chủ đề Cache Coherence và tối ưu hiệu năng nâng cao ở Volume 4.

---

*Hết Chương 2.27. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`, khớp phong cách trình bày đã thiết lập từ Chương 1.5 và các chương trước của Part V. Công thức AMAT được xây dựng có chủ đích như một phép biến đổi tương đương của Formula Box $T_{\text{avg}}$ ở Chương 2.26, đảm bảo tính liên tục toán học xuyên suốt Part V. Đang chờ rà soát trước khi tiếp tục sang Chương 2.28 — chương khép lại Part V.*
