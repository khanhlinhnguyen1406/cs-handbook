# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 02 — Computer Science Foundations
### Part V — Computer Organization & Architecture
## Chương 2.26 — Memory Hierarchy

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 2.26 |
| Thuộc Part | V — Computer Organization & Architecture |
| Thuộc Volume | 02 — Computer Science Foundations |
| Thời gian đọc ước tính | 45–55 phút |
| Độ khó | ★★★☆☆ |
| Kiến thức tiên quyết | Chương 2.22 — Tổng quan (đặc biệt Von Neumann Bottleneck, Mục 14); Chương 2.25 — Pipelining (giai đoạn MEM) |
| Chương liên quan | 2.27 — Cache Memory (áp dụng trực tiếp và mở rộng mọi khái niệm ở chương này) |
| Từ khóa | Memory Hierarchy, Locality of Reference, Temporal Locality, Spatial Locality, Hit Rate, Trade-off Speed-Cost-Capacity |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Giải thích vì sao không tồn tại một loại bộ nhớ duy nhất "vừa nhanh, vừa rẻ, vừa dung lượng lớn" — và đây là động lực nền tảng của **Memory Hierarchy**.
- Mô tả các tầng của Memory Hierarchy theo thứ tự: Register → Cache → Main Memory → Secondary Storage, cùng đặc điểm tốc độ/dung lượng/chi phí của từng tầng.
- Định nghĩa và phân biệt **Temporal Locality** và **Spatial Locality**.
- Áp dụng công thức thời gian truy cập trung bình hai tầng để tính hiệu quả của một hệ thống bộ nhớ phân tầng đơn giản.
- Giải thích nguyên lý chung "kiểm tra tầng gần trước, chỉ xuống tầng xa khi cần" — nền tảng trực tiếp cho cơ chế Cache sẽ học ở Chương 2.27.

---

### Câu hỏi khơi gợi

> *Nếu có thể, tại sao các nhà thiết kế máy tính không làm toàn bộ RAM nhanh như thanh ghi (register) bên trong CPU? Câu trả lời không nằm ở "chưa ai nghĩ ra cách" — mà nằm ở một quy luật vật lý và kinh tế gần như không thể phá vỡ. Quy luật đó là gì?*

---

## 1. Tổng quan chương

Chương 2.22 (Mục 14) đã nêu tên **Von Neumann Bottleneck** — hạn chế của việc CPU và Memory dùng chung một Bus — nhưng chưa giải quyết nó. Chương 2.25 cho thấy giai đoạn **MEM** trong Pipeline có thể trở thành điểm nghẽn nếu việc truy cập Memory quá chậm so với các giai đoạn khác. Chương này trực tiếp trả lời câu hỏi: **làm sao để CPU truy cập dữ liệu nhanh, mà không cần cả hệ thống bộ nhớ đều nhanh (và do đó, đắt đỏ)?**

Câu trả lời là **Memory Hierarchy** — không dùng một loại bộ nhớ duy nhất, mà tổ chức nhiều tầng bộ nhớ khác nhau, mỗi tầng đánh đổi tốc độ lấy dung lượng (hoặc ngược lại), và khai thác một quan sát thực nghiệm quan trọng về hành vi chương trình gọi là **Locality of Reference**.

> **💡 Insight**
> Đây là một trong những ý tưởng có ảnh hưởng lớn nhất trong toàn bộ Computer Science — không chỉ giới hạn ở phần cứng. Nguyên lý "giữ dữ liệu hay dùng ở gần, dữ liệu ít dùng ở xa" xuất hiện lặp lại dưới nhiều hình thức: bộ nhớ đệm trình duyệt (browser cache), CDN trong hệ thống phân tán (Volume 4), và thậm chí cả cách con người tổ chức bàn làm việc.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Sự kiện | Ý nghĩa |
|---|---|---|
| Thập niên 1950s | Bộ nhớ lõi từ (magnetic core memory) | Công nghệ bộ nhớ chính phổ biến thời kỳ đầu — chậm, đắt, nhưng bền (không mất dữ liệu khi mất điện) |
| 1966 | Robert Dennard (IBM) phát minh **DRAM** (Dynamic RAM) | Đặt nền cho công nghệ bộ nhớ chính hiện đại — rẻ hơn đáng kể so với các công nghệ trước, nhưng cần "làm tươi" (refresh) liên tục để giữ dữ liệu |
| 1965 | Maurice Wilkes (đã gặp ở Chương 2.24, Mục 2) đề xuất khái niệm **Cache Memory** | Lần đầu tiên hình thức hóa ý tưởng thêm một tầng bộ nhớ nhỏ, cực nhanh, giữa CPU và Main Memory — nền tảng trực tiếp cho Chương 2.27 |
| Từ 1980s → nay | Khoảng cách tốc độ CPU và Memory ngày càng nới rộng ("Memory Wall") | Tốc độ CPU tăng nhanh hơn tốc độ Memory qua nhiều thập kỷ, khiến Memory Hierarchy chuyển từ "cải tiến hữu ích" thành "yêu cầu bắt buộc" đối với hiệu năng hệ thống |

---

## 3. Động lực

Quay lại ví dụ ở Chương 2.22 (Mục 3): hai cách duyệt ma trận có cùng độ phức tạp $O(n^2)$ nhưng tốc độ thực tế khác nhau đáng kể. Chương 2.22 hẹn sẽ giải thích đầy đủ ở đây — và giờ ta đã có đủ công cụ.

Hãy hình dung bàn làm việc của bạn: tài liệu đang đọc dở nằm ngay trước mặt (truy cập tức thì), tài liệu tham khảo gần đây nằm trên kệ sách cạnh bàn (mất vài giây để lấy), còn tài liệu cũ hơn nằm trong kho lưu trữ ở tầng hầm (mất vài phút để đi lấy). Không ai thiết kế văn phòng với "toàn bộ tài liệu đều nằm ngay trước mặt" — vì diện tích bàn có hạn. Thay vào đó, ta tự nhiên tổ chức theo tần suất sử dụng. Memory Hierarchy áp dụng đúng logic đó vào phần cứng, và chính cách một chương trình *truy cập bộ nhớ theo mẫu hình* (Mục 6) quyết định nó tận dụng được tổ chức phân tầng này tốt đến đâu.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Memory Hierarchy giống như một **thư viện được tổ chức theo tầng**: sách đang đọc nằm trên bàn của bạn (nhanh nhất, nhưng chỉ chứa được vài cuốn); sách mượn gần đây nằm trên giá sách cá nhân cạnh bàn (nhanh, chứa được vài chục cuốn); phần lớn sách còn lại nằm trong kho lưu trữ trung tâm của thư viện (chậm hơn nhiều, nhưng chứa được hàng triệu cuốn). Bạn không cần — và không thể — có mọi cuốn sách ngay trên bàn cùng lúc.

| Trực giác kỹ thuật bạn đã có | Khái niệm Memory Hierarchy tương ứng |
|---|---|
| Biến cục bộ trong một hàm đang chạy | Register — tầng nhanh nhất, dung lượng nhỏ nhất |
| Browser cache lưu ảnh/CSS đã tải trước đó | Cache Memory (Chương 2.27) |
| RAM của máy tính | Main Memory |
| Ổ cứng / SSD lưu file lâu dài | Secondary Storage |

---

## 5. Trực quan hóa khái niệm

**Hình 2.26.1 — Kim tự tháp Memory Hierarchy**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
                    ┌───────────┐
                    │ Registers │   ← nhanh nhất, nhỏ nhất, đắt nhất/byte
                    ├───────────┤
                   ╱│   Cache    │╲   ← rất nhanh, nhỏ (Chương 2.27)
                  ╱ ├───────────┤ ╲
                 ╱  │Main Memory │  ╲  ← nhanh vừa phải, dung lượng vừa (RAM)
                ╱   ├───────────┤   ╲
               ╱    │  Secondary  │    ╲  ← chậm nhất, dung lượng lớn nhất,
              ╱     │   Storage   │     ╲    rẻ nhất/byte (SSD, HDD)
             ╱      └───────────┘      ╲
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Cho thấy trực quan mối quan hệ nghịch đảo giữa tốc độ và dung lượng — càng lên cao (nhanh hơn), diện tích (dung lượng) càng nhỏ |
| Điểm mấu chốt | Không tầng nào "tốt hơn" tầng khác một cách tuyệt đối — mỗi tầng tồn tại vì một lý do kinh tế/vật lý cụ thể (Mục 7); toàn bộ hệ thống hoạt động hiệu quả nhờ cách các tầng **phối hợp** với nhau, không phải nhờ một tầng đơn lẻ nào xuất sắc |

---

**Hình 2.26.2 — Temporal Locality và Spatial Locality**

```text
for i in range(1000):
    total += array[i]      # (A) Spatial Locality: array[i] và array[i+1]
                            #     nằm SÁT NHAU trong bộ nhớ
    print(total)            # (B) Temporal Locality: biến 'total' được
                            #     truy cập LẶP LẠI ở mỗi vòng lặp
```

*Mục đích:* minh họa hai dạng "mẫu hình sử dụng" cụ thể ngay trong một đoạn code quen thuộc — không phải khái niệm trừu tượng xa vời. *Điểm mấu chốt:* chính hai mẫu hình này là lý do tại sao Memory Hierarchy **hoạt động tốt trong thực tế**, thay vì chỉ là một ý tưởng lý thuyết đẹp trên giấy — sẽ chứng minh định lượng ở Mục 9–10.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Memory Hierarchy**
>
> **Memory Hierarchy** là cách tổ chức hệ thống bộ nhớ của máy tính thành nhiều **tầng (level)**, sắp xếp theo thứ tự giảm dần về tốc độ và tăng dần về dung lượng, nhằm mang lại **ảo giác** về một bộ nhớ vừa nhanh như tầng nhanh nhất, vừa lớn như tầng lớn nhất — mà không cần trả chi phí thực tế cho điều đó.

| Tầng | Tốc độ tương đối | Dung lượng tương đối | Vị trí vật lý |
|---|---|---|---|
| Register | Nhanh nhất (~1 chu kỳ) | Nhỏ nhất (vài chục thanh ghi) | Ngay trong CPU (Chương 2.24) |
| Cache | Rất nhanh (vài chu kỳ) | Nhỏ (KB đến vài chục MB) | Trong hoặc rất gần CPU (Chương 2.27) |
| Main Memory | Vừa phải (hàng chục–hàng trăm chu kỳ) | Vừa (GB) | Trên bo mạch chủ, qua Bus (Chương 2.22) |
| Secondary Storage | Chậm nhất (hàng nghìn–hàng triệu chu kỳ) | Lớn nhất (TB) | Thiết bị lưu trữ ngoài (SSD, HDD) |

> **📌 Remember — Locality of Reference (Tính cục bộ tham chiếu)**
>
> Một quan sát thực nghiệm mang tính quy luật: **chương trình không truy cập bộ nhớ một cách ngẫu nhiên** — chúng có xu hướng truy cập theo các mẫu hình dự đoán được, chia thành hai loại:
>
> - **Temporal Locality:** nếu một địa chỉ bộ nhớ vừa được truy cập, nó **có khả năng cao sẽ được truy cập lại trong tương lai gần** (ví dụ: biến vòng lặp).
> - **Spatial Locality:** nếu một địa chỉ bộ nhớ vừa được truy cập, các địa chỉ **nằm gần nó** có khả năng cao sẽ sớm được truy cập (ví dụ: các phần tử liền kề trong mảng).

---

## 7. Nền tảng toán học

### 7.1 Vì sao không thể có "bộ nhớ hoàn hảo"

Đây không phải một công thức duy nhất, mà là một **quan hệ đánh đổi (trade-off)** mang tính vật lý và kinh tế: bộ nhớ càng nhanh (dùng công nghệ như SRAM cho Register/Cache) thì càng tốn diện tích chip và chi phí sản xuất trên mỗi byte; bộ nhớ càng rẻ và dung lượng lớn (như đĩa từ) thì càng chậm về bản chất vật lý (cơ chế cơ khí, khoảng cách tín hiệu điện). Không có đột phá công nghệ nào (tính đến kiến thức nền tảng của Handbook) phá vỡ hoàn toàn quan hệ đánh đổi ba chiều này — Tốc độ, Dung lượng, Chi phí — cùng lúc.

### 7.2 Thời gian truy cập trung bình của hệ thống hai tầng

> **📦 Formula Box — Thời gian truy cập trung bình (Two-Level Average Access Time)**
>
> $$T_{\text{avg}} = H \times T_{\text{gần}} + (1 - H) \times T_{\text{xa}}$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $H$ | **Hit Rate** — tỷ lệ (0 đến 1) các lần truy cập được đáp ứng ngay ở tầng gần hơn, không cần xuống tầng xa |
> | $T_{\text{gần}}$ | Thời gian truy cập tầng gần (nhanh) |
> | $T_{\text{xa}}$ | Thời gian truy cập tầng xa (chậm) — thường bao gồm cả thời gian đã "phí" khi kiểm tra tầng gần trước rồi mới xuống tầng xa |
> | **Diễn giải kỹ thuật** | Nếu $H$ càng gần 1 (hầu hết truy cập được tầng gần đáp ứng), $T_{\text{avg}}$ càng gần $T_{\text{gần}}$ — đây chính là "ảo giác tốc độ" đã nêu ở Mục 6: hệ thống *cảm giác* nhanh gần như tầng nhanh nhất, dù phần lớn dữ liệu thực ra nằm ở tầng chậm hơn |
> | **Ứng dụng thường gặp** | Công thức tổng quát này sẽ được Chương 2.27 mở rộng thành **AMAT (Average Memory Access Time)** — phiên bản chuyên biệt hóa cho Cache, có thêm khái niệm Miss Penalty |

**Ví dụ kiểm chứng:** giả sử $T_{\text{gần}} = 2$ ns, $T_{\text{xa}} = 100$ ns, và $H = 0.95$ (95% truy cập được tầng gần đáp ứng — một con số thực tế hợp lý nhờ Locality of Reference). Khi đó:

$$T_{\text{avg}} = 0.95 \times 2 + 0.05 \times 100 = 1.9 + 5 = 6.9 \text{ ns}$$

Kết quả $6.9$ ns gần với $T_{\text{gần}} = 2$ ns hơn nhiều so với $T_{\text{xa}} = 100$ ns — minh chứng định lượng cho "ảo giác tốc độ" nói trên, và cũng là lời giải thích đầy đủ, có số liệu, cho hiện tượng duyệt ma trận ở Chương 2.22, Mục 3.

---

## 8. Thuật toán / Cơ chế

**Cơ chế truy cập tổng quát trong hệ thống phân tầng** — nguyên lý chung áp dụng cho mọi cặp tầng liền kề (Register–Cache, Cache–Main Memory, Main Memory–Storage):

```text
Bước 1 — CPU cần đọc dữ liệu tại địa chỉ X
        │
        ▼
Bước 2 — Kiểm tra: dữ liệu tại X có đang nằm ở TẦNG GẦN không?
        │
        ├─── CÓ (Hit) ──► Bước 3a: Trả dữ liệu ngay, tốn T_gần
        │
        └─── KHÔNG (Miss) ──► Bước 3b:
                │
                ▼
              Truy cập TẦNG XA để lấy dữ liệu, tốn T_xa
                │
                ▼
              Sao chép dữ liệu đó (và thường cả các địa chỉ
              LÂN CẬN, tận dụng Spatial Locality) vào TẦNG GẦN,
              để lần truy cập sau (nếu có, nhờ Temporal Locality)
              sẽ là Hit
```

> **💡 Insight**
> Bước cuối cùng — "sao chép dữ liệu và cả các địa chỉ lân cận vào tầng gần" — chính là nơi Spatial Locality được **chủ động khai thác**, chứ không chỉ đơn thuần được "quan sát thấy". Đây là cơ chế cốt lõi mà Chương 2.27 sẽ mô tả chi tiết dưới tên gọi **Cache Line** — CPU không bao giờ chỉ sao chép đúng một byte cần dùng, mà sao chép cả một khối liền kề.

---

## 9. Triển khai

Mô phỏng hệ thống bộ nhớ hai tầng bằng Python, đo lường trực tiếp lợi ích của Locality of Reference — không dùng số liệu giả định như Mục 7.2, mà **tính ra** Hit Rate từ một mẫu truy cập cụ thể.

```python
import time


class TwoLevelMemory:
    """Mô phỏng hệ thống hai tầng: 'near' (nhanh, nhỏ) và
    'far' (chậm, lớn), đúng cơ chế ở Mục 8."""

    def __init__(self, near_capacity=4):
        self.near_capacity = near_capacity
        self.near = {}          # tầng gần — giới hạn dung lượng
        self.far = {i: i * 10 for i in range(1000)}  # tầng xa — "vô hạn"
        self.hits = 0
        self.misses = 0

    def access(self, address):
        if address in self.near:
            self.hits += 1
            return self.near[address]

        # MISS: lấy từ tầng xa, rồi đưa vào tầng gần
        self.misses += 1
        value = self.far[address]

        if len(self.near) >= self.near_capacity:
            # đơn giản hóa: loại bỏ một mục bất kỳ khi đầy
            self.near.pop(next(iter(self.near)))
        self.near[address] = value
        return value

    def hit_rate(self):
        total = self.hits + self.misses
        return self.hits / total if total else 0.0


def access_pattern_with_locality(mem, array_indices):
    """Mô phỏng Hình 2.26.2 (B): truy cập lặp lại cùng vài địa chỉ,
    khai thác Temporal Locality."""
    for _ in range(10):          # lặp lại nhiều vòng (Temporal Locality)
        for idx in array_indices:  # các chỉ số liền kề (Spatial Locality)
            mem.access(idx)
```

---

## 10. Trực quan hóa quá trình thực thi

So sánh hai mẫu truy cập: **có Locality** (giống Hình 2.26.2) và **ngẫu nhiên hoàn toàn** (không có Locality):

```python
import random

mem_locality = TwoLevelMemory(near_capacity=4)
access_pattern_with_locality(mem_locality, [0, 1, 2, 3])

mem_random = TwoLevelMemory(near_capacity=4)
random.seed(42)
for _ in range(40):
    mem_random.access(random.randint(0, 999))

print(f"Hit Rate (có Locality):   {mem_locality.hit_rate():.2%}")
print(f"Hit Rate (ngẫu nhiên):    {mem_random.hit_rate():.2%}")
```

**Bảng kết quả:**

| Mẫu truy cập | Số lần truy cập | Hit | Miss | Hit Rate |
|---|---:|---:|---:|---:|
| Có Locality (`[0,1,2,3]` lặp 10 vòng) | 40 | 36 | 4 | 90.0% |
| Ngẫu nhiên hoàn toàn | 40 | 0 | 40 | 0.0% |

*(Số liệu Hit minh họa theo đúng logic thuật toán ở Mục 8–9: 4 lần Miss đầu tiên của mẫu có Locality là để nạp `0,1,2,3` vào tầng gần lần đầu; toàn bộ 36 lần truy cập còn lại đều Hit.)*

Áp dụng Formula Box Mục 7.2 với $H=0.9$ (kết quả đo được) so với $H=0.0$ (mẫu ngẫu nhiên): chênh lệch $T_{\text{avg}}$ giữa hai trường hợp minh chứng bằng số liệu thực nghiệm — không còn là giả định — cho lý do vì sao thứ tự truy cập bộ nhớ ảnh hưởng mạnh đến hiệu năng chương trình thực tế.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Locality of Reference không chỉ là hiện tượng phần cứng "âm thầm" hưởng lợi — nó là kiến thức mà kỹ sư phần mềm có thể **chủ động khai thác**.

| Bối cảnh công nghiệp | Liên hệ với nội dung chương |
|---|---|
| Duyệt mảng/matrix theo đúng thứ tự lưu trữ trong bộ nhớ (row-major) | Khai thác trực tiếp Spatial Locality — chính là lời giải đầy đủ cho ví dụ mở đầu Chương 2.22 |
| Thiết kế cấu trúc dữ liệu "cache-friendly" (ví dụ B-Tree trong cơ sở dữ liệu, thay vì Binary Search Tree thông thường) | Tối ưu hóa số lượng Miss khi truy cập, dựa trực tiếp trên nguyên lý Mục 8 |
| CDN (Content Delivery Network) trong hệ thống phân tán | Áp dụng đúng triết lý Memory Hierarchy ở quy mô toàn cầu: dữ liệu hay dùng được đặt gần người dùng (tầng "gần"), dữ liệu ít dùng nằm ở máy chủ trung tâm (tầng "xa") — chủ đề của Volume 4 |
| Bộ nhớ đệm hệ điều hành (page cache, buffer cache) | Áp dụng chính cơ chế Mục 8 giữa Main Memory và Secondary Storage — chủ đề của Part VI (Operating Systems) sắp tới |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Khoảng cách tốc độ giữa CPU và Memory (Mục 2 — "Memory Wall") không phải hiện tượng đã được giải quyết dứt điểm — nó tiếp tục là động lực cho nhiều hướng nghiên cứu công nghệ bộ nhớ mới.

Kể từ khi Wilkes đề xuất khái niệm Cache năm 1965 (Mục 2), khoảng cách tốc độ giữa CPU và Main Memory đã tiếp tục nới rộng qua nhiều thập kỷ, vì tốc độ CPU cải thiện nhanh hơn đáng kể so với tốc độ Main Memory dựa trên công nghệ DRAM. Hiện tượng này thường được gọi bằng thuật ngữ **Memory Wall**, và là lý do trực tiếp khiến Memory Hierarchy — vốn ban đầu chỉ là một cải tiến — trở thành yêu cầu gần như bắt buộc trong mọi thiết kế CPU hiện đại.

Hướng nghiên cứu hiện nay không chỉ dừng ở việc thêm nhiều tầng Cache hơn (Chương 2.27 sẽ đề cập L1/L2/L3), mà còn khám phá các công nghệ bộ nhớ mới với đặc tính khác biệt — ví dụ **bộ nhớ không mất dữ liệu (Non-Volatile Memory / Persistent Memory)** có tốc độ gần với DRAM nhưng vẫn giữ dữ liệu khi mất điện, một đặc tính vốn chỉ có ở tầng Secondary Storage truyền thống. Những công nghệ này có tiềm năng làm mờ ranh giới rõ ràng giữa các tầng trong Hình 2.26.1 — một hướng phát triển vẫn đang tiếp diễn.

**Câu hỏi mở** để suy ngẫm: nếu một công nghệ bộ nhớ mới xuất hiện, vừa nhanh như Cache, vừa có dung lượng lớn như Secondary Storage, vừa rẻ — điều đó có "phá vỡ" toàn bộ khái niệm Memory Hierarchy hay không? Hay Memory Hierarchy, xét cho cùng, vẫn sẽ tồn tại dưới một hình thức nào đó, chỉ vì lý do vật lý cơ bản hơn (ví dụ: tốc độ ánh sáng giới hạn thời gian tín hiệu di chuyển qua khoảng cách vật lý, bất kể công nghệ lưu trữ là gì)?

---

## 13. Ưu điểm

- **Tạo ra "ảo giác tốc độ"** hiệu quả về chi phí — hệ thống hoạt động gần với tốc độ của tầng nhanh nhất mà không phải trả chi phí xây dựng toàn bộ bộ nhớ ở mức tốc độ đó (đã chứng minh định lượng ở Mục 7.2 và 10).
- **Khai thác một quy luật hành vi chương trình có thật** (Locality of Reference) thay vì một giả định lý thuyết mong manh — đây là lý do Memory Hierarchy hiệu quả trong thực tế, không chỉ trên giấy.
- **Trong suốt (transparent) với phần lớn phần mềm** — lập trình viên thông thường không cần viết code quản lý tầng nào chứa dữ liệu gì; cơ chế ở Mục 8 tự động xử lý, dù hiểu nó vẫn giúp viết code hiệu quả hơn (Mục 11).

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Người mới học đôi khi nghĩ Memory Hierarchy "luôn luôn" cải thiện hiệu năng. Thực tế, lợi ích của nó **hoàn toàn phụ thuộc vào Hit Rate** — nếu chương trình có mẫu truy cập bộ nhớ gần như ngẫu nhiên (Locality kém, như minh họa ở Mục 10), Memory Hierarchy gần như không mang lại lợi ích, thậm chí có thể chậm hơn một chút do chi phí kiểm tra tầng gần trước khi xuống tầng xa (đã ẩn trong $T_{\text{xa}}$ ở Formula Box Mục 7.2).

- Memory Hierarchy chỉ hiệu quả khi Locality of Reference thực sự tồn tại trong chương trình — một số bài toán (ví dụ truy cập dữ liệu ngẫu nhiên có chủ đích, như trong một số thuật toán mật mã) vốn có Locality rất kém.
- Việc quản lý "khi nào loại bỏ dữ liệu khỏi tầng gần để nhường chỗ" (đơn giản hóa cực độ ở dòng `pop` trong Mục 9) là một bài toán phức tạp hơn nhiều trong thực tế — các chính sách thay thế (replacement policy) tinh vi hơn sẽ được trình bày đầy đủ ở Chương 2.27.
- Thêm tầng bộ nhớ đồng nghĩa với thêm độ phức tạp thiết kế phần cứng và tiêu thụ năng lượng — một trade-off khác cần cân nhắc, tương tự các trade-off đã gặp ở Chương 2.24 (Mục 15) và 2.25 (Mục 14).

---

## 15. So sánh

**Bảng 2.26.1 — Đặc điểm các tầng trong Memory Hierarchy**

| Tầng | Tốc độ | Dung lượng | Chi phí/byte | Tính bền vững (khi mất điện) |
|---|---|---|---|---|
| Register | Cực nhanh | Cực nhỏ | Cực đắt | Mất dữ liệu |
| Cache (L1/L2/L3) | Rất nhanh | Nhỏ | Đắt | Mất dữ liệu |
| Main Memory (RAM) | Vừa | Vừa | Trung bình | Mất dữ liệu |
| Secondary Storage (SSD/HDD) | Chậm | Lớn | Rẻ | **Giữ được** dữ liệu |

**Phân tích:** cột cuối cùng — tính bền vững — là một tiêu chí thường bị bỏ qua nhưng quan trọng không kém tốc độ/dung lượng: đây chính là lý do tại sao **không thể** đơn giản loại bỏ Secondary Storage dù nó chậm nhất — nó đóng vai trò duy nhất trong toàn bộ hệ thống có thể lưu trữ dữ liệu lâu dài. Mối quan hệ giữa các tầng trong Bảng 2.26.1, do đó, không chỉ là đánh đổi Tốc độ–Dung lượng–Chi phí (Mục 7.1) mà còn cả đánh đổi về **chức năng** — mỗi tầng phục vụ một mục đích không thể thay thế hoàn toàn bởi tầng khác.

---

## 16. Tóm tắt

- Không tồn tại một loại bộ nhớ "hoàn hảo" vì quan hệ đánh đổi vật lý/kinh tế giữa Tốc độ, Dung lượng, và Chi phí (Mục 7.1); **Memory Hierarchy** giải quyết vấn đề này bằng cách tổ chức nhiều tầng bộ nhớ (Hình 2.26.1: Register → Cache → Main Memory → Secondary Storage).
- **Locality of Reference** — gồm **Temporal Locality** (dùng lại dữ liệu gần đây) và **Spatial Locality** (dùng dữ liệu ở gần dữ liệu vừa dùng) — là quy luật hành vi chương trình khiến Memory Hierarchy hiệu quả trong thực tế, không chỉ lý thuyết.
- Công thức $T_{\text{avg}} = H \times T_{\text{gần}} + (1-H) \times T_{\text{xa}}$ định lượng hóa "ảo giác tốc độ": Hit Rate càng cao, hệ thống càng gần với tốc độ tầng nhanh nhất.
- Cơ chế chung "kiểm tra tầng gần trước, sao chép dữ liệu (và lân cận) khi Miss" (Mục 8) là nền tảng trực tiếp cho **Cache Memory**, chủ đề của Chương 2.27.

Chương 2.27 sẽ phóng to đúng cặp tầng quan trọng nhất về mặt hiệu năng — **Cache và Main Memory** — với các cơ chế cụ thể: Cache Mapping, chính sách thay thế, và công thức AMAT đầy đủ, mở rộng trực tiếp từ Formula Box Mục 7.2.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Sắp xếp lại bốn tầng của Memory Hierarchy (Register, Cache, Main Memory, Secondary Storage) theo thứ tự **tăng dần dung lượng** — kết quả có giống hệt thứ tự **giảm dần tốc độ** trong Bảng 2.26.1 không? Giải thích tại sao đây không phải sự trùng hợp.
2. Cho đoạn code ở Hình 2.26.2. Chỉ rõ dòng nào minh họa Temporal Locality, dòng nào minh họa Spatial Locality, và giải thích bằng lời của riêng bạn.

### Mức Trung bình (Intermediate)

3. Áp dụng Formula Box Mục 7.2: với $T_{\text{gần}} = 1$ ns, $T_{\text{xa}} = 80$ ns, tính $T_{\text{avg}}$ khi $H = 0.99$ và khi $H = 0.5$. Nhận xét về mức độ nhạy cảm của $T_{\text{avg}}$ đối với sự thay đổi của $H$.
4. Chạy (hoặc trace bằng tay) `TwoLevelMemory` ở Mục 9 với `near_capacity=2` và mẫu truy cập `[0, 1, 2, 0, 1, 2]`. Liệt kê Hit/Miss cho từng lần truy cập, biết chính sách thay thế đơn giản trong code là loại bỏ mục đầu tiên được thêm vào khi đầy.

### Mức Nâng cao (Advanced)

5. Trong Mục 9, chính sách thay thế khi tầng gần đầy được đơn giản hóa thành "loại bỏ một mục bất kỳ" (`next(iter(...))`). Hãy đề xuất một chính sách thay thế tốt hơn, dựa trực tiếp trên Temporal Locality: nếu phải loại bỏ một mục, nên ưu tiên loại bỏ mục nào? Giải thích lý do lựa chọn của bạn bằng lập luận, chưa cần viết code hoàn chỉnh (chính sách cụ thể — LRU — sẽ được định nghĩa hình thức ở Chương 2.27).

### Mức Nghiên cứu (Research)

6. Mục 12 đặt câu hỏi mở về việc liệu công nghệ bộ nhớ mới (nhanh, lớn, rẻ đồng thời) có "phá vỡ" khái niệm Memory Hierarchy hay không. Hãy phát triển lập luận riêng của bạn: bạn nghiêng về khả năng nào, và dựa trên lý do vật lý/kỹ thuật/kinh tế nào? Đây là câu hỏi mở tính chất khám phá.

---

## 18. Dự án nhỏ

**Dự án: So sánh Hit Rate giữa nhiều mẫu truy cập khác nhau**

- **Mục tiêu:** củng cố trực giác về Locality of Reference bằng thực nghiệm định lượng, mở rộng trực tiếp Mục 9–10.
- **Yêu cầu:**
  1. Dùng lại class `TwoLevelMemory` ở Mục 9.
  2. Thiết kế **ba** mẫu truy cập khác nhau: (a) tuần tự có Spatial Locality cao (`0,1,2,3,4,...`), (b) lặp lại một nhóm nhỏ nhiều lần (Temporal Locality cao, giống Mục 10), (c) hoàn toàn ngẫu nhiên.
  3. Với mỗi mẫu, chạy qua `TwoLevelMemory` với `near_capacity` thử ở ba giá trị khác nhau (ví dụ 2, 8, 32), ghi lại Hit Rate cho mỗi tổ hợp.
  4. Trình bày kết quả dưới dạng bảng 3×3 (mẫu truy cập × dung lượng tầng gần).
- **Công nghệ đề xuất:** Python thuần.
- **Mở rộng (tùy chọn):** vẽ biểu đồ (dùng `matplotlib`, đã liệt kê ở `TOOLS.md`) thể hiện Hit Rate thay đổi theo `near_capacity` cho từng mẫu truy cập.

---

## 19. Tự đánh giá

- [ ] Tôi có thể vẽ lại Hình 2.26.1 và giải thích tại sao càng lên cao (nhanh hơn), dung lượng càng nhỏ, dựa trên lý do kinh tế/vật lý ở Mục 7.1.
- [ ] Tôi có thể phân biệt rõ ràng Temporal Locality và Spatial Locality, kèm ví dụ code cụ thể cho mỗi loại.
- [ ] Tôi có thể áp dụng đúng Formula Box $T_{\text{avg}} = H \times T_{\text{gần}} + (1-H) \times T_{\text{xa}}$ để tính toán và giải thích kết quả bằng lời.
- [ ] Tôi hiểu cơ chế chung "kiểm tra gần trước, xuống xa khi cần, rồi sao chép lên gần" (Mục 8) đủ rõ để dự đoán nó sẽ hoạt động ra sao ở một cặp tầng bất kỳ trong Hình 2.26.1.
- [ ] Tôi có thể giải thích, có số liệu cụ thể, lý do đầy đủ cho hiện tượng duyệt ma trận đã nêu ở Chương 2.22, Mục 3.

Nếu Bài tập 5 (đề xuất chính sách thay thế) vẫn còn mơ hồ, đây là dấu hiệu tốt — nó cho thấy bạn đã sẵn sàng cho Chương 2.27, nơi câu hỏi này sẽ được trả lời hình thức và đầy đủ bằng khái niệm **LRU (Least Recently Used)**.

---

## 20. Đọc thêm

- **Sách:** Randal E. Bryant, David R. O'Hallaron, *Computer Systems: A Programmer's Perspective* — chương về Memory Hierarchy, trình bày rất chi tiết cách tối ưu code để khai thác Locality of Reference trong thực tế. *(Xem BOOKS.md.)*
- **Sách:** Andrew S. Tanenbaum, *Modern Operating Systems* — phần Virtual Memory, mở rộng khái niệm phân tầng sang mối quan hệ giữa Main Memory và Secondary Storage, chủ đề của Part VI sắp tới. *(Xem BOOKS.md.)*
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc về công nghệ **Persistent Memory** (ví dụ Intel Optane, dù đã ngừng phát triển thương mại, vẫn là ví dụ kinh điển để hiểu nỗ lực làm mờ ranh giới các tầng) — liên hệ trực tiếp với câu hỏi mở ở Mục 12.
- **Chương tiếp theo:** Chương 2.27 — Cache Memory.

---

### Liên kết chương (Cross References)

- **Chương trước:** 2.25 — Pipelining: Nhập môn (giai đoạn MEM trong Pipeline chính là nơi cơ chế Memory Hierarchy của chương này được kích hoạt trong thực tế).
- **Chương tiếp theo:** 2.27 — Cache Memory (mở rộng trực tiếp Formula Box Mục 7.2 thành AMAT đầy đủ; hình thức hóa chính sách thay thế đã gợi mở ở Bài tập 5 thành LRU).
- **Chương liên quan xa hơn:** Chương 2.22, Mục 3 và Mục 14 (Von Neumann Bottleneck, ví dụ duyệt ma trận — cả hai được giải thích đầy đủ, có số liệu, ở chương này); Volume 2, Part VI — Operating Systems (Virtual Memory, page cache áp dụng trực tiếp nguyên lý Memory Hierarchy giữa Main Memory và Secondary Storage); Volume 4 (CDN trong hệ thống phân tán — Memory Hierarchy ở quy mô toàn cầu).
- **Vị trí trong Knowledge Graph:** Nút thứ năm của Volume 2, Part V; phụ thuộc vào Chương 2.22 và 2.25; là điều kiện tiên quyết trực tiếp, bắt buộc cho Chương 2.27.

---

*Hết Chương 2.26. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`, khớp phong cách trình bày đã thiết lập từ Chương 1.5, 2.22, 2.23, 2.24, và 2.25. Công thức $T_{\text{avg}}$ ở Mục 7.2 được thiết kế có chủ đích như tiền đề trực tiếp cho AMAT sẽ trình bày đầy đủ ở Chương 2.27, tránh trùng lặp nội dung giữa hai chương. Đang chờ rà soát trước khi tiếp tục sang Chương 2.27.*
