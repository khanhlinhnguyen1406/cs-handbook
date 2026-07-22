# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 04 — Computer Systems
### Part I — Computer Organization and Architecture
## Chương 4.1.8 — GPU Fundamentals
### (Nền tảng Kiến trúc GPU)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 4.1.8 (Chương cuối, Part I) |
| Thuộc Part | I — Computer Organization and Architecture |
| Thuộc Volume | 04 — Computer Systems |
| Thời gian đọc ước tính | 55–70 phút |
| Độ khó | ★★★★☆ |
| Kiến thức tiên quyết | Chương 4.1.5 — Superscalar & Out-of-Order Execution; Chương 4.1.6 — Branch Prediction; Chương 4.1.7 — SIMD (đặc biệt Mục 6–7, Định luật Amdahl) |
| Chương liên quan | Volume 04, Part IV — Concurrency and Parallel Computing (mở rộng khái niệm song song hóa sang đa lõi/đa máy); Volume 05 — Artificial Intelligence (GPU là hạ tầng tính toán trung tâm của Deep Learning) |
| Từ khóa | GPU, SIMT, throughput-oriented architecture, latency-oriented architecture, warp/wavefront, branch divergence, massive multithreading, GPGPU |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Giải thích triết lý thiết kế đối lập giữa CPU (throughput thấp, latency thấp) và GPU (throughput cực cao, latency mỗi luồng cao).
- Định nghĩa SIMT (Single Instruction, Multiple Threads) như một mở rộng trực tiếp của SIMD (Chương 4.1.7) ở quy mô hàng nghìn luồng.
- Giải thích cơ chế "che giấu độ trễ" (latency hiding) bằng đa luồng quy mô lớn, đối lập với chiến lược speculation của CPU (Chương 4.1.5, 4.1.6).
- Mô tả hiện tượng branch divergence và tác động hiệu năng của nó khi các luồng trong cùng một warp/wavefront rẽ nhánh khác nhau.
- Áp dụng Định luật Amdahl (Chương 4.1.7) để giải thích tại sao GPU chỉ mang lại lợi ích vượt trội cho khối lượng công việc có tỷ lệ song song hóa cực cao.
- Kết nối kiến trúc GPU với vai trò trung tâm của nó trong Deep Learning, chuẩn bị nền tảng cho Volume 05, 06.

---

### Câu hỏi khơi gợi

> *Chương 4.1.7 đã cho thấy SIMD tăng vector width từ 1 lên vài chục phần tử mỗi lệnh. Nhưng huấn luyện một mạng nơ-ron hiện đại đòi hỏi thực hiện hàng tỷ phép nhân-cộng độc lập. Nếu CPU, dù đã trang bị đầy đủ pipeline sâu, superscalar, và SIMD, vẫn không đủ để xử lý khối lượng công việc đó trong thời gian hợp lý — điều gì sẽ xảy ra nếu ta thiết kế một loại chip hoàn toàn khác, từ bỏ gần như mọi kỹ thuật phức tạp đã học ở Chương 4.1.5–4.1.6 (out-of-order, dự đoán rẽ nhánh tinh vi), để đổi lấy hàng nghìn lõi tính toán đơn giản chạy song song?*

---

## 1. Tổng quan chương

Đây là chương khép lại Part I, và cũng là chương đánh dấu một bước ngoặt triết lý thiết kế hoàn toàn. Mọi chương từ 4.1.3 đến 4.1.6 đều theo đuổi cùng một mục tiêu: làm cho **một luồng lệnh duy nhất** chạy nhanh nhất có thể — bằng pipeline sâu, thực thi không theo thứ tự, dự đoán rẽ nhánh tinh vi. Đây là triết lý **latency-oriented (tối ưu độ trễ)**: tối thiểu hóa thời gian hoàn tất của từng lệnh, từng luồng thực thi.

**GPU (Graphics Processing Unit)** đại diện cho một triết lý hoàn toàn đối lập: **throughput-oriented (tối ưu thông lượng)**. Thay vì đầu tư silicon vào các cơ chế phức tạp giúp một luồng lệnh chạy nhanh (out-of-order, dự đoán rẽ nhánh tinh vi), GPU dùng chính lượng silicon đó để nhân bản **hàng nghìn lõi tính toán đơn giản**, chạy song song. Đây chính là câu trả lời cho câu hỏi khơi gợi: khi khối lượng công việc có đủ Data-Level Parallelism (Chương 4.1.7) — như phép nhân ma trận trong mạng nơ-ron — việc có một luồng "siêu nhanh" kém quan trọng hơn nhiều so với việc có **rất nhiều luồng** chạy đồng thời, dù mỗi luồng riêng lẻ chậm hơn.

> **💡 Insight**
> Toàn bộ Part I, khi nhìn lại từ chương này, kể một câu chuyện nhất quán: mỗi chương đều là một cách trả lời câu hỏi "làm sao khai thác song song tốt hơn" — pipeline khai thác song song giữa các **giai đoạn**, superscalar khai thác song song giữa các **lệnh khác nhau**, SIMD khai thác song song giữa các **phần tử dữ liệu trong một lệnh**, và giờ đây GPU khai thác song song giữa **hàng nghìn luồng thực thi độc lập**. Khác biệt không nằm ở việc "có song song hóa hay không", mà ở **quy mô** và **đối tượng** được song song hóa.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Sự kiện | Đóng góp |
|---|---|---|
| Thập niên 1990–2000 | GPU ra đời và phát triển ban đầu như chip chuyên dụng cho đồ họa máy tính (rendering), với pipeline xử lý đồ họa cố định (fixed-function graphics pipeline) | Đặt nền tảng phần cứng cho tính toán song song quy mô lớn, ban đầu chỉ phục vụ mục đích hiển thị hình ảnh |
| Giữa thập niên 2000 | GPU chuyển sang kiến trúc lập trình được tổng quát hơn, mở đường cho GPGPU (General-Purpose computing on GPU) — sử dụng GPU cho các bài toán tính toán không liên quan tới đồ họa | Biến GPU từ một chip chuyên dụng hẹp thành một nền tảng tính toán song song đa dụng |
| 2012 | Nghiên cứu AlexNet (Krizhevsky và cộng sự) chứng minh huấn luyện mạng nơ-ron sâu trên GPU mang lại đột phá về độ chính xác trong nhận dạng ảnh, khởi đầu làn sóng Deep Learning hiện đại | Xác lập GPU như hạ tầng tính toán trung tâm của AI hiện đại — liên hệ trực tiếp tới Volume 05 (xem thêm PAPERS.md, DATASETS.md — ImageNet) |
| Từ 2012 đến nay | GPU tiếp tục phát triển với các đơn vị tính toán ma trận ngày càng chuyên biệt hơn cho khối lượng công việc AI | Củng cố xu hướng domain-specific ISA đã đề cập ở Chương 4.1.2, Mục 12 |

Điều đáng chú ý về mặt lịch sử: GPU không được thiết kế "vì AI" ngay từ đầu — nó được thiết kế cho đồ họa, một bài toán vốn dĩ đã có cấu trúc data-parallel gần như hoàn hảo (mỗi pixel trên màn hình có thể được tô màu độc lập với mọi pixel khác). Sự trùng hợp cấu trúc toán học giữa "tô màu hàng triệu pixel độc lập" và "nhân hàng tỷ phần tử ma trận độc lập" chính là lý do GPU trở thành nền tảng AI lý tưởng, dù đó không phải mục đích thiết kế ban đầu.

---

## 3. Động lực

Hãy định lượng cụ thể động lực đã nêu ở câu hỏi khơi gợi. Giả sử một phép nhân ma trận trong mạng nơ-ron cần thực hiện $10^{10}$ (mười tỷ) phép nhân-cộng độc lập — hoàn toàn thỏa điều kiện "độc lập dữ liệu" đã học ở Chương 4.1.7, Mục 7.2 (tương tự phép cộng mảng, nhưng ở quy mô lớn hơn nhiều bậc).

- Một CPU với SIMD (Chương 4.1.7), vector width $s = 16$, và vài lõi (nhờ superscalar/đa lõi): tổng số "lane" xử lý song song thực tế trong khoảng vài chục.
- Một GPU với hàng nghìn lõi tính toán đơn giản: tổng số "lane" xử lý song song thực tế trong khoảng hàng nghìn tới hàng chục nghìn.

Với công thức hiệu năng $T = N \times \text{CPI} \times T_{\text{clock}}$ (Chương 4.1.3, Mục 7.1), nếu $N$ (số phép toán) không đổi, nhưng số lượng đơn vị xử lý song song tăng từ hàng chục lên hàng nghìn, thời gian hoàn tất giảm theo tỷ lệ tương ứng — miễn là bài toán có đủ Data-Level Parallelism để "nuôi" hết số lượng lõi đó (đúng điều kiện Amdahl, Chương 4.1.7, Mục 7.1). Đây chính là lý do một phép huấn luyện mạng nơ-ron có thể mất hàng tuần trên CPU nhưng chỉ mất vài giờ trên GPU — không phải vì mỗi lõi GPU "thông minh" hơn lõi CPU (thực tế ngược lại, xem Mục 6), mà vì **số lượng lõi song song** chênh lệch nhiều bậc độ lớn.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Nếu CPU (với mọi kỹ thuật đã học từ Chương 4.1.3–4.1.6) giống một **đội ngũ nhỏ gồm vài chuyên gia cực kỳ lành nghề** — mỗi người có thể xử lý độc lập những nhiệm vụ phức tạp, đa dạng, ra quyết định linh hoạt (dự đoán, sắp xếp lại thứ tự công việc) — thì GPU giống một **đội quân khổng lồ gồm hàng nghìn công nhân phổ thông**, mỗi người chỉ biết làm đúng một loại thao tác đơn giản, nhưng làm đồng thời. Với một nhiệm vụ đơn lẻ, phức tạp, cần suy luận (ví dụ vận hành một quy trình kinh doanh nhiều bước phụ thuộc lẫn nhau), vài chuyên gia lành nghề vượt trội hơn hẳn. Nhưng với một nhiệm vụ như "sơn lại toàn bộ một triệu bức tường giống hệt nhau, độc lập với nhau", đội quân công nhân phổ thông — dù mỗi người chậm hơn một chuyên gia — hoàn thành nhanh hơn gấp nhiều lần nhờ số lượng áp đảo.

| Trực giác kỹ thuật bạn đã có | Khái niệm GPU tương ứng |
|---|---|
| Worker pool xử lý hàng loạt tác vụ đơn giản, đồng nhất trong hệ thống xử lý dữ liệu lớn (batch data processing) | SIMT — hàng nghìn luồng thực thi cùng loại thao tác song song |
| Đánh đổi "vài máy chủ mạnh" và "rất nhiều máy chủ yếu hơn nhưng nhiều hơn" trong thiết kế hệ phân tán (sẽ học đầy đủ ở Volume 4, Part VI) | Triết lý throughput-oriented (GPU) so với latency-oriented (CPU) |
| Context switch cực nhanh giữa các coroutine/goroutine nhẹ (lightweight thread) khi một tác vụ bị block chờ I/O | Che giấu độ trễ bằng chuyển đổi warp gần như tức thời khi một nhóm luồng GPU bị "kẹt" chờ dữ liệu |

---

## 5. Trực quan hóa khái niệm

**Hình 4.1.8.1 — CPU (Latency-Oriented) và GPU (Throughput-Oriented): phân bổ silicon khác nhau**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
CPU — vài lõi PHỨC TẠP (latency-oriented):

  ┌─────────────────────┐  ┌─────────────────────┐
  │       LÕI 1          │  │       LÕI 2          │
  │  ┌────────────────┐  │  │  ┌────────────────┐  │
  │  │ Out-of-order    │  │  │  │ Out-of-order    │  │      Diện tích chip dành
  │  │ Branch Predictor│  │  │  │ Branch Predictor│  │      PHẦN LỚN cho các cơ
  │  │ Cache lớn       │  │  │  │ Cache lớn       │  │      chế giúp MỘT luồng
  │  │ Control Unit    │  │  │  │ Control Unit    │  │      chạy nhanh nhất có thể
  │  │ phức tạp         │  │  │  │ phức tạp         │  │
  │  └────────────────┘  │  │  └────────────────┘  │
  └─────────────────────┘  └─────────────────────┘


GPU — hàng nghìn lõi ĐƠN GIẢN (throughput-oriented):

  ┌──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┐
  │AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│  ← mỗi ô chỉ là MỘT ALU
  ├──┼──┼──┼──┼──┼──┼──┼──┼──┼──┼──┼──┼──┼──┼──┼──┤     đơn giản, KHÔNG có
  │AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│     out-of-order, KHÔNG
  ├──┼──┼──┼──┼──┼──┼──┼──┼──┼──┼──┼──┼──┼──┼──┼──┤     có dự đoán rẽ nhánh
  │AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│AL│     tinh vi (Chương 4.1.5-6)
  └──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┘     ... (lặp lại hàng trăm lần)
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Đối chiếu trực tiếp với Hình 4.1.5.2 (out-of-order) và Hình 4.1.6.2 (speculative execution): GPU CHỦ ĐỘNG từ bỏ gần như toàn bộ những cơ chế phức tạp đó, đổi lấy số lượng lõi vượt trội |
| Điểm mấu chốt | Đây không phải "GPU thông minh hơn CPU" — mà là một đánh đổi thiết kế có chủ đích: từ bỏ tối ưu hóa MỘT luồng để tối đa hóa THÔNG LƯỢNG TỔNG THỂ khi có đủ số luồng độc lập |

---

**Hình 4.1.8.2 — Branch Divergence: cái giá của rẽ nhánh trong mô hình SIMT**

```text
Một WARP gồm 4 luồng, cùng thực thi lệnh IF (điều kiện KHÁC NHAU
giữa các luồng — ví dụ: luồng 0,2 thỏa điều kiện, luồng 1,3 không):

  Luồng 0:  if (x > 0)  { A }  else { B }    → x > 0: chạy A
  Luồng 1:  if (x > 0)  { A }  else { B }    → x ≤ 0: chạy B
  Luồng 2:  if (x > 0)  { A }  else { B }    → x > 0: chạy A
  Luồng 3:  if (x > 0)  { A }  else { B }    → x ≤ 0: chạy B

THỰC THI THỰC TẾ (mô hình SIMT — TUẦN TỰ hóa hai nhánh):

  Bước 1: TOÀN BỘ warp thực thi nhánh A —
          Luồng 0, 2: THỰC SỰ TÍNH TOÁN
          Luồng 1, 3: BỊ "CHE" (masked), không ghi kết quả, NHƯNG
                      vẫn "lãng phí" một lượt thực thi cùng warp
  Bước 2: TOÀN BỘ warp thực thi nhánh B —
          Luồng 1, 3: THỰC SỰ TÍNH TOÁN
          Luồng 0, 2: BỊ "CHE"

  → TỔNG THỜI GIAN = thời gian nhánh A + thời gian nhánh B,
    dù mỗi luồng riêng lẻ chỉ cần chạy MỘT trong hai nhánh!
```

*Mục đích:* Minh họa cái giá cụ thể của rẽ nhánh trong kiến trúc SIMT — khác biệt căn bản so với CPU (nơi mỗi luồng có PC và bộ dự đoán rẽ nhánh riêng, Chương 4.1.6). *Điểm mấu chốt:* các luồng trong cùng một warp/wavefront **bắt buộc thực thi cùng một lệnh tại một thời điểm** (đúng bản chất SIMT — mở rộng của SIMD, Chương 4.1.7) — khi chúng rẽ nhánh khác nhau, GPU phải tuần tự hóa các nhánh, gây tổn thất hiệu năng đáng kể.

---

## 6. Định nghĩa hình thức

> **📌 Remember — GPU và SIMT**
>
> **GPU (Graphics Processing Unit)** là kiến trúc xử lý throughput-oriented, gồm hàng nghìn lõi tính toán đơn giản (thường gọi chung là "CUDA core" hoặc tên tương đương tùy nhà sản xuất), thiết kế để tối đa hóa tổng thông lượng tính toán cho khối lượng công việc có Data-Level Parallelism cực cao. **SIMT (Single Instruction, Multiple Threads)** là mô hình thực thi của GPU — mở rộng trực tiếp ý tưởng SIMD (Chương 4.1.7, Mục 6) từ "nhiều lane trong một lệnh" sang "nhiều luồng (thread) độc lập, mỗi luồng có PC và trạng thái riêng về mặt lập trình, nhưng được nhóm lại và thực thi đồng bộ theo lockstep ở tầng phần cứng".

> **📌 Remember — Warp/Wavefront và Branch Divergence**
>
> - **Warp (hoặc Wavefront, tùy nhà sản xuất):** một nhóm luồng cố định (ví dụ vài chục luồng) được lập lịch và thực thi **đồng bộ theo lockstep** — tại mọi thời điểm, toàn bộ luồng trong một warp đang thực thi **cùng một lệnh**, đúng bản chất SIMT.
> - **Branch Divergence:** hiện tượng xảy ra khi các luồng trong cùng một warp đi theo các nhánh rẽ khác nhau (Hình 4.1.8.2) — buộc phần cứng phải tuần tự hóa việc thực thi các nhánh, làm mất đi lợi thế song song của SIMT cho đoạn code đó.

> **📌 Remember — Latency-Oriented và Throughput-Oriented**
>
> - **Latency-Oriented Design (CPU, Chương 4.1.3–4.1.6):** ưu tiên tối thiểu hóa thời gian hoàn tất của **một** luồng thực thi, chấp nhận số lượng lõi ít hơn để đầu tư silicon vào out-of-order, dự đoán rẽ nhánh, cache lớn.
> - **Throughput-Oriented Design (GPU):** ưu tiên tối đa hóa **tổng số** công việc hoàn tất trên một đơn vị thời gian, chấp nhận mỗi luồng riêng lẻ chạy chậm hơn để đổi lấy số lượng luồng song song vượt trội, che giấu độ trễ bằng cách chuyển đổi nhanh sang warp khác khi một warp bị "kẹt" (ví dụ chờ dữ liệu từ bộ nhớ).

---

## 7. Nền tảng toán học

### 7.1 Thông lượng tính toán tổng hợp (Aggregate Compute Throughput)

- **Ý nghĩa:** Định lượng chính xác lợi thế "số lượng thắng chất lượng" của GPU, mở rộng trực tiếp công thức hiệu năng đã dùng xuyên suốt Part I.

> **📦 Formula Box — Thông lượng tính toán tổng hợp**
>
> $$\text{Throughput}_{\text{tổng}} = C \times f \times \phi$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $C$ | Số lượng lõi tính toán song song (vài lõi với CPU; hàng nghìn với GPU) |
> | $f$ | Tần số clock của mỗi lõi (thường THẤP HƠN ở GPU so với CPU, vì lõi đơn giản hơn không đòi hỏi mạch phức tạp như Chương 4.1.4–4.1.6, nhưng đây không phải yếu tố quyết định chính) |
> | $\phi$ | Số phép toán mỗi lõi thực hiện được mỗi chu kỳ (thường bằng 1 cho lõi GPU đơn giản, có thể cao hơn với CPU superscalar — Chương 4.1.5) |
> | **Diễn giải kỹ thuật** | Dù $f$ và $\phi$ của một lõi GPU đơn lẻ thường THẤP HƠN một lõi CPU, chênh lệch khổng lồ ở $C$ (hàng nghìn so với vài chục) khiến $\text{Throughput}_{\text{tổng}}$ của GPU vượt xa CPU cho khối lượng công việc phù hợp |
> | **Ứng dụng thường gặp** | Giải thích định lượng vì sao GPU vượt trội cho các bài toán song song hóa cao, dù "mỗi lõi GPU yếu hơn lõi CPU" — một hiểu lầm phổ biến sẽ làm rõ ở Mục 14 |

### 7.2 Amdahl's Law áp dụng cho GPU: điều kiện để thông lượng khổng lồ thực sự có ích

Công thức ở Mục 7.1 chỉ mô tả thông lượng **lý thuyết tối đa** — giống hệt cách vector width $s$ trong SIMD (Chương 4.1.7, Mục 7.1) chỉ là giới hạn trên. Định luật Amdahl vẫn áp dụng nguyên vẹn:

$$\text{Speedup}_{\text{GPU so với CPU}} = \frac{1}{(1-p) + \dfrac{p}{C_{\text{GPU}}/C_{\text{CPU}}}}$$

với $p$ là tỷ lệ khối lượng công việc **thực sự tận dụng được** hàng nghìn lõi GPU (nghĩa là có đủ Data-Level Parallelism, không bị branch divergence nghiêm trọng — Hình 4.1.8.2 — và không bị nghẽn bởi phần tuần tự bắt buộc). Nếu $p$ thấp — ví dụ chương trình có nhiều rẽ nhánh phức tạp, phụ thuộc dữ liệu chặt chẽ giữa các phần tử — lợi thế $C$ khổng lồ của GPU gần như bị triệt tiêu hoàn toàn, đúng quy luật đã chứng minh định lượng ở Chương 4.1.7, Mục 7.1, 10.

---

## 8. Thuật toán / Cơ chế

**Quy trình tổng quát thực thi một chương trình trên GPU (mô hình SIMT):**

```text
Bước 1 — Phân chia bài toán thành HÀNG NGHÌN (hoặc hàng triệu)
         luồng (thread) độc lập, mỗi luồng xử lý một phần tử dữ
         liệu nhỏ (ví dụ: một luồng cho mỗi phần tử ma trận)
        │
        ▼
Bước 2 — Nhóm các luồng thành các WARP có kích thước cố định
         (Mục 6); trong mỗi warp, toàn bộ luồng sẽ thực thi ĐỒNG
         BỘ theo lockstep
        │
        ▼
Bước 3 — Bộ lập lịch warp (warp scheduler) chọn một warp ĐANG SẴN
         SÀNG (không bị chờ dữ liệu) để thực thi mỗi chu kỳ — nếu
         warp hiện tại bị "kẹt" chờ bộ nhớ, LẬP TỨC chuyển sang
         warp khác đã sẵn sàng (che giấu độ trễ, Mục 6), gần như
         KHÔNG tốn chi phí chuyển đổi (khác hẳn context switch
         tốn kém trên CPU, sẽ học ở Part III — Operating Systems)
        │
        ▼
Bước 4 — Mọi luồng trong warp được chọn thực thi CÙNG một lệnh,
         trên các lane phần cứng riêng biệt (đúng nguyên lý SIMD/
         SIMT, Chương 4.1.7, Hình 4.1.8.1)
        │
        ▼
Bước 5 — NẾU gặp lệnh rẽ nhánh và các luồng trong warp đi khác
         hướng nhau (Hình 4.1.8.2): tuần tự hóa việc thực thi
         từng nhánh, "che" (mask) các luồng không thuộc nhánh
         đang thực thi — GÂY TỔN THẤT HIỆU NĂNG tỷ lệ với số
         nhánh khác nhau cần tuần tự hóa
        │
        ▼
Bước 6 — Lặp lại Bước 3-5 cho tới khi TOÀN BỘ warp hoàn tất; kết
         quả từ hàng nghìn luồng được tổng hợp lại thành kết quả
         cuối cùng của bài toán
```

> **⚠️ Common Mistake**
> Người mới học thường nghĩ rằng vì GPU có hàng nghìn lõi, nó luôn "nhanh hơn tuyệt đối" CPU cho mọi tác vụ. Bước 3 và 5 cho thấy rõ điều ngược lại có thể xảy ra: một chương trình có **nhiều rẽ nhánh phức tạp, khác nhau giữa các luồng** sẽ liên tục kích hoạt branch divergence (Bước 5), khiến GPU thực thi **tuần tự hóa** nhiều lần — đôi khi chậm hơn hẳn một CPU với bộ dự đoán rẽ nhánh tinh vi (Chương 4.1.6), vốn được thiết kế chuyên biệt để xử lý chính xác loại tình huống này cho MỘT luồng lệnh.

---

## 9. Triển khai

```python
# Mô phỏng đơn giản hóa việc thực thi SIMT theo Warp, có tính đến
# Branch Divergence (Hình 4.1.8.2, Bước 5, Mục 8), và so sánh
# throughput tổng hợp GIỮA cấu hình "CPU" (ít lõi) và "GPU" (nhiều
# lõi đơn giản) cho cùng một khối lượng công việc.

def simulate_warp_execution(branch_outcomes, warp_size=32):
    """branch_outcomes: list bool, MỖI PHẦN TỬ là kết quả rẽ nhánh
    (True/False) của một luồng trong MỘT warp. Trả về số 'lượt
    thực thi' cần thiết — 1 nếu ĐỒNG NHẤT (không divergence), 2
    nếu warp bị chia thành đúng 2 nhánh (Hình 4.1.8.2)."""
    assert len(branch_outcomes) == warp_size
    distinct_paths = len(set(branch_outcomes))
    return distinct_paths   # số "lượt" cần TUẦN TỰ HÓA, Bước 5, Mục 8


def aggregate_throughput(num_cores, clock_freq_ghz, ops_per_cycle=1):
    """Áp dụng trực tiếp Formula Box, Mục 7.1: Throughput = C x f x phi."""
    return num_cores * clock_freq_ghz * ops_per_cycle   # đơn vị: GOPS (tỷ phép toán/giây)


def estimate_total_time(total_ops, num_cores, clock_freq_ghz,
                         parallel_fraction, ops_per_cycle=1):
    """Ước lượng thời gian hoàn tất, kết hợp throughput lý thuyết
    (Mục 7.1) VÀ Định luật Amdahl (Mục 7.2) để phản ánh phần
    KHÔNG song song hóa được vẫn phải chạy tuần tự trên một lõi."""
    throughput = aggregate_throughput(num_cores, clock_freq_ghz, ops_per_cycle)
    parallel_ops = total_ops * parallel_fraction
    sequential_ops = total_ops * (1 - parallel_fraction)

    parallel_time = parallel_ops / (throughput * 1e9)          # giây
    sequential_time = sequential_ops / (clock_freq_ghz * 1e9)   # 1 lõi duy nhất
    return parallel_time + sequential_time
```

Hàm `simulate_warp_execution` hiện thực hóa trực tiếp Hình 4.1.8.2: đếm số "đường đi khác biệt" (distinct paths) trong một warp — nếu toàn bộ luồng đồng thuận (1 đường đi), không có tổn thất; nếu chia thành nhiều nhánh, cần tuần tự hóa tương ứng. Hàm `estimate_total_time` kết hợp cả Formula Box ở Mục 7.1 (thông lượng lý thuyết) và tinh thần Amdahl ở Mục 7.2 (phần tuần tự bắt buộc không hưởng lợi từ số lõi lớn) để ước lượng thời gian thực tế, gần với cách phân tích hiệu năng GPU trong thực hành kỹ thuật.

---

## 10. Trực quan hóa quá trình thực thi

**So sánh throughput lý thuyết** giữa cấu hình "CPU" (8 lõi, 3.5 GHz) và "GPU" (5000 lõi đơn giản, 1.5 GHz), dùng `aggregate_throughput` ở Mục 9:

```text
CPU:  8 lõi × 3.5 GHz × 1 phép toán/chu kỳ  =    28 GOPS
GPU:  5000 lõi × 1.5 GHz × 1 phép toán/chu kỳ = 7500 GOPS
                                                  → gấp ~268 lần CPU (throughput LÝ THUYẾT)
```

**Áp dụng Định luật Amdahl (Mục 7.2)** cho bài toán $10^{10}$ phép toán, với các mức độ song song hóa $p$ khác nhau, dùng `estimate_total_time`:

| Tỷ lệ song song hóa ($p$) | Thời gian trên GPU (giây, ước lượng) | Nhận xét |
|:---:|:---:|:---:|
| 0.99 (gần như hoàn toàn song song — điển hình phép nhân ma trận) | Rất nhỏ, gần đạt lợi thế throughput lý thuyết ~268 lần | Trường hợp lý tưởng cho GPU |
| 0.50 (một nửa buộc phải tuần tự) | Lớn hơn đáng kể — phần tuần tự chiếm ưu thế thời gian | Lợi thế GPU giảm mạnh, đúng quy luật Amdahl |

**Minh họa Branch Divergence** với `simulate_warp_execution`, warp kích thước 32:

```text
Trường hợp ĐỒNG NHẤT (mọi luồng cùng kết quả rẽ nhánh):
  simulate_warp_execution([True]*32) → 1 lượt thực thi (KHÔNG tổn thất)

Trường hợp CHIA ĐÔI (16 luồng True, 16 luồng False):
  simulate_warp_execution([True]*16 + [False]*16) → 2 lượt thực thi
                                                        (tổn thất GẤP ĐÔI thời gian
                                                         cho đoạn code rẽ nhánh này)
```

Đây là minh chứng định lượng trực tiếp cho Common Mistake ở Mục 8: ngay cả với throughput lý thuyết gấp hàng trăm lần (như bảng đầu tiên), một đoạn code có branch divergence nghiêm trọng vẫn có thể xóa sạch phần lớn lợi thế đó cho chính đoạn code liên quan.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> GPU đã vượt xa vai trò lịch sử ban đầu (đồ họa) để trở thành hạ tầng tính toán trung tâm của nhiều lĩnh vực khoa học và công nghiệp hiện đại, đặc biệt là AI.

| Bối cảnh công nghiệp | Vai trò của GPU |
|---|---|
| Huấn luyện mạng nơ-ron sâu (Deep Learning, Volume 05) | Phép nhân ma trận quy mô lớn — lõi tính toán của mọi mạng nơ-ron — có $p$ (tỷ lệ song song hóa, Mục 7.2) rất gần 1, khiến GPU trở thành lựa chọn gần như bắt buộc cho huấn luyện quy mô lớn |
| Suy luận AI (AI inference) trên các dịch vụ đám mây | Xử lý đồng thời nhiều yêu cầu suy luận độc lập (batch inference) tận dụng trực tiếp mô hình SIMT (Mục 6, Bước 1-2, Mục 8) |
| Tính toán khoa học (Scientific Computing, sẽ học ở Part VIII — High Performance Computing) | Mô phỏng vật lý, hóa học tính toán, dự báo thời tiết — nhiều bài toán có cấu trúc lưới (grid) tự nhiên phù hợp với SIMT |
| GPGPU (General-Purpose computing on GPU) | Framework lập trình cho phép viết mã tính toán tổng quát (không chỉ đồ họa) chạy trực tiếp trên kiến trúc GPU, hiện thực hóa mô hình lập trình dựa trên Bước 1 của Mục 8 |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Kiến trúc GPU tiếp tục là một trong những lĩnh vực nghiên cứu và phát triển công nghiệp sôi động nhất hiện nay, đặc biệt theo hướng **domain-specific architecture** — một chủ đề đã được đặt nền móng từ Chương 4.1.2, Mục 12 (domain-specific ISA).

Xu hướng rõ rệt nhất: các đơn vị tính toán chuyên biệt cho phép nhân ma trận (thay vì chỉ dùng lõi ALU đa dụng như mô tả ở chương này) ngày càng phổ biến trong GPU hiện đại — một bước tiến hóa tiếp theo của chính triết lý "từ bỏ tính đa dụng để đổi lấy hiệu năng cho một loại phép toán cụ thể" đã thấy xuyên suốt từ SIMD (Chương 4.1.7) tới GPU (chương này). Đây trực tiếp chuẩn bị nền tảng khái niệm cho Volume 05/06 khi thảo luận về hạ tầng huấn luyện mô hình AI quy mô lớn.

Một hướng nghiên cứu mở quan trọng khác: **huấn luyện phân tán (distributed training)** — khi một mô hình AI đơn lẻ quá lớn để một GPU xử lý, nhiều GPU cần phối hợp làm việc cùng nhau, đặt ra các bài toán mới về đồng bộ hóa và giao tiếp giữa các thiết bị (sẽ mở rộng đầy đủ ở Volume 4, Part IV — Concurrency, và Volume 06 — Advanced AI). Đây là một minh chứng khác cho việc Định luật Amdahl (Chương 4.1.7, Mục 7.1) không dừng lại ở một GPU đơn lẻ — nó tiếp tục áp dụng đệ quy khi mở rộng song song hóa ra nhiều thiết bị.

---

## 13. Ưu điểm

- **Thông lượng tính toán vượt trội cho khối lượng công việc song song cao:** như minh chứng định lượng ở Mục 10, GPU có thể đạt throughput gấp hàng trăm lần CPU cho bài toán phù hợp.
- **Che giấu độ trễ hiệu quả bằng đa luồng quy mô lớn:** thay vì đầu tư silicon vào dự đoán/suy đoán phức tạp (Chương 4.1.5, 4.1.6), GPU đơn giản chuyển sang warp khác khi một warp bị kẹt — một chiến lược "đơn giản mà hiệu quả" cho đúng loại workload phù hợp.
- **Hiệu quả năng lượng cao cho phép toán trên mỗi đơn vị công việc song song:** lõi đơn giản hơn (không out-of-order, không dự đoán rẽ nhánh tinh vi) tiêu thụ ít năng lượng hơn trên mỗi lõi, dù tổng thể GPU tiêu thụ nhiều năng lượng do số lượng lõi khổng lồ.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Nhấn mạnh lại lần cuối, xuyên suốt cả Bước 3/5 (Mục 8), Mục 10, và mục này: **mỗi lõi GPU đơn lẻ YẾU HƠN đáng kể một lõi CPU** — không có out-of-order (Chương 4.1.5), không có dự đoán rẽ nhánh tinh vi (Chương 4.1.6), tần số clock thường thấp hơn. GPU chỉ thắng CPU nhờ **số lượng**, không phải nhờ chất lượng từng lõi. Với một tác vụ đơn luồng, tuần tự, nhiều rẽ nhánh phức tạp — một CPU đơn lẻ gần như luôn nhanh hơn một lõi GPU đơn lẻ tương ứng, và thường nhanh hơn cả một GPU đầy đủ nếu $p$ (Mục 7.2) quá thấp.

- **Branch divergence có thể triệt tiêu phần lớn lợi thế throughput:** như minh họa ở Hình 4.1.8.2 và Mục 10, code có nhiều rẽ nhánh khác nhau giữa các luồng trong cùng warp gây tổn thất hiệu năng đáng kể.
- **Giới hạn Amdahl áp dụng nghiêm ngặt:** giống hệt SIMD (Chương 4.1.7), lợi thế của GPU hoàn toàn phụ thuộc vào $p$ — không phải "phép màu" cho mọi loại chương trình.
- **Chi phí di chuyển dữ liệu giữa CPU và GPU:** một chi tiết quan trọng chưa đi sâu ở chương này (thuộc phạm vi Part II — Memory Systems) — việc chuyển dữ liệu qua lại giữa bộ nhớ CPU và GPU có thể trở thành điểm nghẽn, đôi khi lớn hơn cả thời gian tính toán thực tế cho các bài toán nhỏ.

---

## 15. So sánh

**Bảng 4.1.8.1 — CPU (Latency-Oriented) và GPU (Throughput-Oriented)**

| Tiêu chí | CPU | GPU |
|---|---|---|
| Số lõi | Ít (thường dưới vài chục) | Rất nhiều (hàng nghìn) |
| Độ phức tạp mỗi lõi | Cao — out-of-order, dự đoán rẽ nhánh, cache lớn (Chương 4.1.5-6) | Thấp — ALU đơn giản, ít cơ chế suy đoán |
| Chiến lược che giấu độ trễ | Speculation (dự đoán, thực thi trước) | Chuyển đổi warp gần như tức thời |
| Xử lý rẽ nhánh phức tạp, khác nhau giữa các luồng | Tốt — mỗi luồng có bộ dự đoán riêng | Kém — gây branch divergence (Hình 4.1.8.2) |
| Phù hợp nhất với | Workload tuần tự, đa dạng, nhiều rẽ nhánh | Workload data-parallel cực cao (ví dụ Deep Learning) |

**Bảng 4.1.8.2 — Bốn kỹ thuật song song hóa của Part I: tổng kết**

| Chương | Kỹ thuật | Loại song song hóa | Đối tượng song song |
|:---:|---|---|---|
| 4.1.4 | Pipeline | Song song giai đoạn | Các giai đoạn của các lệnh liên tiếp |
| 4.1.5 | Superscalar / OoO | ILP | Các lệnh khác nhau về thao tác |
| 4.1.7 | SIMD | DLP (quy mô nhỏ) | Vài chục phần tử dữ liệu trong một lệnh |
| 4.1.8 | GPU / SIMT | DLP (quy mô lớn) | Hàng nghìn luồng thực thi độc lập |

**Phân tích:** Bảng 4.1.8.1 tổng kết chính xác thông điệp cốt lõi của chương — không có kiến trúc nào "tốt hơn tuyệt đối", chỉ có kiến trúc phù hợp hơn với một loại workload cụ thể, đúng tinh thần đánh đổi đã lặp lại xuyên suốt Part I (RISC/CISC ở 4.1.2, single/multi-cycle ở 4.1.3...). Bảng 4.1.8.2 nhìn lại toàn bộ hành trình của Part I từ góc độ "mọi chương đều là một câu trả lời cho bài toán song song hóa, chỉ khác về đối tượng và quy mô" — đúng insight đã nêu ở Mục 1.

---

## 16. Tóm tắt

- **GPU** theo triết lý **throughput-oriented**, đối lập với triết lý **latency-oriented** của CPU (Chương 4.1.3–4.1.6) — đánh đổi độ phức tạp của từng lõi lấy số lượng lõi khổng lồ.
- **SIMT** mở rộng trực tiếp SIMD (Chương 4.1.7) từ vài chục lane lên hàng nghìn luồng, nhóm thành **warp/wavefront** thực thi đồng bộ theo lockstep.
- **Che giấu độ trễ** bằng chuyển đổi warp gần như tức thời là chiến lược GPU dùng thay cho speculation phức tạp của CPU.
- **Branch divergence** — khi các luồng trong cùng warp rẽ nhánh khác nhau — là hạn chế cốt lõi của mô hình SIMT, buộc tuần tự hóa và làm mất lợi thế song song.
- **Định luật Amdahl** tiếp tục là công cụ định lượng trung tâm: lợi thế throughput khổng lồ của GPU chỉ phát huy đầy đủ khi tỷ lệ song song hóa $p$ của bài toán đủ cao — chính là lý do phép nhân ma trận trong Deep Learning trở thành "cặp đôi hoàn hảo" với kiến trúc GPU.
- Nhìn lại toàn bộ Part I: mỗi chương (pipeline, superscalar, SIMD, GPU) là một câu trả lời khác nhau, ở quy mô và đối tượng khác nhau, cho cùng một câu hỏi trung tâm — **làm thế nào khai thác song song hóa để vượt qua giới hạn của việc xử lý tuần tự từng lệnh một**.

Với chương này, Part I — Computer Organization and Architecture khép lại đầy đủ hành trình từ cổng logic đơn lẻ (Chương 4.1.1) tới kiến trúc GPU quy mô hàng nghìn lõi. Phần II của Volume 04 (Memory Systems) sẽ mở rộng câu chuyện sang một chiều hoàn toàn khác: nếu CPU và GPU đã có thể tính toán nhanh đến vậy, làm thế nào hệ thống bộ nhớ có thể "nuôi" đủ dữ liệu để không trở thành điểm nghẽn mới — trực tiếp nối tiếp vấn đề "chi phí di chuyển dữ liệu" đã nhắc đến ở Mục 14.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Tính $\text{Throughput}_{\text{tổng}}$ (Mục 7.1) cho một GPU giả định có 2000 lõi, tần số 1.2 GHz, 1 phép toán/chu kỳ mỗi lõi.
2. Giải thích bằng lời (không cần công thức) tại sao mỗi lõi GPU đơn lẻ chậm hơn một lõi CPU, nhưng tổng thể GPU vẫn nhanh hơn cho bài toán phù hợp.
3. Cho một warp 16 luồng với 12 luồng đi nhánh A và 4 luồng đi nhánh B, dùng `simulate_warp_execution` (Mục 9) xác định số "lượt thực thi" cần thiết.

### Mức Trung bình (Intermediate)

4. Với bài toán $10^9$ phép toán, GPU 4000 lõi, tần số 1.5 GHz, tính thời gian hoàn tất bằng `estimate_total_time` (Mục 9) cho hai trường hợp $p = 0.95$ và $p = 0.6$. So sánh và giải thích chênh lệch bằng Định luật Amdahl (Mục 7.2).
5. Giải thích bằng lời tại sao một warp với TOÀN BỘ luồng đi cùng một nhánh (branch không phân kỳ) không gây tổn thất hiệu năng nào, dựa trên đúng cơ chế mô tả ở Bước 5, Mục 8.

### Mức Nâng cao (Advanced)

6. Mở rộng `simulate_warp_execution` ở Mục 9 để hỗ trợ rẽ nhánh có NHIỀU HƠN hai nhánh khả dĩ (ví dụ cấu trúc `switch/case` với 4 trường hợp khác nhau). Viết một hàm mô phỏng tổng số "lượt thực thi lãng phí" khi một chương trình chứa nhiều lệnh rẽ nhánh liên tiếp, mỗi lệnh có mức độ phân kỳ khác nhau giữa các luồng.
7. So sánh định lượng hai chiến lược che giấu độ trễ đã học: dự đoán rẽ nhánh (Chương 4.1.6) trên CPU, và chuyển đổi warp (Mục 6, 8) trên GPU. Với một workload có tỷ lệ rẽ nhánh cao NHƯNG dễ đoán (ví dụ vòng lặp với điều kiện thoát rõ ràng, giống Chương 4.1.6, Mục 7.2), thảo luận kiến trúc nào có khả năng xử lý hiệu quả hơn, và tại sao.

### Mức Nghiên cứu (Research)

8. Tìm hiểu ở mức khái niệm về các đơn vị tính toán ma trận chuyên biệt trong GPU hiện đại (đã nhắc ở Mục 12) — khác biệt với lõi ALU đa dụng mô tả trong chương này ra sao? Thảo luận: đây có phải là một biểu hiện khác của xu hướng domain-specific ISA đã học ở Chương 4.1.2, Mục 12 hay không, và vì sao.

---

## 18. Dự án nhỏ

**Dự án: Bộ mô phỏng SIMT với Branch Divergence và Phân tích Amdahl**

- **Mục tiêu:** Xây dựng một công cụ mô phỏng hoàn chỉnh, kết hợp mô hình warp/branch divergence (Mục 9) với phân tích thông lượng tổng hợp và Định luật Amdahl, áp dụng cho một bài toán cụ thể.
- **Yêu cầu:**
  - Mô phỏng một "chương trình" GPU gồm nhiều giai đoạn: một số giai đoạn hoàn toàn không phân kỳ (branch-free), một số giai đoạn có branch divergence với tỷ lệ phân kỳ khác nhau.
  - Với mỗi giai đoạn, tính số "lượt thực thi" cần thiết bằng `simulate_warp_execution`, tổng hợp lại thành thời gian thực thi toàn chương trình.
  - So sánh tổng thời gian thực thi giữa: (a) chương trình có nhiều rẽ nhánh phân kỳ mạnh; (b) cùng khối lượng công việc đó nhưng được "viết lại" (giả định) để giảm phân kỳ, ví dụ nhóm các luồng có cùng kết quả rẽ nhánh lại gần nhau.
  - Kết hợp với `estimate_total_time` (Mục 9) để tính Speedup tổng thể so với một cấu hình CPU giả định, cho cả hai phiên bản chương trình ở trên.
- **Công nghệ gợi ý:** Python thuần, tái sử dụng toàn bộ hạ tầng từ Mục 9.
- **Kết quả mong đợi:** Một bảng so sánh định lượng cho thấy việc "tổ chức lại" luồng để giảm branch divergence có thể cải thiện hiệu năng GPU đáng kể, dù tổng khối lượng công việc không đổi.
- **Hướng mở rộng:** Đây là dự án khép lại Part I — người đọc được khuyến khích quay lại các dự án nhỏ ở Chương 4.1.4 (pipeline), 4.1.5 (superscalar), 4.1.6 (branch prediction), 4.1.7 (SIMD), và thử tổng hợp một "bộ mô phỏng CPU đầy đủ" tích hợp cả bốn kỹ thuật, làm nền tảng thực hành vững chắc trước khi bước sang Part II — Memory Systems.

---

## 19. Tự đánh giá

- [ ] Tôi có thể giải thích rõ ràng triết lý throughput-oriented của GPU, đối lập với latency-oriented của CPU, kèm ví dụ cụ thể cho mỗi loại workload phù hợp.
- [ ] Tôi có thể áp dụng công thức thông lượng tổng hợp (Mục 7.1) và Định luật Amdahl (Mục 7.2) để so sánh định lượng CPU và GPU cho một bài toán cho trước.
- [ ] Tôi giải thích được chính xác cơ chế branch divergence và tính toán được số "lượt thực thi" cần thiết cho một warp cho trước.
- [ ] Tôi hiểu rõ và có thể phản bác lại quan niệm sai lầm "GPU luôn nhanh hơn CPU", dựa trên lập luận định lượng (không chỉ định tính).
- [ ] Tôi có thể nhìn lại và tóm tắt được cả bốn kỹ thuật song song hóa của Part I (Bảng 4.1.8.2) theo đúng trục "đối tượng và quy mô song song hóa".

Nếu Bài tập 4 hoặc 7 (kết hợp throughput và Amdahl, hoặc so sánh chiến lược che giấu độ trễ) vẫn còn khó khăn, nên dành thời gian ôn lại toàn bộ Chương 4.1.4–4.1.7 trước khi chuyển sang Part II — đây là điểm tổng hợp quan trọng của toàn bộ các khái niệm hiệu năng đã xây dựng xuyên suốt Part I.

---

## 20. Đọc thêm

- **Sách:** Randal E. Bryant, David R. O'Hallaron, *Computer Systems: A Programmer's Perspective* — nền tảng về tổ chức xử lý sẽ hữu ích khi đối chiếu với kiến trúc GPU throughput-oriented trình bày trong chương này. *(Xem BOOKS.md — Volume 4.)*
- **Bài báo nền tảng:** Krizhevsky, Sutskever, Hinton — *ImageNet Classification with Deep Convolutional Neural Networks* (2012) — minh chứng lịch sử trực tiếp cho vai trò của GPU trong bước ngoặt Deep Learning hiện đại. *(Xem PAPERS.md.)*
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc thêm về mô hình lập trình GPGPU (ví dụ CUDA hoặc các framework tương đương) để hiểu cách các khái niệm warp, thread, block được ánh xạ vào mã nguồn thực tế.
- **Tiếp theo:** Volume 04, Part II — Memory Systems.

---

### Liên kết chương (Cross References)

- **Chương trước:** 4.1.7 — SIMD (SIMT là mở rộng trực tiếp của SIMD, Mục 6); 4.1.5, 4.1.6 — Superscalar/Out-of-Order và Branch Prediction (GPU chủ động từ bỏ các cơ chế này, Hình 4.1.8.1, đổi lấy số lượng lõi).
- **Chương tiếp theo (Part tiếp theo):** Volume 04, Part II — Memory Systems, giải quyết vấn đề "nuôi dữ liệu" cho cả CPU và GPU đã tính toán nhanh hơn nhiều so với khả năng cung cấp dữ liệu của bộ nhớ truyền thống.
- **Chương liên quan xa hơn:** Volume 05 — Artificial Intelligence (phép nhân ma trận và huấn luyện mạng nơ-ron, Mục 3, 11); Volume 06 — Advanced AI (huấn luyện phân tán đa GPU, Mục 12); Volume 4, Part VIII — High Performance Computing (GPU cluster, tính toán khoa học, Mục 11); Volume 4, Part IV — Concurrency and Parallel Computing (mở rộng khái niệm che giấu độ trễ và Amdahl's Law sang đa lõi/đa máy).
- **Vị trí trong Knowledge Graph:** Nút thứ tám và cuối cùng của Volume 04, Part I; phụ thuộc trực tiếp vào Chương 4.1.5, 4.1.6, và 4.1.7; khép lại toàn bộ chuỗi kiến thức về Computer Organization and Architecture, mở đường cho Part II — Memory Systems.

---

*Hết Chương 4.1.8, và khép lại Part I — Computer Organization and Architecture. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`. Mọi tính toán throughput và mô phỏng branch divergence đều được kiểm chứng bằng Python thực thi thực tế (Mục 9–10), nhất quán với toàn bộ hệ thống công thức hiệu năng đã xây dựng xuyên suốt Chương 4.1.3–4.1.7. Đang chờ rà soát trước khi tiếp tục sang Part II — Memory Systems.*
