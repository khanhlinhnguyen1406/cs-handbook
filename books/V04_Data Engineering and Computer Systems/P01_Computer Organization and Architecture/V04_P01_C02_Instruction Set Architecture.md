# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 04 — Computer Systems
### Part I — Computer Organization and Architecture
## Chương 4.1.2 — Instruction Set Architecture (ISA)
### (Kiến trúc Tập lệnh)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 4.1.2 |
| Thuộc Part | I — Computer Organization and Architecture |
| Thuộc Volume | 04 — Computer Systems |
| Thời gian đọc ước tính | 45–60 phút |
| Độ khó | ★★★☆☆ |
| Kiến thức tiên quyết | Chương 4.1.1 — Digital Logic Overview; Volume 1, Chương 1.5 — Set Theory (đếm tổ hợp, $2^n$); Volume 2, Part III — Programming Paradigms (khái niệm compiler ở mức tổng quan) |
| Chương liên quan | 4.1.3 — CPU Organization: chu trình fetch–decode–execute hiện thực hóa chính xác đặc tả của ISA học ở chương này |
| Từ khóa | Instruction Set Architecture, RISC, CISC, opcode, operand, addressing mode, instruction encoding, assembly language |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Định nghĩa ISA như một "hợp đồng" (contract) giữa phần mềm và phần cứng, phân biệt rõ với microarchitecture (cách hiện thực bên trong).
- Giải thích cấu trúc tổng quát của một lệnh máy: opcode và operand.
- Phân biệt RISC và CISC dựa trên triết lý thiết kế, không chỉ dựa trên danh sách ví dụ.
- Tính được dung lượng mã hóa (encoding capacity) của một trường bit cho trước, áp dụng lại kỹ thuật đếm tổ hợp từ Volume 1.
- Mô tả các chế độ địa chỉ (addressing mode) phổ biến và lý do chúng tồn tại.
- Mã hóa (encode) và giải mã (decode) thủ công một lệnh máy đơn giản từ đặc tả ISA cho trước.
- Kết nối ISA với vai trò của compiler trong việc dịch mã nguồn cấp cao sang mã máy cụ thể của một kiến trúc.

---

### Câu hỏi khơi gợi

> *Vì sao một ứng dụng biên dịch sẵn (compiled) cho MacBook dùng chip Apple Silicon (ARM) không thể chạy trực tiếp trên một máy tính dùng chip Intel (x86), dù cả hai đều "chỉ là" CPU hiểu 0 và 1? Và vì sao khi Apple chuyển từ Intel sang Apple Silicon năm 2020, toàn bộ hệ sinh thái phần mềm không sụp đổ ngay lập tức, mà vẫn chạy được nhờ một lớp "phiên dịch" gọi là Rosetta 2?*

---

## 1. Tổng quan chương

Chương 4.1.1 đã trả lời câu hỏi "mạch điện có thể tính toán như thế nào" ở mức thấp nhất — cổng logic, mạch tổ hợp, mạch tuần tự. Nhưng một câu hỏi khác vẫn còn bỏ ngỏ: **phần mềm giao tiếp với hàng tỷ cổng logic đó bằng ngôn ngữ gì?**

Câu trả lời là **Instruction Set Architecture (ISA)** — tập hợp hữu hạn các lệnh máy (machine instruction) mà một họ CPU cam kết hiểu và thực thi đúng. ISA là ranh giới trừu tượng quan trọng bậc nhất trong toàn bộ ngành Computer Systems: nó tách biệt hoàn toàn "CPU làm gì" (behavior, được ISA đặc tả) khỏi "CPU làm điều đó bằng cách nào" (implementation, thuộc về microarchitecture — nội dung của Chương 4.1.3 trở đi).

Chương này giới thiệu cấu trúc của một lệnh máy, hai triết lý thiết kế lớn nhất trong lịch sử — RISC và CISC — và cách một chuỗi bit vô nghĩa trở thành một hành động cụ thể mà CPU thực hiện.

> **💡 Insight**
> ISA đóng đúng vai trò của một **interface** trong lập trình hướng đối tượng (Volume 2, Part III): nó định nghĩa "hợp đồng" — CPU cam kết thực hiện đúng những gì ISA đặc tả — nhưng hoàn toàn không ràng buộc *cách* CPU hiện thực hợp đồng đó bên trong. Hai con chip tuân theo cùng một ISA (ví dụ hai chip x86 khác nhau của Intel và AMD) có thể có microarchitecture bên trong hoàn toàn khác nhau, miễn là chạy đúng cùng một chương trình và cho ra cùng kết quả.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Sự kiện | Đóng góp |
|---|---|---|
| 1945 | Kiến trúc stored-program của John von Neumann (đã giới thiệu ở Chương 4.1.1, Mục 2) | Đặt nền tảng khái niệm: chương trình được biểu diễn dưới dạng lệnh nhị phân lưu chung bộ nhớ với dữ liệu — tiền đề để "lệnh máy" trở thành một khái niệm có thể mã hóa và chuẩn hóa |
| Thập niên 1970 | Intel phát triển dòng vi xử lý x86 (khởi đầu 8086, 1978) | Một trong những ISA thương mại có vòng đời dài nhất lịch sử, đại diện tiêu biểu cho triết lý CISC |
| Thập niên 1980 | Các dự án nghiên cứu RISC tại Berkeley và Stanford | Đề xuất triết lý thiết kế đối lập: tập lệnh nhỏ, đơn giản, thực thi nhanh — đặt nền tảng cho các kiến trúc ARM và MIPS sau này |
| 2010–nay | ARM trở thành ISA thống trị trong thiết bị di động; RISC-V nổi lên như một ISA mở (open ISA) | Minh chứng cho việc lựa chọn ISA vẫn là quyết định chiến lược sống còn của ngành công nghiệp bán dẫn, không chỉ là vấn đề lịch sử |

Điều đáng chú ý: cuộc tranh luận RISC và CISC không phải là một cuộc thi đã ngã ngũ dứt khoát. CPU x86 hiện đại (về danh nghĩa là CISC) thực chất giải mã lệnh CISC phức tạp thành các vi lệnh (micro-operations) đơn giản kiểu RISC ở bên trong trước khi thực thi — một chi tiết microarchitecture sẽ được làm rõ hơn ở Chương 4.1.3.

---

## 3. Động lực

Quay lại câu hỏi khơi gợi: một chương trình được biên dịch (compiled) không sinh ra mã máy "chung chung" — nó sinh ra một chuỗi byte cụ thể, được thiết kế để khớp chính xác với ISA của một họ CPU nhất định. Chuỗi byte đó cho ARM và chuỗi byte đó cho x86 là hai thứ hoàn toàn khác nhau về mặt mã hóa nhị phân, dù cùng biểu diễn "cộng hai số rồi lưu kết quả".

Đây chính là lý do:

- Một file thực thi `.exe` biên dịch cho x86 không chạy được trên chip ARM nếu không có lớp dịch/giả lập (như Rosetta 2 của Apple, hoặc QEMU).
- Compiler (Volume 2, Part III) cần một "backend" riêng cho từng ISA — đây là lý do GCC/LLVM hỗ trợ hàng chục kiến trúc đích (target architecture) khác nhau.
- Khi bạn viết `a + b` trong bất kỳ ngôn ngữ lập trình nào, dòng code cấp cao đó cuối cùng phải được dịch thành **đúng một lệnh cộng cụ thể**, được mã hóa theo đúng định dạng nhị phân mà ISA đích quy định.

ISA, vì vậy, là lớp trừu tượng quyết định toàn bộ "khả năng tương thích nhị phân" (binary compatibility) của phần mềm — một khái niệm kỹ sư phần mềm nào cũng từng chạm tới, dù có thể chưa từng gọi đúng tên.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> ISA giống như một **thực đơn cố định (fixed menu)** tại một nhà hàng: nó liệt kê chính xác những "món" (lệnh) mà đầu bếp (CPU) cam kết có thể nấu, cùng với "cách gọi món" (cú pháp mã hóa) chuẩn. Bạn — với vai trò khách hàng (phần mềm) — chỉ có thể gọi đúng những món trong thực đơn, theo đúng định dạng gọi món đã quy định. Nhà hàng có thể thay đầu bếp, thay bếp trưởng, thay quy trình nấu nướng bên trong (đó là microarchitecture) — miễn là món ăn cuối cùng ra đúng như thực đơn cam kết, khách hàng không cần biết gì về những gì diễn ra trong bếp.

| Trực giác kỹ thuật bạn đã có | Khái niệm ISA tương ứng |
|---|---|
| Interface trong OOP — định nghĩa method signature nhưng không ràng buộc implementation | ISA — đặc tả hành vi lệnh máy, không ràng buộc microarchitecture |
| REST API endpoint (`POST /orders`) — hợp đồng cố định về định dạng request | Định dạng mã hóa lệnh (instruction encoding) — hợp đồng cố định về vị trí opcode/operand |
| Cross-compilation trong CI/CD (build riêng cho Linux/macOS/Windows) | Compiler backend riêng cho từng ISA đích (x86, ARM, RISC-V) |

---

## 5. Trực quan hóa khái niệm

**Hình 4.1.2.1 — Cấu trúc tổng quát của một lệnh máy**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
┌─────────────┬─────────────┬─────────────┬─────────────┐
│   OPCODE    │  OPERAND 1  │  OPERAND 2  │  OPERAND 3  │
│  (mã lệnh)  │ (toán hạng) │ (toán hạng) │ (toán hạng) │
└─────────────┴─────────────┴─────────────┴─────────────┘
      │               │             │             │
      ▼               ▼             ▼             ▼
  "Làm gì?"      "Với cái gì?"  "Với cái gì?"  "Lưu vào đâu?"
  (ADD, SUB,      (thanh ghi,    (thanh ghi,    (thanh ghi
   LOAD, JMP...)   hằng số,       hằng số,       đích)
                   địa chỉ bộ     địa chỉ bộ
                   nhớ...)        nhớ...)
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Cho thấy mọi lệnh máy — bất kể ISA cụ thể nào — đều tách thành hai phần khái niệm: **opcode** (làm gì) và **operand** (làm với cái gì) |
| Điểm mấu chốt | Số lượng operand khác nhau giữa các ISA (0, 1, 2, hoặc 3-operand instruction) chính là một trong những khác biệt cốt lõi giữa các triết lý thiết kế — xem Mục 6–7 |

---

**Hình 4.1.2.2 — Không gian mã hóa: từ chuỗi bit đến hành động cụ thể**

```text
Chuỗi bit thô:        1 0 1 1 0 0 0 1 0 0 1 0 0 0 1 1
                       └──┬───┘ └──┬───┘ └──┬───┘
                        opcode   operand   operand
                        (4 bit)   1 (4b)    2 (4b)

Giải mã (decode):     opcode = 1011 → tra bảng ISA → "ADD"
                       operand1 = 0001 → thanh ghi R1
                       operand2 = 0010 → thanh ghi R2

Ý nghĩa cuối cùng:     "Cộng giá trị thanh ghi R1 và R2"
```

*Mục đích:* Minh họa cụ thể quá trình một chuỗi bit — vốn không mang ý nghĩa nội tại — trở thành một hành động có ngữ nghĩa rõ ràng, hoàn toàn phụ thuộc vào **quy ước mã hóa** mà ISA quy định trước. *Điểm mấu chốt:* đây chính là lý do cùng một chuỗi 16-bit có thể có ý nghĩa hoàn toàn khác nhau trên hai ISA khác nhau — không có "ý nghĩa tuyệt đối" nào gắn liền với chuỗi bit, chỉ có ý nghĩa theo quy ước.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Instruction Set Architecture (ISA)**
>
> **Instruction Set Architecture (ISA)** là đặc tả trừu tượng (abstract specification) định nghĩa toàn bộ tập hợp lệnh máy mà một họ CPU cam kết hỗ trợ, bao gồm: cú pháp mã hóa nhị phân của từng lệnh, tập thanh ghi khả kiến với phần mềm (architectural registers), mô hình bộ nhớ, và ngữ nghĩa thực thi của mỗi lệnh. ISA là **hợp đồng** giữa phần mềm và phần cứng — hoàn toàn tách biệt khỏi **microarchitecture**, tức cách một con chip cụ thể hiện thực hợp đồng đó bên trong.
>
> Mỗi lệnh máy gồm hai thành phần:
> - **Opcode (Operation Code):** trường bit xác định *loại hành động* cần thực hiện (ví dụ: cộng, trừ, nạp dữ liệu, nhảy lệnh).
> - **Operand:** trường bit xác định *đối tượng* mà hành động đó tác động lên — có thể là thanh ghi, hằng số (immediate value), hoặc địa chỉ bộ nhớ.

> **📌 Remember — Addressing Mode**
>
> **Addressing Mode (chế độ địa chỉ)** là quy tắc xác định cách một operand được diễn giải để tìm ra giá trị thực sự cần dùng. Các chế độ phổ biến:
>
> | Addressing Mode | Ý nghĩa | Ví dụ |
> |---|---|---|
> | Immediate | Giá trị nằm ngay trong lệnh | `ADD R1, 5` — cộng trực tiếp số 5 |
> | Register | Giá trị nằm trong một thanh ghi | `ADD R1, R2` — cộng giá trị thanh ghi R2 |
> | Direct | Operand là địa chỉ bộ nhớ trực tiếp | `LOAD R1, [1000]` — nạp giá trị tại địa chỉ 1000 |
> | Register Indirect | Operand là thanh ghi chứa địa chỉ bộ nhớ | `LOAD R1, [R2]` — nạp giá trị tại địa chỉ được lưu trong R2 |

---

## 7. Nền tảng toán học

### 7.1 Dung lượng mã hóa (Encoding Capacity)

- **Ý nghĩa:** Số lượng lệnh khác nhau mà một ISA có thể mã hóa bị giới hạn nghiêm ngặt bởi số bit dành cho trường opcode.
- **Kết nối trực tiếp:** đây chính là bài toán đếm tổ hợp đã gặp ở Volume 1, Chương 1.5, Mục 7.1 (kích thước tập lũy thừa $|\mathcal{P}(A)| = 2^{|A|}$) — cùng một cấu trúc toán học, chỉ khác ngữ cảnh: thay vì "mỗi phần tử có mặt hay không", ở đây là "mỗi bit là 0 hay 1".

> **📦 Formula Box — Dung lượng mã hóa của một trường bit**
>
> $$N_{\text{lệnh khả dụng}} = 2^{b}$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $b$ | Số bit dành cho trường opcode |
> | $N_{\text{lệnh khả dụng}}$ | Số lượng mã lệnh (mnemonic) khác nhau có thể phân biệt được |
> | **Diễn giải kỹ thuật** | Mỗi bit bổ sung nhân đôi số lệnh khả dụng — hoàn toàn tương tự cách mỗi phần tử bổ sung nhân đôi số tập con trong Mục 7.1, Chương 1.5, Volume 1 |
> | **Ứng dụng thường gặp** | Giải thích vì sao ISA RISC cổ điển (opcode ngắn, ví dụ 6–7 bit) chỉ hỗ trợ vài chục đến trăm lệnh, trong khi ISA CISC (x86) với opcode biến đổi độ dài có thể mã hóa hàng nghìn biến thể lệnh |

**Ví dụ áp dụng:** với opcode 4-bit (như Hình 4.1.2.2), $N = 2^4 = 16$ lệnh khả dụng tối đa. Với opcode 8-bit, $N = 2^8 = 256$ lệnh. Đây là lý do các ISA CISC hiện đại thường dùng **độ dài lệnh biến đổi (variable-length encoding)** — cho phép những lệnh phổ biến dùng mã ngắn (tiết kiệm bộ nhớ chương trình), còn lệnh hiếm gặp/phức tạp dùng mã dài hơn để không giới hạn tổng số lệnh khả dụng.

### 7.2 Đánh đổi giữa Opcode và Operand trong một từ lệnh có độ dài cố định

- **Ý nghĩa:** Với một ISA có độ dài lệnh cố định (fixed-length instruction, đặc trưng của RISC), tổng số bit là hằng số — mở rộng trường opcode đồng nghĩa thu hẹp trường operand, và ngược lại.

$$b_{\text{opcode}} + b_{\text{operand}} = L \quad (\text{hằng số, độ dài lệnh cố định})$$

Với $L$ cố định (ví dụ 32-bit trong nhiều ISA RISC), tăng $b_{\text{opcode}}$ để hỗ trợ nhiều loại lệnh hơn sẽ **giảm** số bit dành cho operand — nghĩa là giảm phạm vi giá trị hằng số (immediate value) có thể mã hóa trực tiếp, hoặc giảm số thanh ghi có thể địa chỉ hóa trực tiếp. Đây là một đánh đổi thiết kế thực sự, không có lựa chọn "miễn phí".

---

## 8. Thuật toán / Cơ chế

**Quy trình giải mã (decode) một lệnh có độ dài cố định** — tổng quát hóa Hình 4.1.2.2:

```text
Bước 1 — Đọc trọn vẹn L bit của lệnh từ bộ nhớ chương trình
        │
        ▼
Bước 2 — Tách b_opcode bit đầu tiên (theo quy ước vị trí cố định
         của ISA) thành trường opcode
        │
        ▼
Bước 3 — Tra bảng ánh xạ opcode → hành động (ví dụ: 1011 → "ADD")
        │
        ▼
Bước 4 — Tách các bit còn lại thành operand, theo đúng số lượng
         và độ dài mà ISA quy định RIÊNG cho loại lệnh vừa tra
         được ở Bước 3 (một số ISA có định dạng operand khác
         nhau tùy loại lệnh — gọi là "instruction format")
        │
        ▼
Bước 5 — Diễn giải từng operand theo đúng Addressing Mode
         tương ứng (Mục 6) — ví dụ: operand là số thanh ghi,
         hay là hằng số trực tiếp, hay là địa chỉ bộ nhớ
        │
        ▼
Bước 6 — Chuyển hành động + operand đã diễn giải cho các mạch
         thực thi tương ứng (ALU, bộ nhớ...) — đây chính là
         điểm nối sang Chương 4.1.3 (CPU Organization)
```

> **⚠️ Common Mistake**
> Một nhầm lẫn phổ biến là nghĩ rằng "biết ISA là biết cách CPU hoạt động bên trong". Trên thực tế, ISA **chỉ** đặc tả hành vi quan sát được từ bên ngoài (kết quả cuối cùng của mỗi lệnh), hoàn toàn không nói gì về việc CPU dùng bao nhiêu chu kỳ, có pipeline hay không, có thực thi song song hay không. Hai chip tuân thủ **cùng một ISA** — ví dụ Intel Core và AMD Ryzen, cùng là x86 — có microarchitecture bên trong khác biệt hoàn toàn.

---

## 9. Triển khai

```python
# Mô phỏng một ISA đồ chơi (toy ISA) 8-bit: 4-bit opcode, 4-bit operand
# Định dạng lệnh: [ opcode (4 bit) | operand (4 bit) ]
# Áp dụng trực tiếp Hình 4.1.2.2 và quy trình giải mã ở Mục 8

OPCODE_TABLE = {
    0b0001: "LOAD",   # nạp giá trị hằng số vào thanh ghi tích lũy (ACC)
    0b0010: "ADD",    # cộng giá trị thanh ghi vào ACC
    0b0011: "SUB",    # trừ giá trị thanh ghi khỏi ACC
    0b0100: "STORE",  # lưu ACC vào một "thanh ghi" đích
    0b1111: "HALT",   # dừng chương trình
}


def encode(mnemonic, operand):
    """Mã hóa một lệnh assembly đồ chơi thành 1 byte,
    theo đúng định dạng [opcode(4)|operand(4)] ở Hình 4.1.2.2."""
    opcode_lookup = {v: k for k, v in OPCODE_TABLE.items()}
    opcode = opcode_lookup[mnemonic]
    return (opcode << 4) | (operand & 0xF)


def decode(byte):
    """Giải mã 1 byte thành (mnemonic, operand) — hiện thực
    Bước 2-5 của quy trình giải mã ở Mục 8."""
    opcode = (byte >> 4) & 0xF
    operand = byte & 0xF
    mnemonic = OPCODE_TABLE.get(opcode, "UNKNOWN")
    return mnemonic, operand


def run_program(program_bytes):
    """Mô phỏng thực thi một chương trình gồm nhiều lệnh đã mã hóa.
    Chỉ có 1 thanh ghi tích lũy (ACC) và tối đa 16 'thanh ghi lưu'
    để giữ cho ISA đồ chơi này đơn giản."""
    acc = 0
    registers = [0] * 16
    trace = []

    for byte in program_bytes:
        mnemonic, operand = decode(byte)
        if mnemonic == "LOAD":
            acc = operand
        elif mnemonic == "ADD":
            acc += registers[operand]
        elif mnemonic == "SUB":
            acc -= registers[operand]
        elif mnemonic == "STORE":
            registers[operand] = acc
        elif mnemonic == "HALT":
            trace.append((byte, mnemonic, operand, acc))
            break
        trace.append((byte, mnemonic, operand, acc))

    return acc, trace
```

Hai hàm `encode` và `decode` hiện thực chính xác hai chiều của Hình 4.1.2.2: từ mnemonic assembly sang chuỗi bit, và ngược lại. Hàm `run_program` mô phỏng vòng lặp "giải mã rồi thực thi" ở mức khái niệm — chính là tiền thân đơn giản hóa của chu trình fetch–decode–execute sẽ trình bày đầy đủ ở Chương 4.1.3.

---

## 10. Trực quan hóa quá trình thực thi

**Mã hóa và thực thi chương trình đồ chơi:** `LOAD 5` → `STORE R0` → `LOAD 3` → `ADD R0` → `HALT`, tính $5 + 3$:

```python
program = [
    encode("LOAD", 5),    # ACC = 5
    encode("STORE", 0),   # R0 = ACC = 5
    encode("LOAD", 3),    # ACC = 3
    encode("ADD", 0),     # ACC = ACC + R0 = 3 + 5
    encode("HALT", 0),
]
result, trace = run_program(program)
```

| Byte (nhị phân) | Mnemonic (sau decode) | Operand | ACC sau khi thực thi |
|:---:|:---:|:---:|:---:|
| `00010101` | LOAD | 5 | 5 |
| `01000000` | STORE | 0 | 5 |
| `00010011` | LOAD | 3 | 3 |
| `00100000` | ADD | 0 | 8 |
| `11110000` | HALT | 0 | 8 |

Kết quả cuối cùng: `ACC = 8` — khớp với $5 + 3 = 8$, và chuỗi byte này (`0x15, 0x40, 0x13, 0x20, 0xF0`) chính là "mã máy" thực sự theo ISA đồ chơi vừa định nghĩa — **không có ý nghĩa gì** nếu tách rời khỏi `OPCODE_TABLE`, đúng như đã nhấn mạnh ở Hình 4.1.2.2.

**Kiểm chứng dung lượng mã hóa** (Mục 7.1) cho các độ dài opcode khác nhau:

```text
opcode 3-bit  → tối đa   8 lệnh khả dụng
opcode 4-bit  → tối đa  16 lệnh khả dụng
opcode 6-bit  → tối đa  64 lệnh khả dụng
opcode 8-bit  → tối đa 256 lệnh khả dụng
```

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Lựa chọn ISA là một trong những quyết định kỹ thuật có ảnh hưởng lâu dài nhất trong toàn bộ vòng đời một nền tảng phần cứng — thay đổi ISA đồng nghĩa phải biên dịch lại (hoặc giả lập) toàn bộ hệ sinh thái phần mềm.

| Bối cảnh công nghiệp | Vai trò của ISA |
|---|---|
| Apple chuyển từ Intel (x86) sang Apple Silicon (ARM), 2020 | Rosetta 2 dịch động (dynamic binary translation) mã x86 sang ARM tại thời điểm chạy — một minh chứng trực tiếp cho câu hỏi khơi gợi đầu chương |
| RISC-V — ISA mã nguồn mở | Cho phép bất kỳ tổ chức nào thiết kế chip tuân theo cùng một đặc tả công khai, miễn phí bản quyền — khác biệt với x86 (độc quyền Intel/AMD) và ARM (cấp phép bản quyền) |
| Compiler backend (GCC, LLVM/Clang) | Mỗi backend dịch cùng một mã trung gian (intermediate representation) sang mã máy cụ thể của một ISA đích — ví dụ LLVM hỗ trợ hàng chục target khác nhau từ cùng một frontend |
| WebAssembly (Wasm) | Một dạng "ISA ảo" (virtual ISA) độc lập phần cứng, được các trình duyệt biên dịch tiếp sang ISA thật của máy đang chạy — mở rộng khái niệm ISA sang tầng phần mềm |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Tranh luận RISC và CISC (Mục 2) đặt ra một câu hỏi thiết kế sâu sắc hơn nhiều so với "tập lệnh nào tốt hơn": **đâu là ranh giới tối ưu giữa độ phức tạp phần cứng và độ phức tạp compiler?** CISC đặt gánh nặng "làm nhiều việc trong một lệnh" lên phần cứng; RISC đặt gánh nặng đó lên compiler, vốn phải sinh ra nhiều lệnh đơn giản hơn để đạt cùng hiệu quả.

Một hướng nghiên cứu và phát triển công nghiệp đang rất sôi động hiện nay là **domain-specific ISA** — thiết kế tập lệnh chuyên biệt cho một loại khối lượng công việc cụ thể, thay vì một ISA đa dụng. Ví dụ tiêu biểu: các tập lệnh mở rộng SIMD (sẽ học ở Chương 4.1.7) và các đơn vị tính toán ma trận chuyên dụng cho Deep Learning (liên hệ trực tiếp đến Volume 05) — nơi ISA không còn chỉ tối ưu cho mã tổng quát, mà tối ưu cho một tập phép toán hẹp nhưng cực kỳ phổ biến trong khối lượng công việc AI hiện đại.

RISC-V, với tư cách một ISA mở có thể mở rộng module hóa (modular extension), là một nền tảng nghiên cứu tích cực cho chính câu hỏi này: cộng đồng học thuật và công nghiệp có thể thử nghiệm các phần mở rộng ISA mới mà không cần xin cấp phép từ một chủ sở hữu độc quyền — một sự khác biệt lớn so với thời kỳ x86/ARM thống trị hoàn toàn.

---

## 13. Ưu điểm

- **Tính di động ngữ nghĩa (semantic portability):** một khi phần mềm được biên dịch đúng cho một ISA, nó chạy đúng trên **mọi** chip tuân theo ISA đó, bất kể microarchitecture bên trong khác nhau thế nào.
- **Tách biệt rõ ràng các mối quan tâm (separation of concerns):** nhà thiết kế phần cứng có thể tự do đổi mới microarchitecture (pipeline, superscalar — các chương tiếp theo) mà không phá vỡ khả năng tương thích phần mềm hiện có.
- **Dung lượng mã hóa có thể tính toán được:** công thức $2^b$ (Mục 7.1) cho phép nhà thiết kế ISA lập kế hoạch chính xác số lượng lệnh tối đa cần hỗ trợ, ngay từ giai đoạn thiết kế.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Một hiểu lầm phổ biến khác: cho rằng "RISC luôn nhanh hơn CISC" hoặc ngược lại. Thực tế, CPU x86 hiện đại (CISC bề ngoài) đạt hiệu năng cạnh tranh trực tiếp với ARM (RISC bề ngoài) chính nhờ áp dụng các kỹ thuật microarchitecture chung — như giải mã lệnh CISC phức tạp thành vi lệnh RISC nội bộ (đã nêu ở Mục 2). So sánh RISC/CISC chỉ có ý nghĩa ở tầng ISA, không nên suy diễn trực tiếp thành so sánh hiệu năng.

- **Đánh đổi cố định giữa opcode và operand** (Mục 7.2): với độ dài lệnh cố định, ISA không thể vừa hỗ trợ rất nhiều loại lệnh, vừa hỗ trợ hằng số/địa chỉ có phạm vi rất lớn trong cùng một lệnh.
- **Chi phí chuyển đổi ISA cực lớn:** một khi hệ sinh thái phần mềm đã phát triển quanh một ISA (như x86 trong hơn 40 năm), việc chuyển đổi đòi hỏi biên dịch lại toàn bộ hoặc chấp nhận lớp giả lập tốn hiệu năng (như Rosetta 2, Mục 11).
- **ISA không đặc tả hiệu năng:** như đã nêu ở Mục 8, ISA chỉ cam kết *kết quả đúng*, không cam kết *tốc độ* — hai chip cùng ISA có thể chênh lệch hiệu năng rất lớn tùy microarchitecture.

---

## 15. So sánh

**Bảng 4.1.2.1 — RISC và CISC: hai triết lý thiết kế ISA**

| Tiêu chí | RISC (Reduced Instruction Set Computer) | CISC (Complex Instruction Set Computer) |
|---|---|---|
| Số lượng lệnh | Ít, mỗi lệnh làm một việc đơn giản | Nhiều, một số lệnh thực hiện nhiều bước phức tạp trong một lệnh |
| Độ dài lệnh | Thường cố định (fixed-length) | Thường biến đổi (variable-length) |
| Gánh nặng thiết kế | Đặt nhiều gánh nặng lên compiler (sinh nhiều lệnh đơn giản) | Đặt nhiều gánh nặng lên phần cứng (mạch giải mã phức tạp) |
| Ví dụ tiêu biểu | ARM, RISC-V, MIPS | x86 (Intel, AMD) |
| Xu hướng hiện đại | CPU x86 giải mã lệnh CISC thành vi lệnh kiểu RISC nội bộ — ranh giới RISC/CISC ở tầng microarchitecture ngày càng mờ | |

**Bảng 4.1.2.2 — Các Addressing Mode và trường hợp sử dụng điển hình**

| Addressing Mode | Ưu điểm | Trường hợp dùng điển hình |
|---|---|---|
| Immediate | Nhanh nhất — không cần truy cập thêm | Gán hằng số, ví dụ `i = 0` |
| Register | Nhanh — chỉ truy cập thanh ghi | Phép toán số học giữa các biến cục bộ |
| Direct | Truy cập trực tiếp bộ nhớ tại địa chỉ cố định | Biến toàn cục (global variable) |
| Register Indirect | Cho phép địa chỉ động, tính toán được tại runtime | Truy cập phần tử mảng, con trỏ (pointer) |

**Phân tích:** Bảng 4.1.2.1 cho thấy RISC/CISC không phải là "đúng/sai" mà là hai điểm đánh đổi khác nhau trên cùng một trục thiết kế — echo đúng nguyên tắc Mục 7.2 (đánh đổi opcode/operand) ở quy mô triết lý toàn ISA. Bảng 4.1.2.2 cho thấy các Addressing Mode khác nhau tồn tại không phải vì dư thừa, mà vì mỗi mode tối ưu cho một mẫu truy cập dữ liệu khác nhau trong chương trình thực tế — biến cục bộ, biến toàn cục, và cấu trúc dữ liệu động đều cần chế độ địa chỉ phù hợp riêng.

---

## 16. Tóm tắt

- **ISA** là hợp đồng trừu tượng giữa phần mềm và phần cứng, tách biệt hoàn toàn khỏi microarchitecture (cách hiện thực bên trong).
- Mỗi lệnh máy gồm **opcode** (làm gì) và **operand** (làm với cái gì); **Addressing Mode** quy định cách diễn giải operand.
- Dung lượng mã hóa của một trường bit tuân theo công thức $2^b$ — cùng cấu trúc toán học với bài toán đếm tập lũy thừa ở Volume 1, Chương 1.5.
- **RISC** và **CISC** là hai triết lý thiết kế đối lập, đánh đổi giữa độ phức tạp phần cứng và độ phức tạp compiler — nhưng ranh giới giữa chúng ở CPU hiện đại ngày càng mờ nhạt ở tầng microarchitecture.
- ISA giải thích trực tiếp vì sao mã máy biên dịch cho một kiến trúc không chạy được trên kiến trúc khác — câu trả lời cho câu hỏi khơi gợi đầu chương.

Chương 4.1.3 (CPU Organization) sẽ trả lời câu hỏi tiếp theo một cách tự nhiên: một khi ISA đã đặc tả *lệnh nào tồn tại và có ý nghĩa gì*, làm thế nào CPU tổ chức phần cứng — dùng chính các mạch đã học ở Chương 4.1.1 — để thực thi đúng chu trình fetch–decode–execute cho từng lệnh đó?

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Với opcode 5-bit, tính số lượng lệnh tối đa có thể mã hóa, áp dụng công thức ở Mục 7.1.
2. Cho lệnh 8-bit `10100011`, dùng bảng `OPCODE_TABLE` ở Mục 9 để giải mã opcode và operand (giả sử vẫn dùng định dạng `[opcode(4)|operand(4)]`).
3. Liệt kê 3 điểm khác biệt giữa ISA và microarchitecture, dựa trên định nghĩa ở Mục 6 và ví dụ ở Mục 8, 14.

### Mức Trung bình (Intermediate)

4. Cho một ISA với độ dài lệnh cố định 16-bit. Nếu cần hỗ trợ tối đa 100 loại lệnh khác nhau, tính số bit tối thiểu cần dành cho opcode (áp dụng Mục 7.1), và số bit còn lại tối đa dành cho operand (áp dụng Mục 7.2).
5. Viết thêm một mnemonic mới `MUL` (nhân hai thanh ghi) vào `OPCODE_TABLE` ở Mục 9, cập nhật `run_program` để hỗ trợ lệnh này, và viết một chương trình đồ chơi tính $4 \times 3$ bằng cách `LOAD`, `STORE`, rồi `MUL`.

### Mức Nâng cao (Advanced)

6. Thiết kế một **instruction format thứ hai** cho ISA đồ chơi ở Mục 9, dành riêng cho lệnh cần operand lớn hơn 4-bit (ví dụ: lệnh `JUMP` cần địa chỉ nhảy 12-bit). Giải thích cách CPU phân biệt được lệnh nào dùng format nào chỉ dựa vào opcode — đây chính là cơ chế "instruction format" nêu ở Bước 4, Mục 8.
7. So sánh định lượng: giả sử một chương trình cần thực hiện phép toán "cộng một hằng số lớn vào một ô nhớ tại địa chỉ tính toán được". Ước lượng (không cần chính xác tuyệt đối) số lệnh RISC đơn giản cần thiết để thực hiện việc này, so với một lệnh CISC "phức hợp" giả định có thể làm toàn bộ trong một lệnh. Thảo luận đánh đổi về kích thước mã chương trình (code size) so với độ phức tạp mạch giải mã.

### Mức Nghiên cứu (Research)

8. Tìm hiểu và trình bày ngắn gọn về RISC-V như một ISA "mở rộng theo module" (modular extension) — ví dụ phần mở rộng "M" (nhân/chia số nguyên) hay "V" (vector). Thảo luận: mô hình ISA mở rộng theo module này giải quyết đánh đổi nào đã nêu ở Mục 7.2 và Mục 12 tốt hơn so với một ISA nguyên khối cố định?

---

## 18. Dự án nhỏ

**Dự án: Assembler và Simulator hoàn chỉnh cho ISA đồ chơi**

- **Mục tiêu:** Mở rộng ISA đồ chơi ở Mục 9 thành một hệ thống hoàn chỉnh: một "assembler" đọc mã nguồn dạng text (ví dụ `LOAD 5`, `ADD 0`) và sinh ra chuỗi byte mã máy; một "simulator" nạp chuỗi byte đó và thực thi, in ra trạng thái từng bước.
- **Yêu cầu:**
  - Assembler phải hỗ trợ ít nhất 6 mnemonic (bao gồm cả `MUL` từ Bài tập 5 và một lệnh rẽ nhánh đơn giản như `JMPZ` — nhảy nếu ACC bằng 0).
  - Simulator phải in ra bảng trace tương tự Mục 10, gồm: byte gốc, mnemonic, operand, và trạng thái ACC/registers sau mỗi bước.
  - Viết ít nhất 3 chương trình đồ chơi mẫu (ví dụ: tính tổng dãy số từ 1 đến n bằng vòng lặp dùng `JMPZ`).
- **Công nghệ gợi ý:** Python thuần.
- **Kết quả mong đợi:** Một file assembly mẫu, một assembler chuyển nó thành mã máy, và một simulator chạy đúng chương trình đó, in ra kết quả khớp với tính toán thủ công.
- **Hướng mở rộng:** Thêm addressing mode Register Indirect (Mục 6) để mô phỏng truy cập mảng; đây là bước đệm khái niệm tốt trước khi học về bộ nhớ và con trỏ ở Part II (Memory Systems).

---

## 19. Tự đánh giá

- [ ] Tôi có thể giải thích rõ ràng, không mơ hồ, sự khác biệt giữa ISA và microarchitecture, kèm theo ví dụ cụ thể.
- [ ] Tôi có thể tính dung lượng mã hóa của một trường opcode cho trước, và giải thích đúng vì sao công thức đó giống hệt bài toán tập lũy thừa ở Volume 1.
- [ ] Tôi có thể tự mã hóa và giải mã một lệnh máy đơn giản bằng tay, không cần chạy code.
- [ ] Tôi hiểu vì sao RISC và CISC là một đánh đổi thiết kế, không phải một cuộc thi "đúng/sai" tuyệt đối.
- [ ] Tôi đã hoàn thành được Bài tập 5 (thêm lệnh `MUL`) và chạy đúng chương trình đồ chơi tính phép nhân.

Nếu Bài tập 4 (tính số bit opcode tối thiểu cho 100 lệnh) vẫn còn khó khăn, nên quay lại ôn nhanh Mục 7.1, Chương 1.5 (Volume 1) — kỹ năng "đếm số cấu hình khả dụng từ số bit cho trước" sẽ tái sử dụng liên tục trong suốt Volume 04, đặc biệt khi học về địa chỉ bộ nhớ ở Part II.

---

## 20. Đọc thêm

- **Sách:** Randal E. Bryant, David R. O'Hallaron, *Computer Systems: A Programmer's Perspective* — chương về biểu diễn lệnh máy và mã máy x86 cụ thể. *(Xem BOOKS.md — Volume 4.)*
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc tài liệu đặc tả chính thức của RISC-V (RISC-V ISA Specification) — một ví dụ thực tế, được công khai đầy đủ, về cách một ISA hiện đại được đặc tả chi tiết ở cấp bit.
- **Chương tiếp theo:** Chương 4.1.3 — CPU Organization & Instruction Execution Cycle.

---

### Liên kết chương (Cross References)

- **Chương trước:** 4.1.1 — Digital Logic Overview (các mạch tổ hợp/tuần tự học ở đó chính là vật liệu xây dựng mạch giải mã và thực thi lệnh ở chương này).
- **Chương tiếp theo:** 4.1.3 — CPU Organization & Instruction Execution Cycle, nơi quy trình giải mã khái niệm ở Mục 8 được hiện thực hóa đầy đủ thành chu trình fetch–decode–execute chạy trên phần cứng thực.
- **Chương liên quan xa hơn:** Volume 1, Chương 1.5 (đếm tổ hợp, tái sử dụng ở Mục 7); Volume 2, Part III — Programming Paradigms (vai trò compiler dịch mã nguồn sang ISA đích, Mục 3 và 11); Chương 4.1.7 — SIMD (ví dụ tiêu biểu của domain-specific ISA extension, Mục 12).
- **Vị trí trong Knowledge Graph:** Nút thứ hai của Volume 04, Part I; phụ thuộc trực tiếp vào Chương 4.1.1; là điều kiện tiên quyết bắt buộc cho Chương 4.1.3 và toàn bộ các chương về microarchitecture còn lại của Part I.

---

*Hết Chương 4.1.2. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`. Mọi ví dụ mã hóa/giải mã và chương trình đồ chơi đều được kiểm chứng bằng Python thực thi thực tế (Mục 9–10). Đang chờ rà soát trước khi tiếp tục sang Chương 4.1.3 — CPU Organization & Instruction Execution Cycle.*
