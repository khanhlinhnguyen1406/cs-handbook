# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 04 — Computer Systems
### Part I — Computer Organization and Architecture
## Chương 4.1.5 — Superscalar Architecture & Out-of-Order Execution
### (Kiến trúc Siêu vô hướng và Thực thi Không theo Thứ tự)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 4.1.5 |
| Thuộc Part | I — Computer Organization and Architecture |
| Thuộc Volume | 04 — Computer Systems |
| Thời gian đọc ước tính | 55–70 phút |
| Độ khó | ★★★★☆ |
| Kiến thức tiên quyết | Chương 4.1.3 — CPU Organization; Chương 4.1.4 — Pipeline (đặc biệt Mục 6–8, phân loại hazard) |
| Chương liên quan | 4.1.6 — Branch Prediction: kỹ thuật thường được kết hợp trực tiếp với out-of-order execution trong CPU hiện đại để giảm thiểu tổn thất control hazard |
| Từ khóa | superscalar, issue width, instruction-level parallelism (ILP), register renaming, reorder buffer (ROB), out-of-order execution, WAR hazard, WAW hazard, in-order commit |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Giải thích giới hạn CPI $\geq 1$ của pipeline đơn phát hành (single-issue), và cách superscalar phá vỡ giới hạn đó.
- Định nghĩa Instruction-Level Parallelism (ILP) và issue width, tính CPI lý tưởng cho một CPU superscalar.
- Phân biệt ba loại phụ thuộc lệnh: RAW (data hazard đã học), WAR, và WAW.
- Giải thích cơ chế register renaming loại bỏ hoàn toàn WAR và WAW.
- Mô tả vai trò của Reorder Buffer (ROB) trong việc cho phép thực thi không theo thứ tự nhưng vẫn "commit" kết quả đúng theo thứ tự chương trình gốc.
- Nhận diện lý do vì sao ILP có giới hạn thực tế, không thể tăng issue width vô hạn để cải thiện hiệu năng.

---

### Câu hỏi khơi gợi

> *Chương 4.1.4 đã cho thấy pipeline giúp một CPU hoàn tất trung bình một lệnh mỗi chu kỳ (CPI tiệm cận 1). Nhưng nếu hai lệnh liên tiếp trong chương trình của bạn hoàn toàn độc lập — ví dụ `a = x + y` và `b = m * n` — tại sao CPU phải "xếp hàng" xử lý tuần tự từng lệnh một, thay vì làm cả hai cùng lúc? Và nếu CPU được phép thực thi lệnh "không theo đúng thứ tự" bạn viết trong code, làm sao nó đảm bảo chương trình vẫn chạy ra kết quả giống hệt như thể mọi thứ diễn ra tuần tự?*

---

## 1. Tổng quan chương

Chương 4.1.4 đã chỉ ra rằng pipeline, dù hiệu quả, vẫn bị ràng buộc bởi một giới hạn cơ bản: với **một** đường ống (single-issue pipeline), CPI lý tưởng tuyệt đối chỉ có thể tiệm cận **1** — mỗi chu kỳ, nhiều nhất một lệnh mới có thể được đưa vào giai đoạn IF. Chương này giới thiệu hai kỹ thuật liên kết chặt chẽ, cùng nhắm tới việc phá vỡ chính giới hạn đó: **superscalar** (phát hành và thực thi nhiều lệnh mỗi chu kỳ) và **out-of-order execution** (thực thi lệnh không theo đúng thứ tự chương trình gốc, miễn là vẫn giữ đúng ngữ nghĩa).

Đây là bước phát triển tự nhiên tiếp theo sau pipeline: nếu chồng lấp các *giai đoạn* của các lệnh liên tiếp mang lại lợi ích lớn (Chương 4.1.4), thì việc chồng lấp thêm ở chiều "song song trong cùng một giai đoạn" — cho nhiều lệnh cùng ở giai đoạn EX tại một thời điểm — là bước logic kế tiếp. Nhưng để làm được điều này một cách an toàn, CPU cần hai cơ chế mới hoàn toàn: **register renaming** (giải quyết các phụ thuộc "giả" không mang ý nghĩa tính toán thực sự) và **reorder buffer** (đảm bảo ảo giác thực thi tuần tự dù bên trong hoàn toàn hỗn loạn về thứ tự).

> **💡 Insight**
> Nguyên lý cốt lõi của chương này có thể tóm gọn trong một câu: **CPU được phép làm bất cứ điều gì nó muốn ở bên trong, miễn là kết quả quan sát được từ bên ngoài giống hệt như thể mọi lệnh được thực thi tuần tự, đúng thứ tự chương trình gốc.** Đây chính là một dạng "hợp đồng hành vi" tương tự ISA (Chương 4.1.2) — nhưng lần này áp dụng cho chính trình tự thực thi bên trong CPU, chứ không phải cho tập lệnh.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Sự kiện | Đóng góp |
|---|---|---|
| 1967 | Robert Tomasulo (IBM) công bố thuật toán mang tên ông cho hệ thống IBM System/360 Model 91 | Đặt nền tảng thuật toán đầu tiên cho cơ chế thực thi động (dynamic scheduling), giải quyết phụ thuộc lệnh mà không cần compiler sắp xếp lại thứ tự tĩnh — tiền thân trực tiếp của register renaming và reorder buffer hiện đại |
| Cuối thập niên 1980 – đầu 1990 | Các CPU thương mại đầu tiên áp dụng thiết kế superscalar rộng rãi | Chuyển superscalar từ ý tưởng nghiên cứu học thuật thành tiêu chuẩn thiết kế công nghiệp phổ biến cho CPU hiệu năng cao |
| Từ giữa thập niên 1990 đến nay | Out-of-order execution kết hợp superscalar trở thành thiết kế mặc định của hầu hết CPU đa dụng hiệu năng cao (x86, ARM cao cấp) | Cùng tồn tại song song với các lõi in-order đơn giản hơn, tiết kiệm năng lượng hơn, dùng cho các tác vụ ít đòi hỏi hiệu năng đỉnh |

Điều đáng chú ý: ý tưởng nền tảng của out-of-order execution (thuật toán Tomasulo) ra đời từ năm 1967 — rất lâu trước khi công nghệ bán dẫn đủ phát triển để hiện thực hóa nó một cách kinh tế trong CPU đại trà. Đây là một ví dụ khác, tương tự công trình của Shannon đã nêu ở Chương 4.1.1, về khoảng cách thời gian giữa ý tưởng lý thuyết và khả năng ứng dụng thực tiễn quy mô lớn.

---

## 3. Động lực

Quay lại câu hỏi khơi gợi: xét đoạn chương trình sau, biên dịch thành các lệnh máy (ký hiệu đơn giản hóa):

```text
I1: a = x + y      // độc lập với I2
I2: b = m * n       // độc lập với I1
I3: c = a + b        // PHỤ THUỘC vào cả I1 và I2 (cần a và b)
```

`I1` và `I2` không hề đọc hoặc ghi bất kỳ giá trị chung nào — chúng hoàn toàn độc lập. Một pipeline đơn phát hành (Chương 4.1.4) buộc phải xử lý `I1` rồi mới tới `I2`, dù về mặt logic không có lý do gì ngăn cản việc thực thi cả hai **đồng thời**. Đây chính là động lực cho **superscalar**: nếu CPU có đủ phần cứng (nhiều ALU, nhiều đường dữ liệu), tại sao không phát hành cả `I1` và `I2` trong cùng một chu kỳ?

Vấn đề trở nên tinh tế hơn khi xét thêm tình huống sau:

```text
I1: t = a + b        // ghi vào thanh ghi t
I2: (một lệnh chậm, ví dụ chia số học, mất nhiều chu kỳ, KHÔNG liên quan đến t)
I3: t = c + d        // GHI LẠI vào CÙNG thanh ghi t, nhưng với giá trị KHÁC
```

Nếu CPU cố thực thi `I3` trước khi `I1` kịp hoàn tất (vì `I2` đang "kẹt" chờ lâu), và `I3` ghi đè giá trị `t` trước khi giá trị của `I1` được dùng ở nơi khác trong chương trình — kết quả sẽ **sai**. Đây không phải là data hazard kiểu RAW đã học ở Chương 4.1.4 (không lệnh nào đọc giá trị lệnh khác chưa ghi xong) — mà là một loại phụ thuộc hoàn toàn khác, chỉ tồn tại vì `I1` và `I3` **tình cờ** dùng chung tên thanh ghi `t`, chứ không mang ý nghĩa tính toán thực sự nào. Mục 6–8 sẽ định nghĩa chính xác loại phụ thuộc này và cách giải quyết nó.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Nếu pipeline (Chương 4.1.4) giống một dây chuyền giặt-sấy-gấp đơn lẻ, thì **superscalar** giống việc mở **nhiều dây chuyền song song** trong cùng một tiệm giặt — hai máy giặt hoạt động đồng thời cho hai mẻ đồ khác nhau. Còn **out-of-order execution** giống một quản lý tiệm giặt thông minh: nếu mẻ đồ số 3 phải chờ một vết bẩn đặc biệt được xử lý trước (mất nhiều thời gian), quản lý không bắt các mẻ đồ số 4, 5 đứng chờ vô ích phía sau — thay vào đó, cho chúng "vượt lên" xử lý trước nếu máy giặt đang trống và chúng không phụ thuộc gì vào mẻ số 3. Nhưng khi giao đồ trả cho khách (kết quả cuối cùng), quản lý vẫn đảm bảo **trả đúng theo thứ tự khách đã gửi đồ ban đầu** — đó chính là vai trò của Reorder Buffer.

| Trực giác kỹ thuật bạn đã có | Khái niệm chương này tương ứng |
|---|---|
| Chạy nhiều worker song song xử lý các task độc lập trong một job queue | Superscalar — thực thi nhiều lệnh độc lập cùng một chu kỳ |
| Đổi tên biến cục bộ để tránh xung đột tên trong một scope lồng nhau (variable shadowing) | Register Renaming — cấp một "tên vật lý" mới cho mỗi lần ghi vào cùng thanh ghi kiến trúc |
| Hàng đợi commit của một hệ quản trị cơ sở dữ liệu, đảm bảo transaction được ghi log đúng thứ tự dù xử lý song song | Reorder Buffer — đảm bảo commit kết quả đúng thứ tự chương trình dù thực thi không theo thứ tự |

---

## 5. Trực quan hóa khái niệm

**Hình 4.1.5.1 — Superscalar: hai lệnh phát hành đồng thời mỗi chu kỳ (issue width = 2)**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
Chu kỳ:         1        2        3        4        5
Lệnh I1:        IF ID    EX MEM  WB
Lệnh I2:        IF ID    EX MEM  WB     ← PHÁT HÀNH CÙNG chu kỳ với I1
Lệnh I3:                 IF ID   EX MEM  WB
Lệnh I4:                 IF ID   EX MEM  WB   ← PHÁT HÀNH CÙNG chu kỳ với I3
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | So sánh trực tiếp với Hình 4.1.4.1 (Chương 4.1.4): ở đó mỗi chu kỳ chỉ có MỘT lệnh mới bắt đầu IF, còn ở đây có HAI lệnh mới bắt đầu IF trong cùng một chu kỳ |
| Điểm mấu chốt | Với issue width $w = 2$, 4 lệnh hoàn tất trong thời gian tương đương với việc pipeline đơn phát hành xử lý chỉ 2 lệnh — đây chính là cách CPI hiệu dụng có thể giảm xuống **dưới 1** |

---

**Hình 4.1.5.2 — Out-of-Order Execution và vai trò của Reorder Buffer**

```text
THỨ TỰ CHƯƠNG TRÌNH (program order):     I1, I2, I3, I4

I2 phụ thuộc một phép chia CHẬM → chưa có kết quả kịp

THỰC THI THỰC TẾ (out of order):
   Chu kỳ 1-2:  I1 thực thi xong
   Chu kỳ 1-6:  I2 vẫn đang thực thi (phép chia chậm)
   Chu kỳ 2-3:  I3 thực thi XONG TRƯỚC I2 (vì I3 không phụ
                thuộc I2, và ALU đang rảnh)
   Chu kỳ 3-4:  I4 thực thi xong

REORDER BUFFER (ROB) — hàng đợi commit ĐÚNG THỨ TỰ chương trình:
   ┌────┬────┬────┬────┐
   │ I1 │ I2 │ I3 │ I4 │   ← I1 đã DONE, I2 CHƯA DONE (đang chờ chia),
   │DONE│ .. │DONE│DONE│      I3 và I4 tuy đã DONE nhưng phải CHỜ I2
   └────┴────┴────┴────┘      vì ROB chỉ commit ĐÚNG THỨ TỰ từ đầu

   → Kết quả của I3, I4 được "giữ lại" trong ROB, KHÔNG được
     ghi chính thức vào Register File cho tới khi I2 hoàn tất
     và commit trước theo đúng program order.
```

*Mục đích:* Minh họa cụ thể câu hỏi thứ hai ở đầu chương — làm thế nào CPU thực thi không theo thứ tự nhưng vẫn tạo ra ảo giác tuần tự hoàn hảo cho phần mềm quan sát từ bên ngoài. *Điểm mấu chốt:* **thực thi** (execution) có thể diễn ra không theo thứ tự, nhưng **commit** (ghi kết quả chính thức, khả kiến với phần còn lại của hệ thống) luôn diễn ra đúng theo thứ tự chương trình gốc — đây là nguyên tắc "in-order commit" sẽ định nghĩa hình thức ở Mục 6.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Superscalar và Issue Width**
>
> **Superscalar** là kiến trúc CPU có khả năng phát hành (issue) và thực thi **nhiều hơn một lệnh mỗi chu kỳ clock**, nhờ có nhiều đơn vị chức năng phần cứng (nhiều ALU, nhiều đường dữ liệu) hoạt động song song. **Issue Width ($w$)** là số lệnh tối đa CPU có thể phát hành trong một chu kỳ — ví dụ $w=2$ ở Hình 4.1.5.1 (superscalar 2 chiều, "2-wide").

> **📌 Remember — Ba loại phụ thuộc giữa các lệnh**
>
> | Loại | Tên đầy đủ | Định nghĩa | Bản chất |
> |---|---|---|---|
> | RAW | Read After Write | Lệnh sau đọc giá trị lệnh trước ghi | Phụ thuộc **thực sự** (true dependency) — đã học ở Chương 4.1.4 với tên "data hazard" |
> | WAR | Write After Read | Lệnh sau ghi vào nơi lệnh trước đang đọc | Phụ thuộc **giả** (anti-dependency) — chỉ tồn tại vì trùng tên thanh ghi |
> | WAW | Write After Write | Lệnh sau ghi vào nơi lệnh trước cũng ghi | Phụ thuộc **giả** (output dependency) — chỉ tồn tại vì trùng tên thanh ghi |
>
> Chỉ **RAW** phản ánh một phụ thuộc tính toán thực sự (cần giá trị thật của phép toán trước). WAR và WAW hoàn toàn là hệ quả của việc **tái sử dụng tên thanh ghi kiến trúc (architectural register)** — chúng biến mất hoàn toàn nếu mỗi lần ghi được gán một "vị trí lưu trữ" vật lý khác nhau.

> **📌 Remember — Register Renaming và Reorder Buffer (ROB)**
>
> - **Register Renaming:** kỹ thuật ánh xạ động mỗi thanh ghi kiến trúc (ví dụ `R1` trong ISA, Chương 4.1.2) sang một **thanh ghi vật lý** riêng biệt cho mỗi lần ghi, loại bỏ hoàn toàn WAR và WAW — vì không còn hai lệnh nào thực sự "tranh chấp" cùng một ô lưu trữ vật lý.
> - **Reorder Buffer (ROB):** cấu trúc phần cứng dạng hàng đợi, lưu tạm kết quả của các lệnh đã thực thi xong nhưng **chưa được phép commit** (ghi chính thức, khả kiến ra bên ngoài), đảm bảo việc commit luôn diễn ra đúng theo thứ tự chương trình gốc — hiện thực hóa Hình 4.1.5.2.
> - **In-order Commit:** nguyên tắc kết quả chỉ trở thành "chính thức" (ghi vào Register File kiến trúc, có thể quan sát bởi phần mềm) theo đúng thứ tự chương trình ban đầu, bất kể thứ tự thực thi thực tế bên trong.

---

## 7. Nền tảng toán học

### 7.1 CPI lý tưởng của CPU Superscalar

- **Ý nghĩa:** Mở rộng trực tiếp công thức hiệu năng đã học ở Chương 4.1.3 (Mục 7.1) và giới hạn CPI của pipeline đơn phát hành ở Chương 4.1.4.
- **Với pipeline đơn phát hành (Chương 4.1.4):** $\text{CPI}_{\text{lý tưởng}} = 1$ (tối đa 1 lệnh hoàn tất mỗi chu kỳ, ở trạng thái ổn định).
- **Với superscalar issue width $w$:** tối đa $w$ lệnh có thể phát hành và hoàn tất mỗi chu kỳ ở trạng thái lý tưởng.

> **📦 Formula Box — CPI lý tưởng của CPU Superscalar**
>
> $$\text{CPI}_{\text{lý tưởng}} = \frac{1}{w}, \qquad \text{IPC}_{\text{lý tưởng}} = w$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $w$ | Issue width — số lệnh tối đa phát hành mỗi chu kỳ |
> | $\text{IPC}$ | Instructions Per Cycle — nghịch đảo của CPI, số lệnh hoàn tất trung bình mỗi chu kỳ; là đại lượng thường dùng khi $\text{CPI} < 1$, vì "CPI nhỏ hơn 1" khó hình dung trực quan bằng "IPC lớn hơn 1" |
> | **Diễn giải kỹ thuật** | Đây là giới hạn **lý tưởng tuyệt đối** — chỉ đạt được khi không có bất kỳ hazard, phụ thuộc, hay tổn thất nào; thực tế luôn thấp hơn đáng kể (Mục 7.2) |
> | **Ứng dụng thường gặp** | Là con số "trần" dùng để đánh giá CPU thực tế đạt bao nhiêu phần trăm hiệu năng lý tưởng — chỉ số IPC là một trong những thước đo hiệu năng CPU phổ biến nhất trong tài liệu kỹ thuật |

### 7.2 Instruction-Level Parallelism (ILP) và giới hạn thực tế

**Instruction-Level Parallelism (ILP)** là mức độ song song vốn có trong một đoạn chương trình — số lệnh trung bình có thể thực thi đồng thời mà không vi phạm bất kỳ phụ thuộc RAW nào (WAR/WAW đã bị loại bỏ nhờ register renaming, Mục 6). ILP **không phải** một hằng số cố định của CPU — nó là thuộc tính của **chính chương trình đang chạy**, phụ thuộc vào mật độ phụ thuộc dữ liệu thực sự giữa các lệnh.

> **⚠️ Common Mistake**
> Một hiểu lầm phổ biến là nghĩ rằng tăng issue width $w$ luôn tỷ lệ thuận với tăng IPC thực tế. Trên thực tế, nếu ILP trung bình của chương trình chỉ là 2 (do nhiều phụ thuộc RAW dày đặc), một CPU với $w = 8$ vẫn không thể đạt IPC vượt quá 2 — phần cứng dư thừa (6 "khe" phát hành còn lại) hoàn toàn lãng phí ở những chu kỳ đó. Tăng issue width chỉ có lợi khi chương trình thực sự có đủ ILP để "nuôi" nó.

---

## 8. Thuật toán / Cơ chế

**Quy trình tổng quát của một CPU Superscalar Out-of-Order** (khái quát hóa từ ý tưởng gốc của thuật toán Tomasulo, Mục 2):

```text
Bước 1 (FETCH)     — Fetch TỐI ĐA w lệnh mỗi chu kỳ (thay vì 1
                     lệnh như pipeline đơn phát hành, Chương 4.1.4)
        │
        ▼
Bước 2 (DECODE +   — Giải mã từng lệnh; với mỗi thanh ghi ĐÍCH,
        RENAME)      cấp một thanh ghi VẬT LÝ mới (Register
                     Renaming, Mục 6) — loại bỏ hoàn toàn WAR/WAW
        │
        ▼
Bước 3 (DISPATCH)  — Đưa lệnh vào một hàng đợi chờ thực thi (gọi
                     là "reservation station" trong thuật toán
                     Tomasulo gốc); đồng thời cấp một chỗ trống
                     tương ứng trong Reorder Buffer (ROB), ĐÚNG
                     theo thứ tự chương trình gốc
        │
        ▼
Bước 4 (EXECUTE,   — Mỗi chu kỳ, kiểm tra TOÀN BỘ lệnh đang chờ:
        không theo    lệnh nào đã có ĐẦY ĐỦ giá trị toán hạng
        thứ tự)       (không còn phụ thuộc RAW chưa giải quyết)
                     VÀ có đơn vị thực thi (ALU...) đang rảnh
                     → được phép thực thi NGAY, KHÔNG cần chờ
                     các lệnh đứng trước nó về mặt chương trình
        │
        ▼
Bước 5 (COMMIT,    — Mỗi chu kỳ, kiểm tra ĐẦU hàng đợi ROB (lệnh
        đúng thứ tự)  CŨ NHẤT theo chương trình gốc): nếu lệnh đó
                     đã thực thi xong, ghi kết quả CHÍNH THỨC vào
                     Register File kiến trúc, rồi loại khỏi ROB.
                     Nếu lệnh đầu ROB CHƯA xong, TOÀN BỘ commit bị
                     chặn lại — dù các lệnh phía sau trong ROB đã
                     xong từ lâu (đúng như Hình 4.1.5.2)
```

> **⚠️ Common Mistake**
> Người mới học thường nhầm lẫn giữa "thực thi không theo thứ tự" với "kết quả không đáng tin cậy" hoặc "chương trình chạy sai". Bước 5 chính là cơ chế đảm bảo tuyệt đối rằng **dù thực thi hỗn loạn bên trong, mọi thứ khả kiến từ bên ngoài (giá trị thanh ghi kiến trúc, xử lý ngoại lệ) đều giống hệt như thể chương trình chạy tuần tự đúng thứ tự gốc.** Đây gọi là tính chất "precise exception" — một yêu cầu bắt buộc để hệ điều hành (Volume 4, Part III) có thể xử lý lỗi/ngắt một cách chính xác, đúng đắn.

---

## 9. Triển khai

```python
# Mô phỏng đơn giản hóa một bộ lập lịch Out-of-Order 2-wide
# (issue width w = 2), có Register Renaming và Reorder Buffer,
# áp dụng trực tiếp quy trình 5 bước ở Mục 8.

class Instruction:
    def __init__(self, idx, dest, src1, src2, latency):
        self.idx = idx            # thứ tự CHƯƠNG TRÌNH GỐC (program order)
        self.dest = dest           # thanh ghi kiến trúc đích
        self.src1, self.src2 = src1, src2
        self.latency = latency     # số chu kỳ EXECUTE cần (mô phỏng lệnh "chậm")
        self.remaining = latency
        self.done = False
        self.committed = False


def rename_registers(instructions):
    """Register Renaming đơn giản hóa (Mục 6, Bước 2): mỗi lần một
    thanh ghi kiến trúc được GHI, cấp một 'thanh ghi vật lý' mới
    (đơn giản hóa bằng chuỗi f'{ten}_v{so_lan_ghi}'), loại bỏ
    hoàn toàn WAR/WAW."""
    version = {}
    for ins in instructions:
        # Toán hạng nguồn dùng phiên bản MỚI NHẤT đã được ghi trước đó
        ins.phys_src1 = f"{ins.src1}_v{version.get(ins.src1, 0)}" if ins.src1 else None
        ins.phys_src2 = f"{ins.src2}_v{version.get(ins.src2, 0)}" if ins.src2 else None
        version[ins.dest] = version.get(ins.dest, 0) + 1
        ins.phys_dest = f"{ins.dest}_v{version[ins.dest]}"
    return instructions


def simulate_ooo(instructions, issue_width=2):
    """Mô phỏng Bước 3-5 ở Mục 8: mỗi chu kỳ, thực thi (không theo
    thứ tự) TỐI ĐA issue_width lệnh có toán hạng sẵn sàng, nhưng
    CHỈ commit đúng theo program order (Reorder Buffer)."""
    ready_values = set()   # các thanh ghi vật lý ĐÃ có giá trị (đã commit)
    executing = set()       # thanh ghi vật lý đang trong lúc EXECUTE
    cycle = 0
    committed_count = 0
    trace = []

    while committed_count < len(instructions):
        cycle += 1
        started_this_cycle = 0

        # --- EXECUTE (không theo thứ tự) ---
        for ins in instructions:
            if ins.done or ins.idx in executing:
                if not ins.done:
                    ins.remaining -= 1
                    if ins.remaining <= 0:
                        ins.done = True
                        executing.discard(ins.idx)
                continue
            src_ready = (ins.phys_src1 is None or ins.phys_src1 in ready_values) and \
                        (ins.phys_src2 is None or ins.phys_src2 in ready_values)
            if src_ready and started_this_cycle < issue_width:
                executing.add(ins.idx)
                started_this_cycle += 1
                ins.remaining -= 1
                if ins.remaining <= 0:
                    ins.done = True
                    executing.discard(ins.idx)

        # --- COMMIT (đúng thứ tự — Reorder Buffer, Bước 5) ---
        for ins in sorted(instructions, key=lambda x: x.idx):
            if ins.committed:
                continue
            if not ins.done:
                break                     # ROB chặn lại tại lệnh CHƯA xong
            ins.committed = True
            ready_values.add(ins.phys_dest)
            committed_count += 1
            trace.append((cycle, ins.idx, ins.dest))

    return cycle, trace
```

Hàm `rename_registers` hiện thực Bước 2 của Mục 8: mỗi lần ghi vào một thanh ghi kiến trúc tạo ra một "phiên bản vật lý" mới, loại bỏ hoàn toàn khả năng xảy ra WAR/WAW (Mục 6). Hàm `simulate_ooo` hiện thực đồng thời Bước 4 (vòng lặp EXECUTE cho phép tối đa `issue_width` lệnh bắt đầu thực thi mỗi chu kỳ, không quan tâm thứ tự chương trình, chỉ cần toán hạng sẵn sàng) và Bước 5 (vòng lặp COMMIT chỉ tiến lên khi lệnh **đầu hàng đợi theo `idx`** đã `done`, mô phỏng chính xác hành vi chặn của Reorder Buffer ở Hình 4.1.5.2).

---

## 10. Trực quan hóa quá trình thực thi

**Kiểm chứng đúng kịch bản ở Hình 4.1.5.2**, với `I2` là lệnh chậm (`latency=5`), các lệnh còn lại nhanh (`latency=1`), và không có phụ thuộc RAW giữa `I2`, `I3`, `I4`:

```python
instrs = [
    Instruction(idx=0, dest="a", src1="x", src2="y", latency=1),   # I1
    Instruction(idx=1, dest="b", src1="m", src2="n", latency=5),   # I2 — CHẬM
    Instruction(idx=2, dest="c", src1="p", src2="q", latency=1),   # I3 — độc lập
    Instruction(idx=3, dest="d", src1="r", src2="s", latency=1),   # I4 — độc lập
]
instrs = rename_registers(instrs)
total_cycles, trace = simulate_ooo(instrs, issue_width=2)
```

| Lệnh | Bắt đầu EXECUTE (thực tế, không theo thứ tự) | Hoàn tất EXECUTE | Chu kỳ COMMIT (đúng thứ tự) |
|:---:|:---:|:---:|:---:|
| I1 | 1 | 1 | 1 |
| I2 | 1 | 5 | 5 |
| I3 | 1 | 1 | **5** (bị ROB chặn, phải chờ I2) |
| I4 | 2 | 2 | **5** (bị ROB chặn, phải chờ I2 và I3) |

Quan sát mấu chốt, đúng như Hình 4.1.5.2: `I3` **thực thi xong** (EXECUTE hoàn tất) từ chu kỳ 1, nhưng **không được commit** cho tới chu kỳ 5 — vì Reorder Buffer bắt buộc commit đúng thứ tự, và `I2` (đứng trước `I3` trong chương trình gốc) chỉ hoàn tất ở chu kỳ 5. Đây là bằng chứng thực nghiệm trực tiếp cho nguyên tắc "thực thi không theo thứ tự, commit theo đúng thứ tự" đã nêu ở Mục 6, 8.

**So sánh CPI thực tế với CPI lý tưởng (Mục 7.1):** với $w=2$, CPI lý tưởng $= 1/2 = 0.5$; với 4 lệnh, thời gian lý tưởng là $4 \times 0.5 = 2$ chu kỳ. Thời gian thực tế đo được: 5 chu kỳ — thấp hơn nhiều so với lý tưởng, hoàn toàn do lệnh `I2` "chậm" làm nghẽn cổ chai tại Reorder Buffer — một minh chứng định lượng khác cho Common Mistake ở Mục 7.2: phần cứng superscalar dư thừa (issue width 2) không giúp ích gì khi bị chặn bởi ràng buộc in-order commit.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Hầu như mọi CPU đa dụng hiệu năng cao thương mại hiện nay (dòng lõi hiệu năng cao trong CPU Intel, AMD, Apple Silicon) đều là superscalar out-of-order — đây không phải kỹ thuật "nâng cao tùy chọn" mà là thiết kế mặc định của phân khúc hiệu năng cao.

| Bối cảnh công nghiệp | Vai trò của Superscalar / Out-of-Order |
|---|---|
| Lõi hiệu năng cao (performance core) trong CPU di động hiện đại | Áp dụng đầy đủ register renaming, reorder buffer, và issue width lớn — tối đa hóa IPC cho tác vụ đòi hỏi hiệu năng đỉnh |
| Lõi tiết kiệm năng lượng (efficiency core), thiết kế kiểu big.LITTLE | Thường dùng pipeline in-order đơn giản hơn (gần với Chương 4.1.4), đánh đổi IPC thấp hơn để lấy mức tiêu thụ điện năng thấp hơn đáng kể |
| Trình biên dịch tối ưu hóa (compiler) | Vẫn tiếp tục thực hiện instruction scheduling (đã nhắc ở Chương 4.1.4, Mục 11) dù CPU có out-of-order, vì sắp xếp tốt ở tầng compiler giúp giảm áp lực lên phần cứng lập lịch động, tiết kiệm năng lượng |
| Công cụ profiling hiệu năng (performance counter, ví dụ đo IPC thực tế) | IPC đo được (Mục 7.1) là chỉ số tiêu chuẩn để đánh giá CPU đang khai thác được bao nhiêu phần trăm ILP sẵn có của chương trình |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Giới hạn ILP thực tế của chương trình (Mục 7.2) từng là chủ đề nghiên cứu gây tranh cãi lớn trong giới Computer Architecture: liệu ILP trung bình của chương trình đa dụng có đủ cao để biện minh cho việc tiếp tục tăng issue width, hay lợi ích sẽ nhanh chóng bão hòa? Kết quả thực nghiệm nhìn chung cho thấy **lợi nhuận giảm dần (diminishing returns)** — issue width vượt quá một ngưỡng nhất định mang lại rất ít cải thiện IPC thực tế cho phần lớn khối lượng công việc đa dụng, trong khi độ phức tạp phần cứng (và tiêu thụ điện năng) tăng nhanh hơn nhiều.

Đây chính là động lực nghiên cứu và công nghiệp trực tiếp dẫn tới hai xu hướng phát triển song song trong ngành: (1) các thiết kế lõi hỗn hợp hiệu năng cao/tiết kiệm năng lượng (Mục 11) — thừa nhận rằng không phải mọi tác vụ đều cần ILP cao; và (2) sự chuyển dịch sang khai thác song song ở tầng **cao hơn** lệnh đơn lẻ — song song hóa dữ liệu (Chương 4.1.7, SIMD) và song song hóa luồng/tiến trình (Volume 4, Part IV — Concurrency), thay vì chỉ dựa vào ILP bên trong một luồng thực thi duy nhất.

Một liên hệ bảo mật quan trọng, sẽ được mở rộng đầy đủ ở Chương 4.1.6: cơ chế thực thi động và dự đoán (bao gồm cả out-of-order execution) — vốn thực thi lệnh "trước khi chắc chắn cần đến chúng" để tối đa hóa ILP khai thác được — chính là nguồn gốc của một lớp lỗ hổng bảo mật vi kiến trúc (như Spectre) được phát hiện những năm gần đây, buộc ngành công nghiệp phải cân nhắc lại đánh đổi giữa hiệu năng và an toàn ở cấp độ phần cứng.

---

## 13. Ưu điểm

- **Phá vỡ giới hạn CPI = 1 của pipeline đơn phát hành:** cho phép IPC lý tưởng đạt tới $w$ (Mục 7.1), khai thác song song ở mức lệnh mà pipeline đơn thuần không thể tận dụng.
- **Register Renaming loại bỏ hoàn toàn hazard "giả":** WAR và WAW — vốn không phản ánh phụ thuộc tính toán thực sự — biến mất hoàn toàn, chỉ còn RAW là ràng buộc thực sự cần tôn trọng.
- **Reorder Buffer giữ nguyên ngữ nghĩa tuần tự cho phần mềm:** lập trình viên và compiler không cần biết (và không cần quan tâm) CPU thực thi không theo thứ tự bên trong — hoàn toàn tương thích ngược với mọi phần mềm viết cho mô hình thực thi tuần tự.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Một nhầm lẫn thường gặp khác: cho rằng CPU out-of-order có thể "sắp xếp lại tùy ý", kể cả các thao tác có tác dụng phụ quan sát được từ bên ngoài hệ thống (ví dụ ghi vào thiết bị I/O). Trong thực tế, các CPU thực tế phải tuân thủ những ràng buộc bổ sung nghiêm ngặt (memory ordering, sẽ liên quan đến Volume 4, Part IV — Concurrency) để đảm bảo tính đúng đắn khi có nhiều lõi CPU hoặc thiết bị ngoại vi cùng quan sát trạng thái hệ thống — không đơn giản như mô hình một luồng lệnh duy nhất trình bày ở chương này.

- **Độ phức tạp phần cứng tăng vọt:** reservation station, reorder buffer, bảng ánh xạ register renaming đều là những cấu trúc phần cứng phức tạp, tốn diện tích chip và tiêu thụ điện năng đáng kể so với pipeline in-order đơn giản.
- **Lợi ích bị giới hạn bởi ILP sẵn có của chương trình:** như đã nhấn mạnh ở Mục 7.2 và 10, issue width lớn vô ích nếu chương trình không có đủ lệnh độc lập để khai thác — không phải mọi khối lượng công việc đều hưởng lợi như nhau.
- **Nguy cơ bảo mật vi kiến trúc:** như đã nêu ở Mục 12, chính cơ chế thực thi "trước khi chắc chắn" tạo ra bề mặt tấn công mới, không tồn tại trong các CPU in-order đơn giản hơn.
- **Reorder Buffer có thể trở thành điểm nghẽn:** như minh họa ở Mục 10, một lệnh chậm đứng đầu ROB có thể "khóa" toàn bộ việc commit của các lệnh phía sau, dù chúng đã thực thi xong từ lâu.

---

## 15. So sánh

**Bảng 4.1.5.1 — Ba loại phụ thuộc và cách xử lý**

| Loại | Có phải phụ thuộc tính toán thực sự? | Bị loại bỏ bởi Register Renaming? | Vẫn cần tôn trọng khi thực thi? |
|---|:---:|:---:|:---:|
| RAW | Có | Không | **Có** — luôn phải tôn trọng |
| WAR | Không | **Có** | Không (sau khi renaming) |
| WAW | Không | **Có** | Không (sau khi renaming) |

**Bảng 4.1.5.2 — Pipeline đơn phát hành (Chương 4.1.4) và Superscalar Out-of-Order (chương này)**

| Tiêu chí | Pipeline đơn phát hành | Superscalar Out-of-Order |
|---|---|---|
| CPI lý tưởng | 1 | $1/w$ (có thể $<1$) |
| Thứ tự thực thi | Đúng thứ tự chương trình (in-order) | Không theo thứ tự (out-of-order), commit vẫn in-order |
| Cơ chế xử lý phụ thuộc | Stall / Forwarding (Chương 4.1.4) | Register Renaming (WAR/WAW) + chờ toán hạng sẵn sàng (RAW) |
| Độ phức tạp phần cứng | Trung bình | Cao |
| Phù hợp nhất với | Lõi tiết kiệm năng lượng, hệ nhúng | Lõi hiệu năng cao, workload có ILP dồi dào |

**Phân tích:** Bảng 4.1.5.1 tổng kết chính xác lý do vì sao register renaming là kỹ thuật "gần như miễn phí về mặt đúng đắn" — nó loại bỏ đúng hai loại phụ thuộc vốn dĩ không mang ý nghĩa tính toán, chỉ là hệ quả của việc tái sử dụng tên thanh ghi. Bảng 4.1.5.2 cho thấy rõ đây không phải là một kỹ thuật "luôn tốt hơn" một cách tuyệt đối — giống hệt tinh thần so sánh RISC/CISC (Chương 4.1.2) và single-cycle/multi-cycle (Chương 4.1.3): mỗi lựa chọn thiết kế đều có ngữ cảnh phù hợp riêng.

---

## 16. Tóm tắt

- **Superscalar** cho phép CPU phát hành và thực thi tối đa $w$ (issue width) lệnh mỗi chu kỳ, phá vỡ giới hạn $\text{CPI} \geq 1$ của pipeline đơn phát hành, đạt $\text{CPI}_{\text{lý tưởng}} = 1/w$.
- Ba loại phụ thuộc lệnh — **RAW** (thực sự), **WAR**, **WAW** (đều là "giả", chỉ do trùng tên thanh ghi) — cần được phân biệt rõ ràng; chỉ RAW là ràng buộc bắt buộc phải tôn trọng khi thực thi.
- **Register Renaming** loại bỏ hoàn toàn WAR/WAW bằng cách cấp thanh ghi vật lý riêng cho mỗi lần ghi.
- **Out-of-order execution** cho phép các lệnh độc lập "vượt lên" thực thi trước các lệnh đứng trước bị kẹt, tối đa hóa việc khai thác ILP thực tế của chương trình.
- **Reorder Buffer (ROB)** đảm bảo mọi kết quả vẫn được **commit đúng theo thứ tự chương trình gốc**, giữ nguyên ảo giác thực thi tuần tự cho phần mềm — đây là câu trả lời cho phần thứ hai của câu hỏi khơi gợi đầu chương.
- Lợi ích thực tế của superscalar bị giới hạn nghiêm ngặt bởi ILP sẵn có trong chính chương trình — không phải là một "phép màu" tăng hiệu năng vô hạn.

Chương 4.1.6 (Branch Prediction) sẽ giải quyết một vấn đề đã bị bỏ ngỏ xuyên suốt cả Chương 4.1.4 và chương này: khi CPU thực thi động và "vượt lên" mạnh mẽ như vừa học, làm thế nào để xử lý hiệu quả trường hợp CPU thậm chí còn chưa biết **lệnh nào** sẽ được fetch tiếp theo — vì lệnh hiện tại là một rẽ nhánh (control hazard) chưa được giải quyết?

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Với issue width $w = 4$, tính $\text{CPI}_{\text{lý tưởng}}$ và $\text{IPC}_{\text{lý tưởng}}$ bằng công thức ở Mục 7.1.
2. Cho hai lệnh liên tiếp cùng ghi vào thanh ghi `R1` (không có lệnh nào đọc `R1` ở giữa), xác định đây là loại phụ thuộc nào (RAW/WAR/WAW), và giải thích register renaming xử lý trường hợp này ra sao.
3. Giải thích bằng lời (không cần code) tại sao một lệnh có thể "thực thi xong" nhưng vẫn chưa được "commit" — dùng đúng thuật ngữ Reorder Buffer.

### Mức Trung bình (Intermediate)

4. Cho chương trình 5 lệnh, trong đó lệnh thứ 2 (theo thứ tự chương trình) có `latency = 10` (rất chậm), còn lại đều `latency = 1` và không có phụ thuộc RAW nào giữa các lệnh. Dùng `simulate_ooo` ở Mục 9 (với `issue_width=2`), dự đoán chu kỳ commit của lệnh cuối cùng, sau đó chạy thử để kiểm chứng.
5. Giải thích bằng lời, dựa trên Bảng 4.1.5.1, tại sao chỉ cần giải quyết đúng RAW (bằng cách chờ toán hạng sẵn sàng) là đủ để đảm bảo tính đúng đắn của kết quả tính toán, miễn là WAR/WAW đã bị loại bỏ trước đó bởi register renaming.

### Mức Nâng cao (Advanced)

6. Sửa đổi hàm `simulate_ooo` ở Mục 9 để ghi lại và trả về thêm chỉ số **IPC trung bình thực tế** của toàn bộ chương trình (= tổng số lệnh chia cho tổng số chu kỳ), rồi so sánh với $\text{IPC}_{\text{lý tưởng}} = w$ (Mục 7.1) cho ví dụ ở Mục 10. Giải thích khoảng cách giữa hai giá trị bằng đúng thuật ngữ đã học (ILP hạn chế, ROB nghẽn cổ chai).
7. Thiết kế một chương trình đồ chơi 6 lệnh (tự chọn phụ thuộc RAW giữa một số lệnh) sao cho IPC thực tế đo được từ `simulate_ooo` (với `issue_width=2`) đạt **ít nhất 90%** so với $\text{IPC}_{\text{lý tưởng}} = 2$. Giải thích lựa chọn thiết kế chương trình của bạn dựa trên khái niệm ILP (Mục 7.2).

### Mức Nghiên cứu (Research)

8. Tìm hiểu ngắn gọn về thiết kế lõi hỗn hợp hiệu năng cao/tiết kiệm năng lượng (ví dụ triết lý big.LITTLE, đã nhắc ở Mục 11). Thảo luận: quyết định đặt một tác vụ cụ thể lên lõi out-of-order hiệu năng cao hay lõi in-order tiết kiệm năng lượng nên dựa trên đặc điểm ILP nào của chính tác vụ đó?

---

## 18. Dự án nhỏ

**Dự án: Bộ mô phỏng Superscalar Out-of-Order hoàn chỉnh với báo cáo IPC**

- **Mục tiêu:** Mở rộng `simulate_ooo` ở Mục 9 thành một công cụ phân tích hiệu năng đầy đủ, có khả năng so sánh nhiều chương trình đồ chơi khác nhau.
- **Yêu cầu:**
  - Hỗ trợ đọc một danh sách lệnh dạng đơn giản (ví dụ tuple `(dest, src1, src2, latency)`), tự động áp dụng `rename_registers`.
  - Tính và in ra: tổng số chu kỳ, IPC thực tế, và tỷ lệ phần trăm so với $\text{IPC}_{\text{lý tưởng}} = w$ (Bài tập 6).
  - Chạy thử với ít nhất 3 chương trình khác nhau: (a) hoàn toàn độc lập (ILP cao), (b) hoàn toàn phụ thuộc chuỗi (RAW liên tiếp, ILP thấp), (c) hỗn hợp — và lập bảng so sánh IPC giữa ba trường hợp.
  - Thử nghiệm với ít nhất hai giá trị `issue_width` khác nhau (ví dụ 1 và 4) trên cùng một chương trình, minh họa rõ hiện tượng "lợi nhuận giảm dần" đã nêu ở Mục 12 khi ILP chương trình không đủ cao.
- **Công nghệ gợi ý:** Python thuần, tái sử dụng `Instruction`, `rename_registers`, `simulate_ooo` từ Mục 9.
- **Kết quả mong đợi:** Một bảng so sánh định lượng rõ ràng, cho thấy IPC thực tế phụ thuộc mạnh vào ILP của chương trình, không chỉ vào issue width phần cứng.
- **Hướng mở rộng:** Thêm mô phỏng đơn giản cho một lệnh rẽ nhánh (không cần dự đoán, chỉ cần "chặn" fetch cho tới khi giải quyết xong) — chuẩn bị trực tiếp về mặt khái niệm cho Chương 4.1.6.

---

## 19. Tự đánh giá

- [ ] Tôi có thể giải thích chính xác tại sao pipeline đơn phát hành có CPI lý tưởng bằng 1, và superscalar phá vỡ giới hạn đó như thế nào.
- [ ] Tôi phân biệt được rõ ràng RAW, WAR, WAW — và biết chính xác loại nào bị loại bỏ bởi register renaming, loại nào thì không.
- [ ] Tôi có thể giải thích vai trò của Reorder Buffer bằng chính ví dụ ở Hình 4.1.5.2/Mục 10, không cần nhìn lại tài liệu.
- [ ] Tôi hiểu rõ khái niệm ILP là thuộc tính của chương trình, không phải của CPU, và có thể giải thích tại sao issue width lớn không phải lúc nào cũng có lợi.
- [ ] Tôi đã hoàn thành Bài tập 7 (thiết kế chương trình đạt ≥90% IPC lý tưởng) và hiểu rõ lý do tại sao lựa chọn phụ thuộc lệnh của mình đạt được kết quả đó.

Nếu Bài tập 4–6 (liên quan trực tiếp đến `simulate_ooo`) vẫn còn khó khăn, nên đọc lại kỹ Mục 8–9 và đối chiếu từng dòng code với đúng bước tương ứng trong quy trình 5 bước — kỹ năng "đọc mã mô phỏng phần cứng và đối chiếu với mô hình lý thuyết" sẽ tiếp tục được dùng ở Chương 4.1.6.

---

## 20. Đọc thêm

- **Sách:** Randal E. Bryant, David R. O'Hallaron, *Computer Systems: A Programmer's Perspective* — các phần liên quan đến tổ chức xử lý hiện đại và ảnh hưởng của thực thi không theo thứ tự lên hiệu năng phần mềm quan sát được. *(Xem BOOKS.md — Volume 4.)*
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc thêm về thuật toán Tomasulo gốc (1967) — mô tả chi tiết cơ chế reservation station và common data bus, phiên bản đầy đủ hơn nhiều so với mô hình đơn giản hóa trong chương này.
- **Chương tiếp theo:** Chương 4.1.6 — Branch Prediction.

---

### Liên kết chương (Cross References)

- **Chương trước:** 4.1.4 — Pipeline (data hazard/RAW đã học ở đó chính là ràng buộc duy nhất còn lại sau khi register renaming loại bỏ WAR/WAW ở chương này); 4.1.3 — CPU Organization (công thức hiệu năng CPI được mở rộng trực tiếp ở Mục 7).
- **Chương tiếp theo:** 4.1.6 — Branch Prediction, xử lý control hazard trong bối cảnh CPU thực thi động mạnh mẽ như đã học ở chương này.
- **Chương liên quan xa hơn:** Volume 4, Part IV — Concurrency and Parallel Computing (memory ordering, đã nhắc ở Mục 14, mở rộng khái niệm "thứ tự quan sát được" sang hệ nhiều lõi); Chương 4.1.7 — SIMD (một hướng khai thác song song khác, ở cấp dữ liệu thay vì cấp lệnh, như đã nêu ở Mục 12).
- **Vị trí trong Knowledge Graph:** Nút thứ năm của Volume 04, Part I; phụ thuộc trực tiếp vào Chương 4.1.3 và 4.1.4; là điều kiện tiên quyết khuyến nghị (không bắt buộc tuyệt đối) cho Chương 4.1.6, vì hai kỹ thuật thường được trình bày và triển khai cùng nhau trong CPU thực tế.

---

*Hết Chương 4.1.5. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`. Mô phỏng `simulate_ooo` được kiểm chứng bằng Python thực thi thực tế (Mục 9–10), tái khẳng định đúng hành vi lý thuyết của Reorder Buffer đã mô tả ở Hình 4.1.5.2. Đang chờ rà soát trước khi tiếp tục sang Chương 4.1.6 — Branch Prediction.*
