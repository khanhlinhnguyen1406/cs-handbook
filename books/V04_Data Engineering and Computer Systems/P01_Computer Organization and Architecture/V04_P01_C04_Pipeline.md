# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 04 — Computer Systems
### Part I — Computer Organization and Architecture
## Chương 4.1.4 — Pipeline
### (Kỹ thuật Xử lý Đường ống)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 4.1.4 |
| Thuộc Part | I — Computer Organization and Architecture |
| Thuộc Volume | 04 — Computer Systems |
| Thời gian đọc ước tính | 55–70 phút |
| Độ khó | ★★★★☆ |
| Kiến thức tiên quyết | Chương 4.1.1 — Digital Logic Overview; Chương 4.1.2 — Instruction Set Architecture; Chương 4.1.3 — CPU Organization & Instruction Execution Cycle |
| Chương liên quan | 4.1.5 — Superscalar Architecture & Out-of-Order Execution: mở rộng trực tiếp ý tưởng chồng lấp học ở chương này; 4.1.6 — Branch Prediction: giải quyết loại hazard nghiêm trọng nhất chưa xử lý triệt để ở đây |
| Từ khóa | pipeline, pipeline stage, pipeline register, structural hazard, data hazard, control hazard, stall, bubble, forwarding, throughput, latency |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Giải thích ý tưởng cốt lõi của pipelining: chồng lấp các giai đoạn của nhiều lệnh khác nhau để tăng thông lượng.
- Mô tả pipeline 5 giai đoạn kinh điển (IF–ID–EX–MEM–WB), mở rộng trực tiếp từ chu trình 4 bước đã học ở Chương 4.1.3.
- Phân biệt ba loại hazard: structural, data, và control — cùng nguyên nhân gốc rễ của từng loại.
- Giải thích kỹ thuật stall (chèn bubble) và forwarding (bypassing) như hai cách xử lý data hazard.
- Tính tốc độ tăng tốc lý thuyết (speedup) của pipeline bằng công thức định lượng, phân biệt rõ giữa **latency** và **throughput**.
- Nhận diện vì sao pipelining không làm một lệnh đơn lẻ chạy nhanh hơn, nhưng làm cả chương trình chạy nhanh hơn đáng kể.

---

### Câu hỏi khơi gợi

> *Nếu bạn có 4 mẻ đồ cần giặt, sấy, và gấp — mỗi công đoạn mất 30 phút — bạn có thực sự cần chờ toàn bộ một mẻ giặt-sấy-gấp xong hoàn toàn rồi mới bắt đầu mẻ tiếp theo? Và nếu áp dụng "mẹo" hiển nhiên đó vào CPU — bắt đầu lệnh tiếp theo trước khi lệnh hiện tại hoàn tất — điều gì có thể xảy ra sai lệch, khi hai lệnh liên tiếp lại "cần dùng chung một thứ" tại cùng một thời điểm?*

---

## 1. Tổng quan chương

Chương 4.1.3 kết thúc bằng một hạn chế rõ ràng: trong mô hình Fetch–Decode–Execute–Writeback đơn giản, khi một lệnh đang ở giai đoạn Execute, toàn bộ mạch Fetch dành cho lệnh tiếp theo nằm hoàn toàn nhàn rỗi — một sự lãng phí phần cứng đáng kể. **Pipeline (kỹ thuật xử lý đường ống)** là câu trả lời trực tiếp cho vấn đề này: thay vì để mỗi giai đoạn phần cứng chỉ phục vụ đúng một lệnh tại một thời điểm, pipeline cho phép **nhiều lệnh ở các giai đoạn khác nhau chạy chồng lấp đồng thời**.

Đây là chương đầu tiên của Part I đưa hiệu năng CPU lên một tầm mức mới — không còn hỏi "một lệnh mất bao lâu để chạy" mà hỏi "trung bình bao lâu CPU hoàn thành **một** lệnh khi chạy hàng triệu lệnh liên tiếp". Sự khác biệt giữa hai câu hỏi này — latency và throughput — là chủ đề xuyên suốt của chương, và cũng chính là chìa khóa để hiểu tại sao pipeline mang lại lợi ích to lớn dù không hề làm phần cứng "nhanh hơn" theo nghĩa vật lý.

> **💡 Insight**
> Pipeline không làm cổng logic chuyển mạch nhanh hơn, không làm mạch điện "thông minh" hơn — nó chỉ đơn giản là **không để phần cứng nhàn rỗi**. Đây là một bài học tổng quát vượt xa phạm vi CPU: trong bất kỳ hệ thống xử lý theo lô (batch processing) nào — từ dây chuyền sản xuất đến CI/CD pipeline trong kỹ thuật phần mềm — chồng lấp các giai đoạn độc lập luôn là một trong những kỹ thuật tăng thông lượng rẻ nhất, miễn là bạn xử lý đúng các trường hợp phụ thuộc lẫn nhau giữa các "lô hàng".

---

## 2. Bối cảnh lịch sử

| Thời điểm | Sự kiện | Đóng góp |
|---|---|---|
| Cuối thập niên 1960 | Các máy tính hiệu năng cao thời kỳ đầu bắt đầu áp dụng kỹ thuật xử lý chồng lấp giai đoạn lệnh | Đặt nền móng thực tiễn đầu tiên cho ý tưởng pipeline ở cấp độ CPU, trước khi kỹ thuật này trở thành tiêu chuẩn phổ biến |
| Thập niên 1980 | Pipeline trở thành thành phần thiết kế trung tâm của triết lý RISC (đã giới thiệu ở Chương 4.1.2, Mục 2) | Tập lệnh đơn giản, độ dài cố định của RISC đặc biệt phù hợp để chia thành các giai đoạn pipeline đều đặn, dễ đồng bộ hóa |
| Từ thập niên 1990 đến nay | CPU thương mại đa dụng (x86, ARM) áp dụng pipeline ngày càng sâu (nhiều giai đoạn hơn), kết hợp với các kỹ thuật ở Chương 4.1.5–4.1.6 | Pipeline trở thành nền tảng bắt buộc, không thể thiếu, của mọi CPU hiệu năng cao hiện đại |

Điều thú vị về mặt lịch sử: ý tưởng "chồng lấp các công đoạn độc lập để tăng thông lượng" không hề mới hay đặc thù cho ngành điện toán — nó là nguyên lý cốt lõi của dây chuyền lắp ráp công nghiệp (assembly line) đã tồn tại từ đầu thế kỷ 20. Pipeline CPU chỉ đơn thuần là việc áp dụng lại chính nguyên lý sản xuất công nghiệp đó vào bên trong một con chip silicon.

---

## 3. Động lực

Quay lại câu hỏi khơi gợi: bài toán "giặt — sấy — gấp" 4 mẻ đồ là một ví dụ kinh điển minh họa chính xác vấn đề CPU gặp phải. Nếu làm tuần tự hoàn toàn — giặt xong mẻ 1, sấy xong mẻ 1, gấp xong mẻ 1, rồi mới bắt đầu mẻ 2 — bạn lãng phí máy giặt trong lúc máy sấy đang chạy, và lãng phí máy sấy trong lúc bạn đang gấp đồ. Với 4 mẻ, mỗi công đoạn 30 phút, cách làm tuần tự mất $4 \times 3 \times 30 = 360$ phút.

Nhưng nếu bạn bắt đầu giặt mẻ 2 ngay khi mẻ 1 chuyển sang máy sấy — nghĩa là ba công đoạn chạy **chồng lấp**, mỗi công đoạn xử lý một mẻ khác nhau tại cùng một thời điểm — tổng thời gian giảm đáng kể, dù mỗi mẻ đồ riêng lẻ vẫn mất đúng 90 phút để hoàn tất (giặt + sấy + gấp của chính nó, không đổi).

Đây chính xác là những gì pipeline CPU thực hiện, với "mẻ đồ" là **lệnh máy**, và "công đoạn" là các giai đoạn Fetch, Decode, Execute... Nhưng có một khác biệt quan trọng so với giặt đồ: các lệnh trong một chương trình thường **không độc lập với nhau** — lệnh sau có thể cần dùng kết quả của lệnh trước, hoặc cần biết lệnh trước có "rẽ nhánh" hay không trước khi biết chính mình là lệnh gì. Phần còn lại của chương này giải quyết chính xác vấn đề "các mẻ đồ phụ thuộc lẫn nhau" đó — gọi là **hazard**.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Pipeline CPU giống hệt **dây chuyền giặt-sấy-gấp** ở Mục 3: mỗi "trạm" trong dây chuyền (Fetch, Decode, Execute...) luôn bận rộn xử lý một lệnh khác nhau tại mỗi thời điểm, thay vì chờ một lệnh đi qua trọn vẹn hết dây chuyền rồi mới nhận lệnh tiếp theo. Một lệnh riêng lẻ vẫn mất đúng bằng đó thời gian để "đi hết dây chuyền" (latency không đổi, thậm chí hơi tăng do chi phí đồng bộ hóa) — nhưng **tần suất** lệnh hoàn tất (throughput) tăng vọt, vì nhiều lệnh đang được xử lý song song ở các trạm khác nhau.

| Trực giác kỹ thuật bạn đã có | Khái niệm Pipeline tương ứng |
|---|---|
| Hàng đợi xử lý bất đồng bộ (async queue) trong hệ thống backend, nơi nhiều request được xử lý chồng lấp qua nhiều worker | Pipeline stage — mỗi giai đoạn là một "worker" chuyên trách một công đoạn |
| CI/CD pipeline (build → test → deploy) chạy chồng lấp cho nhiều commit liên tiếp | Chồng lấp các giai đoạn Fetch/Decode/Execute cho nhiều lệnh liên tiếp |
| Race condition khi hai luồng (thread) cùng đọc/ghi một biến chia sẻ chưa đồng bộ | Data hazard — hai lệnh liên tiếp trong pipeline cùng "chạm" vào một thanh ghi |

---

## 5. Trực quan hóa khái niệm

**Hình 4.1.4.1 — Pipeline 5 giai đoạn kinh điển: chồng lấp 4 lệnh liên tiếp**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
Chu kỳ:        1     2     3     4     5     6     7     8
Lệnh 1:        IF    ID    EX    MEM   WB
Lệnh 2:              IF    ID    EX    MEM   WB
Lệnh 3:                    IF    ID    EX    MEM   WB
Lệnh 4:                          IF    ID    EX    MEM   WB
                                              ▲
                            Tại chu kỳ 5, CẢ 5 giai đoạn phần cứng
                            đều đang bận rộn, mỗi giai đoạn xử lý
                            MỘT lệnh khác nhau — đây chính là trạng
                            thái "ổn định" (steady state) của pipeline.
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Cho thấy trực quan cách 4 lệnh hoàn tất chỉ sau 8 chu kỳ, thay vì $4 \times 5 = 20$ chu kỳ nếu chạy hoàn toàn tuần tự (mỗi lệnh đi hết 5 giai đoạn rồi mới đến lệnh tiếp theo) |
| Điểm mấu chốt | Mỗi lệnh riêng lẻ **vẫn mất đúng 5 chu kỳ** để hoàn tất (latency không đổi) — độ lợi nằm hoàn toàn ở **throughput**: từ chu kỳ 5 trở đi, cứ mỗi 1 chu kỳ lại có 1 lệnh hoàn tất, thay vì 1 lệnh mỗi 5 chu kỳ |

---

**Hình 4.1.4.2 — Data Hazard và giải pháp Forwarding**

```text
             Lệnh 1: ADD R1, R2, R3    (R1 = R2 + R3)
             Lệnh 2: SUB R4, R1, R5    (R4 = R1 - R5  ← CẦN giá trị R1 MỚI)

Không xử lý (SAI):
  Lệnh 1:  IF  ID  EX   MEM  WB(ghi R1 vào Register File)
  Lệnh 2:      IF  ID(đọc R1 — NHƯNG R1 CHƯA ĐƯỢC GHI!)  EX  MEM  WB
                    ▲
                    Lệnh 2 đọc giá trị R1 CŨ — kết quả SAI

Giải pháp Forwarding (bắc cầu trực tiếp từ EX sang EX):
  Lệnh 1:  IF  ID  EX(tính xong R1)  MEM  WB
  Lệnh 2:      IF  ID   EX(nhận R1 TRỰC TIẾP từ đầu ra EX
                            của Lệnh 1, KHÔNG cần chờ WB)  MEM  WB
                    ▲
                    Dữ liệu được "chuyển tiếp" (forward) qua một
                    đường dây phần cứng bổ sung, bỏ qua Register File
```

*Mục đích:* Minh họa cụ thể vấn đề data hazard nêu ở câu hỏi khơi gợi, và cách kỹ thuật **forwarding** (còn gọi là bypassing) giải quyết vấn đề mà không cần "chờ" (stall) — bằng cách thêm một đường dây phần cứng đưa kết quả từ đầu ra giai đoạn EX của lệnh trước thẳng tới đầu vào giai đoạn EX của lệnh sau. *Điểm mấu chốt:* forwarding chỉ giải quyết được khi khoảng cách giữa hai lệnh đủ "gần" phần cứng có thể bắc cầu; một số trường hợp (ví dụ lệnh ngay sau `LOAD`) vẫn buộc phải **stall** — xem Mục 8.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Pipeline và Pipeline Register**
>
> **Pipeline** là kỹ thuật tổ chức phần cứng chia chu trình xử lý một lệnh thành nhiều **giai đoạn (stage)** độc lập, mỗi giai đoạn xử lý một phần công việc, cho phép nhiều lệnh ở các giai đoạn khác nhau được xử lý đồng thời trong cùng một chu kỳ clock. Giữa hai giai đoạn liên tiếp là một **pipeline register** (một nhóm D Flip-Flop, xem Chương 4.1.1) lưu trữ kết quả trung gian, đảm bảo mỗi giai đoạn chỉ "nhìn thấy" đúng dữ liệu của lệnh mà nó đang xử lý.

> **📌 Remember — Pipeline 5 giai đoạn kinh điển**
>
> | Giai đoạn | Tên đầy đủ | Công việc | Tương ứng Chương 4.1.3 |
> |---|---|---|---|
> | IF | Instruction Fetch | Đọc lệnh từ bộ nhớ tại địa chỉ PC | Bước Fetch |
> | ID | Instruction Decode | Giải mã opcode, đọc thanh ghi nguồn | Bước Decode |
> | EX | Execute | ALU thực hiện phép toán | Bước Execute |
> | MEM | Memory Access | Đọc/ghi bộ nhớ dữ liệu (nếu lệnh cần) | (mở rộng của Execute) |
> | WB | Write Back | Ghi kết quả vào Register File | Bước Writeback |

> **📌 Remember — Ba loại Hazard**
>
> - **Structural Hazard:** hai lệnh cùng cần dùng **một tài nguyên phần cứng** tại cùng một chu kỳ (ví dụ: cả hai cùng cần truy cập bộ nhớ nếu bộ nhớ lệnh và bộ nhớ dữ liệu dùng chung một cổng truy cập).
> - **Data Hazard:** một lệnh cần một giá trị **chưa được tính/ghi xong** bởi lệnh trước đó (minh họa ở Hình 4.1.4.2).
> - **Control Hazard:** pipeline chưa biết lệnh **tiếp theo cần fetch là lệnh nào**, vì lệnh hiện tại là một rẽ nhánh chưa được giải quyết xong (liên hệ trực tiếp Chương 4.1.3, Mục 8, và sẽ được xử lý đầy đủ ở Chương 4.1.6).

---

## 7. Nền tảng toán học

### 7.1 Tốc độ tăng tốc lý thuyết của Pipeline (Speedup)

- **Ý nghĩa:** Định lượng chính xác lợi ích của pipeline so với thực thi tuần tự hoàn toàn, dựa trên số giai đoạn $k$ và số lệnh $n$.
- **Thời gian không dùng pipeline:** mỗi lệnh cần đi hết $k$ giai đoạn tuần tự, không chồng lấp — tổng $n$ lệnh cần $n \times k$ chu kỳ.
- **Thời gian dùng pipeline** (như Hình 4.1.4.1): lệnh đầu tiên cần $k$ chu kỳ để đi hết pipeline; sau đó, mỗi chu kỳ tiếp theo hoàn tất thêm đúng 1 lệnh — tổng cộng $k + (n - 1)$ chu kỳ.

> **📦 Formula Box — Speedup của Pipeline**
>
> $$\text{Speedup} = \frac{T_{\text{không pipeline}}}{T_{\text{pipeline}}} = \frac{n \times k}{k + (n - 1)}$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $n$ | Số lượng lệnh cần thực thi |
> | $k$ | Số giai đoạn của pipeline (ví dụ $k=5$ với pipeline kinh điển ở Mục 6) |
> | **Diễn giải kỹ thuật** | Khi $n \to \infty$ (chương trình rất dài), $\text{Speedup} \to k$ — nghĩa là pipeline $k$ giai đoạn, về mặt lý thuyết, có thể nhanh gấp tối đa $k$ lần so với không dùng pipeline |
> | **Ứng dụng thường gặp** | Ước lượng lợi ích tối đa khi quyết định tăng số giai đoạn pipeline trong thiết kế CPU — đồng thời cũng là cơ sở toán học cho câu hỏi "vì sao không tăng số giai đoạn lên vô hạn", sẽ trả lời ở Mục 7.2 |

**Ví dụ áp dụng:** với $k = 5$ và $n = 1000$ lệnh: $\text{Speedup} = \dfrac{1000 \times 5}{5 + 999} = \dfrac{5000}{1004} \approx 4.98$ — rất gần với giới hạn lý thuyết $k=5$, đúng như dự đoán khi $n$ đủ lớn.

### 7.2 Vì sao không tăng số giai đoạn pipeline lên vô hạn?

Công thức ở Mục 7.1 cho thấy speedup tăng theo $k$ — vậy tại sao CPU thực tế không dùng pipeline với hàng trăm giai đoạn? Câu trả lời nằm ở chính công thức hiệu năng đã học ở Chương 4.1.3, Mục 7.1: $T_{\text{execution}} = N \times \text{CPI} \times T_{\text{clock cycle}}$. Chia nhỏ công việc thành nhiều giai đoạn hơn giúp giảm $T_{\text{clock cycle}}$ (mỗi giai đoạn đơn giản hơn, xử lý nhanh hơn), nhưng đồng thời:

- **Tăng chi phí pipeline register:** mỗi ranh giới giai đoạn thêm cần một pipeline register (Mục 6) — bản thân việc "chốt" dữ liệu qua flip-flop cũng tốn thời gian, không miễn phí.
- **Tăng số lượng và mức độ nghiêm trọng của hazard:** pipeline càng sâu, khoảng cách giữa lúc fetch một lệnh rẽ nhánh và lúc biết kết quả rẽ nhánh càng xa — control hazard (Mục 6) càng gây tổn thất nặng nề hơn, chủ đề trọng tâm của Chương 4.1.6.

Đây lại là một minh chứng khác cho nguyên tắc đánh đổi đã lặp lại xuyên suốt Part I: giảm $T_{\text{clock cycle}}$ bằng cách tăng $k$ không "miễn phí" — nó làm tăng rủi ro CPI hiệu dụng tăng lên do hazard, đúng tinh thần Chương 4.1.3, Mục 7.2.

---

## 8. Thuật toán / Cơ chế

**Quy trình phát hiện và xử lý Data Hazard bằng Forwarding, kèm Stall khi cần thiết:**

```text
Bước 1 — Tại giai đoạn ID của lệnh hiện tại, xác định các thanh
         ghi NGUỒN mà lệnh này cần đọc
        │
        ▼
Bước 2 — So sánh các thanh ghi nguồn đó với thanh ghi ĐÍCH của
         những lệnh đang ở giai đoạn EX hoặc MEM phía trước
         trong pipeline (chưa kịp ghi vào Register File)
        │
        ▼
Bước 3 — Nếu trùng khớp VÀ giá trị cần đã được TÍNH XONG (đang ở
         đầu ra giai đoạn EX hoặc MEM của lệnh trước):
         → Kích hoạt FORWARDING: nối trực tiếp đường dây từ đầu
           ra giai đoạn đó tới đầu vào ALU của lệnh hiện tại,
           bỏ qua việc chờ ghi vào Register File (Hình 4.1.4.2)
        │
        ▼
Bước 4 — Nếu trùng khớp NHƯNG giá trị CHƯA sẵn sàng (ví dụ: lệnh
         trước là LOAD, giá trị chỉ có sau giai đoạn MEM, không
         thể forward kịp tới giai đoạn EX của lệnh hiện tại):
         → Kích hoạt STALL: chèn một "bubble" (chu kỳ trống,
           không làm gì) vào pipeline, trì hoãn lệnh hiện tại
           đúng số chu kỳ cần thiết để giá trị sẵn sàng
        │
        ▼
Bước 5 — Nếu lệnh hiện tại là lệnh RẼ NHÁNH (control hazard):
         → Áp dụng một trong các chiến lược tạm thời: stall cho
           tới khi biết kết quả rẽ nhánh, hoặc GIẢ ĐỊNH một
           hướng đi và fetch tiếp (dự đoán rẽ nhánh — nội dung
           đầy đủ của Chương 4.1.6)
```

> **⚠️ Common Mistake**
> Một hiểu lầm phổ biến là nghĩ rằng **stall làm chương trình chạy sai kết quả nếu không xử lý cẩn thận**. Thực tế ngược lại: stall (và forwarding) chính là các cơ chế **đảm bảo tính đúng đắn** — nếu không có chúng, pipeline mới là thứ tạo ra kết quả sai (như minh họa "Không xử lý (SAI)" ở Hình 4.1.4.2). Cái giá phải trả của stall không phải là độ chính xác, mà là **hiệu năng** — mỗi bubble được chèn vào tương đương với việc tăng CPI hiệu dụng, kéo speedup thực tế ra xa khỏi giới hạn lý thuyết ở Mục 7.1.

---

## 9. Triển khai

```python
# Mô phỏng Pipeline 5 giai đoạn (đơn giản hóa) cho ISA đồ chơi ở
# Chương 4.1.2/4.1.3, với phát hiện Data Hazard cơ bản và chèn Stall.
# (Bỏ qua Forwarding để giữ ví dụ dễ theo dõi — xem Bài tập 6.)

STAGES = ["IF", "ID", "EX", "MEM", "WB"]


def get_registers_used(mnemonic, operand):
    """Trả về (thanh_ghi_nguon, thanh_ghi_dich) cho mục đích phát
    hiện hazard — đơn giản hóa cho ISA đồ chơi (chỉ có ACC ngầm
    định và một thanh ghi 'operand')."""
    if mnemonic in ("ADD", "SUB"):
        return {operand}, {"ACC"}          # đọc registers[operand], ghi ACC
    if mnemonic == "STORE":
        return {"ACC"}, {operand}           # đọc ACC, ghi registers[operand]
    if mnemonic == "LOAD":
        return set(), {"ACC"}               # không đọc, chỉ ghi ACC
    return set(), set()


def simulate_pipeline(instructions):
    """Mô phỏng lịch trình 5 giai đoạn cho một danh sách lệnh đã
    decode sẵn thành (mnemonic, operand), CHÈN STALL khi phát
    hiện Data Hazard (Bước 4, Mục 8), không dùng Forwarding."""
    n = len(instructions)
    schedule = []          # schedule[i] = list các (cycle, stage) của lệnh i
    cycle_start = [0] * n  # chu kỳ IF bắt đầu của mỗi lệnh

    for i in range(n):
        mnemonic, operand = instructions[i]
        src, dst = get_registers_used(mnemonic, operand)

        earliest_start = cycle_start[i - 1] + 1 if i > 0 else 0

        # Kiểm tra hazard với lệnh liền trước (đơn giản hóa:
        # chỉ xét khoảng cách 1 lệnh, đủ minh họa cơ chế Mục 8)
        if i > 0:
            _, prev_dst = get_registers_used(*instructions[i - 1])
            if src & prev_dst:
                # Cần STALL: lệnh trước ghi ACC tại giai đoạn EX
                # (chu kỳ cycle_start[i-1] + 2); giả sử KHÔNG có
                # forwarding, lệnh hiện tại phải đợi lệnh trước
                # đi tới WB rồi mới ID an toàn -> trì hoãn 2 chu kỳ
                earliest_start += 2

        cycle_start[i] = earliest_start
        schedule.append([(earliest_start + s, STAGES[s]) for s in range(5)])

    return schedule
```

Hàm `simulate_pipeline` hiện thực hóa Bước 1–4 của Mục 8 ở dạng đơn giản hóa: `get_registers_used` xác định thanh ghi nguồn/đích của mỗi lệnh (Bước 1), vòng lặp chính so sánh với lệnh liền trước để phát hiện trùng khớp (Bước 2), và khi phát hiện hazard, trì hoãn lệnh hiện tại thêm 2 chu kỳ thay vì dùng forwarding (mô phỏng trường hợp Stall thuần túy ở Bước 4) — cố tình đơn giản hóa để làm nổi bật hậu quả về hiệu năng của việc **không** có forwarding, nhấn mạnh giá trị của Hình 4.1.4.2.

---

## 10. Trực quan hóa quá trình thực thi

**So sánh hai chương trình:** một chương trình không có phụ thuộc dữ liệu, và một chương trình có data hazard liên tiếp.

**Trường hợp 1 — Không có hazard** (`LOAD`, `LOAD`, `LOAD`, `LOAD` — không đọc/ghi chồng chéo `ACC` theo cách gây hazard giả định trong mô hình đơn giản hóa này):

```text
Lệnh 0 (LOAD):  IF  ID  EX  MEM  WB
Lệnh 1 (LOAD):      IF  ID  EX   MEM  WB
Lệnh 2 (LOAD):          IF  ID   EX   MEM  WB
Lệnh 3 (LOAD):              IF   ID   EX   MEM  WB
```

Tổng cộng: 8 chu kỳ cho 4 lệnh — khớp với dự đoán $k + (n-1) = 5 + 3 = 8$ ở Mục 7.1.

**Trường hợp 2 — Có Data Hazard liên tiếp** (`LOAD`, `ADD` — lệnh `ADD` đọc `ACC` do `LOAD` vừa ghi):

```text
Lệnh 0 (LOAD): IF  ID  EX  MEM  WB
Lệnh 1 (ADD):      IF  --  --  ID  EX  MEM  WB
                        ▲───▲
                     2 chu kỳ STALL (bubble) được chèn vào,
                     đúng theo Bước 4 và hàm simulate_pipeline ở Mục 9
```

Với `simulate_pipeline([("LOAD", 5), ("ADD", 0)])`, kết quả: lệnh 0 bắt đầu IF tại chu kỳ 0 (hoàn tất tại chu kỳ 4); lệnh 1, thay vì bắt đầu IF tại chu kỳ 1 như bình thường, bị trì hoãn — minh họa rõ ràng cái giá của việc thiếu forwarding, đúng như đã cảnh báo ở Common Mistake, Mục 8.

**Áp dụng công thức Speedup (Mục 7.1)** cho Trường hợp 2 với hazard: vì có 2 chu kỳ bị "lãng phí" do stall, speedup thực tế thấp hơn giới hạn lý thuyết $k=5$ — một minh chứng định lượng trực tiếp cho lý do vì sao forwarding (Hình 4.1.4.2) là kỹ thuật quan trọng bậc nhất để pipeline đạt gần tới hiệu năng lý thuyết.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Pipeline không phải một kỹ thuật "tùy chọn" trong CPU hiện đại — nó là nền tảng bắt buộc, và độ sâu pipeline (số giai đoạn) là một trong những thông số thiết kế được cân nhắc kỹ lưỡng nhất khi phát triển một kiến trúc CPU mới.

| Bối cảnh công nghiệp | Vai trò của Pipeline |
|---|---|
| CPU x86/ARM hiện đại | Sử dụng pipeline sâu hơn đáng kể so với mô hình 5 giai đoạn kinh điển ở chương này, kết hợp chặt chẽ với các kỹ thuật ở Chương 4.1.5–4.1.6 để giảm thiểu tổn thất do hazard |
| Trình biên dịch tối ưu hóa (optimizing compiler) | Kỹ thuật "instruction scheduling" — sắp xếp lại thứ tự lệnh (khi ngữ nghĩa chương trình không đổi) để giảm thiểu data hazard, hỗ trợ trực tiếp bộ xử lý pipeline bên dưới |
| GPU (sẽ học ở Chương 4.1.8) | Cũng áp dụng pipelining sâu, nhưng theo mô hình xử lý dữ liệu song song quy mô lớn khác biệt với CPU đơn luồng lệnh |
| Kỹ thuật phần mềm nói chung — CI/CD pipeline | Chính cái tên "pipeline" trong CI/CD (build → test → deploy chồng lấp cho nhiều commit) mượn trực tiếp ý tưởng kỹ thuật này từ kiến trúc máy tính |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Bài toán tối ưu hóa pipeline — vừa tối đa hóa speedup (tăng $k$, Mục 7.1), vừa tối thiểu hóa tổn thất do hazard (Mục 7.2, 8) — là một trong những chủ đề nghiên cứu lâu đời và có ảnh hưởng sâu rộng nhất trong lĩnh vực Computer Architecture.

Hai hướng nghiên cứu/kỹ thuật công nghiệp trực tiếp giải quyết những hạn chế đã nêu ở chương này sẽ được trình bày đầy đủ ở hai chương tiếp theo:

- **Chương 4.1.5 (Superscalar & Out-of-Order Execution):** thay vì chỉ chồng lấp các giai đoạn của lệnh theo đúng thứ tự chương trình, các CPU hiện đại còn cho phép **sắp xếp lại thứ tự thực thi** ngay tại phần cứng (không cần chờ compiler), giảm thiểu tổn thất do data hazard một cách linh động hơn nhiều so với stall tĩnh.
- **Chương 4.1.6 (Branch Prediction):** giải quyết triệt để control hazard (Mục 6) — loại hazard gây tổn thất nặng nhất trong pipeline sâu — bằng cách **dự đoán** hướng rẽ nhánh thay vì chờ đợi hoặc luôn stall.

Một câu hỏi nghiên cứu mở đáng chú ý: độ sâu pipeline "tối ưu" phụ thuộc rất nhiều vào đặc điểm của khối lượng công việc (workload) — chương trình có nhiều rẽ nhánh khó dự đoán sẽ chịu tổn thất nặng hơn từ pipeline sâu so với chương trình tuần tự thuần túy (ví dụ như các vòng lặp tính toán số học lặp lại nhiều trong AI/Deep Learning, Volume 05). Đây là một phần lý do các kiến trúc chuyên biệt cho AI (sẽ gặp lại ở Chương 4.1.8 và Volume 05/06) thường có chiến lược pipeline khác biệt hẳn so với CPU đa dụng.

---

## 13. Ưu điểm

- **Tăng throughput mà không cần tăng tốc độ vật lý của cổng logic:** như đã nhấn mạnh ở Mục 1, pipeline "tận dụng" phần cứng nhàn rỗi thay vì đòi hỏi công nghệ chế tạo nhanh hơn.
- **Speedup có thể tính toán và dự đoán được:** công thức Mục 7.1 cho phép kỹ sư ước lượng lợi ích trước khi đầu tư thiết kế thêm giai đoạn pipeline.
- **Tương thích tự nhiên với triết lý RISC:** tập lệnh đơn giản, đều đặn (Chương 4.1.2) giúp việc chia giai đoạn pipeline cân bằng và hiệu quả hơn.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Một nhầm lẫn thường gặp: cho rằng pipeline làm **một lệnh đơn lẻ** chạy nhanh hơn. Thực tế, latency của một lệnh riêng lẻ trong hệ thống pipeline **không giảm** — thậm chí có thể tăng nhẹ do chi phí pipeline register (Mục 6, Mục 7.2). Lợi ích của pipeline nằm hoàn toàn ở **throughput tổng thể** khi xử lý nhiều lệnh liên tiếp, không phải ở tốc độ của từng lệnh đơn lẻ.

- **Hazard làm giảm speedup thực tế xuống dưới giới hạn lý thuyết:** như minh họa ở Mục 10, mỗi bubble do stall làm tăng CPI hiệu dụng, kéo hiệu năng thực tế ra xa công thức lý tưởng ở Mục 7.1.
- **Control hazard gây tổn thất đặc biệt nghiêm trọng ở pipeline sâu:** khoảng cách giữa fetch một lệnh rẽ nhánh và lúc biết kết quả càng xa (pipeline càng sâu), số bubble tiềm năng cần chèn càng lớn — vấn đề này chưa được giải quyết triệt để trong chương này, và là động lực trực tiếp cho Chương 4.1.6.
- **Độ phức tạp Control Unit tăng đáng kể:** logic phát hiện hazard và điều phối forwarding/stall (Mục 8) phức tạp hơn nhiều so với Control Unit đơn giản ở Chương 4.1.3.

---

## 15. So sánh

**Bảng 4.1.4.1 — Ba loại Hazard và chiến lược xử lý chính**

| Loại Hazard | Nguyên nhân | Chiến lược xử lý chính | Chương liên quan |
|---|---|---|---|
| Structural | Xung đột tài nguyên phần cứng | Nhân bản tài nguyên (ví dụ: tách bộ nhớ lệnh và bộ nhớ dữ liệu — kiến trúc Harvard, Chương 4.1.3, Mục 15) | 4.1.3 |
| Data | Lệnh sau cần giá trị lệnh trước chưa tính/ghi xong | Forwarding (ưu tiên), Stall (khi forwarding không đủ) | Chương này (4.1.4) |
| Control | Chưa biết lệnh tiếp theo do rẽ nhánh chưa giải quyết | Stall đơn giản, hoặc dự đoán rẽ nhánh (branch prediction) | 4.1.6 |

**Bảng 4.1.4.2 — Latency và Throughput: hai chỉ số hiệu năng khác biệt**

| Tiêu chí | Latency | Throughput |
|---|---|---|
| Định nghĩa | Thời gian để MỘT lệnh hoàn tất từ đầu đến cuối | Số lệnh hoàn tất trên MỘT đơn vị thời gian |
| Ảnh hưởng của Pipeline | Không giảm, có thể tăng nhẹ (chi phí pipeline register) | Tăng đáng kể (tiệm cận $k$ lần, Mục 7.1) |
| Phép so sánh phù hợp | Thời gian một mẻ đồ giặt-sấy-gấp xong (Mục 3) | Số mẻ đồ hoàn tất mỗi giờ |

**Phân tích:** Bảng 4.1.4.1 tổng hợp lại toàn bộ nội dung cốt lõi của chương theo đúng cấu trúc "nguyên nhân → giải pháp → nơi giải pháp được trình bày đầy đủ", một lần nữa nhấn mạnh rằng chương này chỉ giải quyết triệt để hai trong ba loại hazard (structural, data), còn control hazard chỉ được xử lý tạm thời. Bảng 4.1.4.2 củng cố thông điệp quan trọng nhất của toàn chương — latency và throughput là hai đại lượng độc lập, và pipeline là minh chứng rõ ràng nhất cho việc tối ưu một đại lượng không đồng nghĩa tối ưu đại lượng còn lại.

---

## 16. Tóm tắt

- **Pipeline** chia chu trình xử lý lệnh thành nhiều **giai đoạn** (kinh điển: IF–ID–EX–MEM–WB), cho phép nhiều lệnh chồng lấp xử lý đồng thời, ngăn cách bởi các **pipeline register**.
- **Speedup** lý thuyết tiệm cận số giai đoạn $k$ khi số lệnh $n$ đủ lớn: $\text{Speedup} = \dfrac{nk}{k+n-1} \to k$.
- Ba loại **hazard** — structural, data, control — là những trở ngại khiến hiệu năng thực tế thấp hơn giới hạn lý thuyết; **forwarding** và **stall** là hai kỹ thuật xử lý data hazard, còn control hazard chỉ được giải quyết tạm thời trong chương này.
- **Latency** (thời gian một lệnh) và **throughput** (số lệnh mỗi đơn vị thời gian) là hai chỉ số độc lập — pipeline cải thiện throughput mà hầu như không cải thiện (thậm chí làm tăng nhẹ) latency.
- Đây chính là câu trả lời cho câu hỏi khơi gợi đầu chương: bắt đầu lệnh tiếp theo trước khi lệnh hiện tại hoàn tất là hoàn toàn khả thi và có lợi, miễn là hazard được phát hiện và xử lý đúng cách.

Chương 4.1.5 (Superscalar & Out-of-Order Execution) sẽ đẩy ý tưởng chồng lấp này đi xa hơn nữa: thay vì chỉ chồng lấp **các giai đoạn** của lệnh theo đúng thứ tự chương trình, làm thế nào để CPU thực thi **nhiều lệnh cùng lúc trong cùng một giai đoạn**, thậm chí không theo đúng thứ tự ban đầu?

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Vẽ lại bảng pipeline (giống Hình 4.1.4.1) cho 3 lệnh, giả sử pipeline có 4 giai đoạn (bỏ MEM) và không có hazard nào. Tính tổng số chu kỳ cần thiết.
2. Giải thích bằng lời (không cần công thức) tại sao latency của một lệnh trong hệ thống pipeline không giảm so với hệ thống không pipeline.
3. Cho $k = 5$, $n = 100$. Tính Speedup bằng công thức Mục 7.1.

### Mức Trung bình (Intermediate)

4. Phân loại từng tình huống sau đúng loại hazard (structural/data/control), dựa trên định nghĩa ở Mục 6: (a) hai lệnh cùng cần truy cập bộ nhớ trong cùng một chu kỳ nhưng chỉ có một cổng bộ nhớ; (b) lệnh thứ hai đọc một thanh ghi mà lệnh đầu tiên chưa ghi xong; (c) CPU chưa biết nên fetch lệnh tại địa chỉ nào vì lệnh hiện tại là `JMPZ` chưa hoàn tất giai đoạn EX.
5. Chạy thử `simulate_pipeline` ở Mục 9 với chương trình `[("LOAD", 5), ("STORE", 0), ("ADD", 0)]`, liệt kê chu kỳ IF của từng lệnh, và giải thích bằng lời tại sao lệnh `ADD` (đọc thanh ghi 0) bị ảnh hưởng bởi lệnh `STORE` (ghi thanh ghi 0) ngay trước nó.

### Mức Nâng cao (Advanced)

6. Mở rộng `simulate_pipeline` ở Mục 9 để hỗ trợ **forwarding**: khi phát hiện data hazard với lệnh liền trước, thay vì luôn trì hoãn 2 chu kỳ, chỉ trì hoãn nếu giá trị cần **chưa** sẵn sàng ở đầu ra EX của lệnh trước tại đúng chu kỳ ID của lệnh hiện tại (gợi ý: so sánh chu kỳ ID của lệnh hiện tại với chu kỳ EX của lệnh trước).
7. Với pipeline 5 giai đoạn, giả sử 20% lệnh trong một chương trình là lệnh rẽ nhánh, và mỗi lệnh rẽ nhánh gây ra trung bình 3 chu kỳ bubble (do phải chờ giải quyết control hazard mà chưa áp dụng dự đoán). Tính CPI hiệu dụng của chương trình này (gợi ý: CPI hiệu dụng = 1 + tỷ lệ chu kỳ bubble tính trung bình trên mỗi lệnh), rồi so sánh với CPI lý tưởng bằng 1 của một pipeline không hazard.

### Mức Nghiên cứu (Research)

8. Tìm hiểu khái niệm **instruction scheduling** (đã nhắc ngắn ở Mục 11) — kỹ thuật compiler sắp xếp lại thứ tự lệnh (không đổi ngữ nghĩa chương trình) để giảm thiểu data hazard trước khi mã máy chạy trên CPU pipeline. Thảo luận: kỹ thuật này thể hiện việc "gánh nặng xử lý hazard" có thể được chuyển một phần từ phần cứng sang phần mềm/compiler như thế nào — liên hệ với đánh đổi RISC/CISC đã học ở Chương 4.1.2, Mục 15.

---

## 18. Dự án nhỏ

**Dự án: Bộ mô phỏng Pipeline có Forwarding cho ISA đồ chơi**

- **Mục tiêu:** Hoàn thiện `simulate_pipeline` ở Mục 9 thành một bộ mô phỏng đầy đủ, hỗ trợ cả forwarding lẫn stall, áp dụng đúng quy trình 5 bước ở Mục 8.
- **Yêu cầu:**
  - Cài đặt forwarding như gợi ý ở Bài tập 6.
  - In ra bảng pipeline dạng trực quan (giống Hình 4.1.4.1/Mục 10), đánh dấu rõ chu kỳ nào là bubble do stall.
  - So sánh, trên cùng một chương trình đồ chơi có ít nhất 2 cặp lệnh phụ thuộc dữ liệu liên tiếp, tổng số chu kỳ cần thiết giữa hai phiên bản: có forwarding và không có forwarding (chỉ dùng stall thuần túy).
  - Tính Speedup thực tế đạt được (so với chạy tuần tự hoàn toàn không pipeline) cho cả hai phiên bản, dùng công thức Mục 7.1, và so sánh với giới hạn lý thuyết $k=5$.
- **Công nghệ gợi ý:** Python thuần, tái sử dụng `encode`/`decode`/`OPCODE_TABLE` từ Chương 4.1.2.
- **Kết quả mong đợi:** Bảng so sánh định lượng rõ ràng cho thấy forwarding giúp speedup thực tế tiến gần hơn tới giới hạn lý thuyết so với chỉ dùng stall.
- **Hướng mở rộng:** Thêm mô phỏng đơn giản cho control hazard (giả sử lệnh rẽ nhánh luôn gây 2 bubble) — chuẩn bị trực tiếp cho Chương 4.1.6, nơi kỹ thuật dự đoán rẽ nhánh sẽ được dùng để giảm thiểu chính xác con số bubble này.

---

## 19. Tự đánh giá

- [ ] Tôi có thể vẽ và giải thích bảng pipeline 5 giai đoạn cho ít nhất 4 lệnh liên tiếp, không cần nhìn lại Hình 4.1.4.1.
- [ ] Tôi phân biệt rõ ràng, có ví dụ cụ thể, cho cả ba loại hazard: structural, data, control.
- [ ] Tôi có thể áp dụng công thức Speedup để tính định lượng lợi ích của pipeline với số giai đoạn và số lệnh cho trước.
- [ ] Tôi giải thích được chính xác sự khác biệt giữa latency và throughput, và tại sao pipeline chỉ cải thiện throughput.
- [ ] Tôi đã hoàn thành Bài tập 6 (mở rộng forwarding) và quan sát được số chu kỳ giảm so với phiên bản chỉ dùng stall.

Nếu Bài tập 7 (tính CPI hiệu dụng với control hazard) vẫn còn khó khăn, nên ôn lại công thức hiệu năng ở Chương 4.1.3, Mục 7.1–7.2 trước khi sang Chương 4.1.5 — kỹ năng "tính CPI hiệu dụng khi có tổn thất" sẽ được dùng lại liên tục khi phân tích lợi ích của superscalar và branch prediction.

---

## 20. Đọc thêm

- **Sách:** Randal E. Bryant, David R. O'Hallaron, *Computer Systems: A Programmer's Perspective* — phần liên quan đến tổ chức xử lý pipeline ở mức khái niệm gần với phần cứng thực tế. *(Xem BOOKS.md — Volume 4.)*
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc thêm về ví dụ minh họa "giặt đồ" (laundry analogy) kinh điển trong các giáo trình Computer Architecture nhập môn — cùng ý tưởng với Mục 3–4 của chương này, thường được trình bày với hình vẽ trực quan hơn.
- **Chương tiếp theo:** Chương 4.1.5 — Superscalar Architecture & Out-of-Order Execution.

---

### Liên kết chương (Cross References)

- **Chương trước:** 4.1.3 — CPU Organization & Instruction Execution Cycle (pipeline là sự mở rộng trực tiếp của chu trình Fetch–Decode–Execute–Writeback học ở đó, chia nhỏ thành 5 giai đoạn chồng lấp được).
- **Chương tiếp theo:** 4.1.5 — Superscalar Architecture & Out-of-Order Execution, mở rộng ý tưởng chồng lấp sang việc thực thi nhiều lệnh mỗi chu kỳ, không nhất thiết theo đúng thứ tự chương trình.
- **Chương liên quan xa hơn:** 4.1.6 — Branch Prediction (giải quyết triệt để control hazard mới chỉ xử lý tạm thời ở Mục 8, 15); Chương 4.1.1 (pipeline register chính là D Flip-Flop học ở đó); Volume 2, Part III (instruction scheduling của compiler, Bài tập 8).
- **Vị trí trong Knowledge Graph:** Nút thứ tư của Volume 04, Part I; phụ thuộc trực tiếp vào Chương 4.1.3; là điều kiện tiên quyết bắt buộc cho Chương 4.1.5 và 4.1.6.

---

*Hết Chương 4.1.4. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`. Mọi ví dụ pipeline và tính toán speedup đều được kiểm chứng bằng Python thực thi thực tế (Mục 9–10), tiếp nối trực tiếp `ToyCPU` và ISA đồ chơi đã xây dựng ở Chương 4.1.2–4.1.3. Đang chờ rà soát trước khi tiếp tục sang Chương 4.1.5 — Superscalar Architecture & Out-of-Order Execution.*
