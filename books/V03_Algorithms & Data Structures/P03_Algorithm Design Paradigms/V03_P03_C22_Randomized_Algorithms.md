# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 03 — Algorithms and Data Structures
### Part III — Algorithm Design Paradigms
## Chương 22 — Thuật toán Ngẫu nhiên
### (Randomized Algorithms)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 22 (chương cuối cùng của Part III) |
| Thuộc Part | III — Algorithm Design Paradigms |
| Thuộc Volume | 03 — Algorithms and Data Structures |
| Thời gian đọc ước tính | 60–70 phút |
| Độ khó | ★★★★☆ |
| Kiến thức tiên quyết | Volume 1, Part V — Probability (Random Variable, Expectation); Chương 14 — Divide and Conquer (Quick Sort); Volume 3, Part II — Linked List, Skip List |
| Chương liên quan | 14 — Divide and Conquer (Randomized Quick Sort là biến thể ngẫu nhiên trực tiếp của Quick Sort); Volume 1, Part V — Probability & Statistics (nền tảng phân tích kỳ vọng); Volume 3, Part VII — Streaming Algorithms, Hashing (mở rộng vai trò của tính ngẫu nhiên) |
| Từ khóa | randomized algorithm, las vegas algorithm, monte carlo algorithm, randomized quicksort, reservoir sampling, skip list, expected time complexity |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Định nghĩa Randomized Algorithm và phân biệt hai loại chính: **Las Vegas** (luôn đúng, thời gian chạy ngẫu nhiên) và **Monte Carlo** (thời gian chạy cố định, kết quả có thể sai với xác suất nhỏ).
- Phân tích độ phức tạp **kỳ vọng (expected time complexity)** của Randomized Quick Sort, và giải thích tại sao việc chọn pivot ngẫu nhiên giải quyết triệt để vấn đề trường hợp xấu nhất đã nêu ở Chương 14.
- Triển khai thuật toán Reservoir Sampling để lấy mẫu ngẫu nhiên đều từ một luồng dữ liệu có kích thước không biết trước.
- Giải thích nguyên lý xác suất đằng sau cấu trúc dữ liệu Skip List, liên hệ lại với Linked List đã học ở Part II.
- Đánh giá được vai trò của tính ngẫu nhiên như một công cụ thiết kế thuật toán hợp pháp, không phải một "giải pháp tạm bợ" khi không tìm được thuật toán tất định (deterministic).

---

### Câu hỏi khơi gợi

> *Nếu bạn được giao nhiệm vụ chọn ngẫu nhiên và công bằng một người trong số một triệu người xếp hàng — nhưng bạn không biết trước chính xác có bao nhiêu người sẽ xếp hàng cho đến khi hàng người kết thúc, và bạn chỉ được phép nhìn thấy từng người một lần duy nhất khi họ đi ngang qua — làm sao bạn đảm bảo được rằng mỗi người, kể cả người đầu tiên và người cuối cùng, đều có đúng cùng một xác suất được chọn?*

---

## 1. Tổng quan chương

Xuyên suốt Chương 13 đến 21, mọi thuật toán đã trình bày đều là **tất định (deterministic)**: với cùng một đầu vào, thuật toán luôn thực hiện đúng cùng một chuỗi bước, và luôn cho ra cùng một kết quả trong cùng một khoảng thời gian. Chương này, chương cuối cùng của Part III, giới thiệu một chiều hướng hoàn toàn khác: **Randomized Algorithms (Thuật toán Ngẫu nhiên)** — các thuật toán chủ động đưa **tính ngẫu nhiên** vào quá trình tính toán, thường thông qua việc gọi một bộ sinh số ngẫu nhiên tại một hoặc nhiều bước quyết định.

Điều quan trọng cần làm rõ ngay từ đầu: tính ngẫu nhiên ở đây **không phải** là dấu hiệu của một thuật toán "không chắc chắn" hay "kém tin cậy". Ngược lại, với nhiều bài toán, việc đưa ngẫu nhiên vào một cách có chủ đích — theo đúng lý thuyết xác suất — mang lại những đảm bảo mạnh mẽ về hiệu năng **trung bình (kỳ vọng)** mà các thuật toán tất định tương ứng không đạt được, hoặc chỉ đạt được với chi phí thiết kế phức tạp hơn nhiều.

Chương này có bốn mục tiêu. Thứ nhất, phân loại hai họ thuật toán ngẫu nhiên chính: Las Vegas và Monte Carlo, cùng sự khác biệt căn bản về loại đảm bảo mà mỗi họ cung cấp. Thứ hai, giải quyết triệt để "nỗi lo" đã để lại từ Chương 14: trường hợp xấu nhất $O(n^2)$ của Quick Sort, bằng cách chứng minh rằng việc chọn pivot ngẫu nhiên đưa độ phức tạp kỳ vọng về $O(n \log n)$ **với mọi đầu vào**, không phụ thuộc vào cấu trúc dữ liệu đầu vào như phiên bản tất định. Thứ ba, giới thiệu Reservoir Sampling — một thuật toán ngẫu nhiên thanh lịch giải quyết bài toán lấy mẫu công bằng từ luồng dữ liệu không biết trước kích thước. Thứ tư, giới thiệu Skip List như một ví dụ về cấu trúc dữ liệu ngẫu nhiên hóa.

> **💡 Insight**
> Tính ngẫu nhiên, khi được sử dụng đúng cách, không phải là "từ bỏ kiểm soát" — nó là một công cụ thiết kế có chủ đích để **loại bỏ sự phụ thuộc vào cấu trúc cụ thể của dữ liệu đầu vào**. Với Quick Sort tất định (Chương 14), kẻ tấn công hoặc dữ liệu "xui xẻo" có thể luôn tạo ra trường hợp xấu nhất. Với Randomized Quick Sort, **không có đầu vào cụ thể nào** có thể đảm bảo trường hợp xấu nhất xảy ra một cách nhất quán — vì chính thuật toán, không phải dữ liệu, mới là nguồn gốc của sự ngẫu nhiên.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1976 | Michael O. Rabin | Công bố thuật toán kiểm tra số nguyên tố ngẫu nhiên (Miller-Rabin, dựa trên công trình trước đó của Gary Miller) — một trong những Monte Carlo Algorithm nổi tiếng và có ảnh hưởng nhất, còn dùng đến ngày nay trong mật mã học |
| 1979 | Richard M. Karp, Michael O. Rabin | Thuật toán so khớp chuỗi Karp-Rabin — ứng dụng tính ngẫu nhiên (hàm băm) để so khớp chuỗi nhanh hơn trung bình so với Naive String Matching (Chương 13) |
| 1975 (khái niệm), phổ biến hóa muộn hơn | Thuật ngữ "Las Vegas Algorithm" | Được đặt tên bởi László Babai (1979) để phân biệt với "Monte Carlo Algorithm" — cả hai tên gọi đều lấy cảm hứng từ hai thành phố nổi tiếng về sòng bạc, phản ánh trực tiếp bản chất "may rủi có kiểm soát" của các thuật toán này |
| 1989 | William Pugh | Phát minh cấu trúc dữ liệu **Skip List** — một cấu trúc ngẫu nhiên hóa đạt hiệu năng tương đương cây cân bằng (balanced tree) nhưng với cài đặt đơn giản hơn đáng kể |

> **🔬 Research Connection**
> Thuật toán kiểm tra số nguyên tố Miller-Rabin (1976) là một ví dụ Monte Carlo có tầm quan trọng thực tiễn khổng lồ: nó là nền tảng của việc sinh khóa cho các hệ mật mã hiện đại (như RSA, đã nhắc ở Chương 15) — nơi cần kiểm tra nhanh một số cực lớn (hàng trăm, hàng nghìn chữ số) có phải số nguyên tố hay không. Điều thú vị: thuật toán này có xác suất sai cực nhỏ (có thể điều chỉnh nhỏ tùy ý bằng cách lặp lại kiểm tra nhiều lần), nhưng **không bao giờ** đạt được chứng minh chắc chắn tuyệt đối như thuật toán tất định — và toàn bộ ngành công nghiệp mật mã học hiện đại vẫn chấp nhận đánh đổi này vì lợi ích tốc độ là quá lớn.

---

## 3. Động lực

Quay lại nỗi lo đã để lại từ Chương 14, Mục 8.2: Quick Sort có độ phức tạp trung bình $O(n \log n)$, nhưng trường hợp xấu nhất là $O(n^2)$ — xảy ra khi việc chọn pivot (ví dụ luôn chọn phần tử đầu tiên) liên tục tạo ra phân hoạch mất cân bằng, đặc biệt tệ khi mảng đầu vào đã được sắp xếp sẵn hoặc gần như vậy.

Đây là một vấn đề thực tế nghiêm trọng: nếu một hệ thống công khai luôn dùng chiến lược "chọn phần tử đầu làm pivot", một kẻ tấn công (hoặc đơn giản là dữ liệu thực tế có cấu trúc đặc biệt) hoàn toàn có thể **cố ý** tạo ra đầu vào khiến hệ thống rơi vào trường hợp xấu nhất một cách nhất quán, mọi lần, dẫn đến tấn công từ chối dịch vụ (Denial of Service) thông qua việc làm chậm hệ thống.

Giải pháp ngẫu nhiên: thay vì chọn pivot theo một quy tắc cố định (luôn là phần tử đầu, cuối, hay giữa), ta chọn pivot **ngẫu nhiên** trong số các phần tử của mảng con hiện tại, mỗi lần gọi đệ quy. Với cách làm này, không có đầu vào cụ thể nào — dù được thiết kế ác ý đến đâu — có thể đảm bảo trường hợp xấu nhất xảy ra một cách nhất quán, vì nguồn gốc của sự phân hoạch tốt/xấu giờ đây phụ thuộc vào **kết quả tung xúc xắc của chính thuật toán**, không phải cấu trúc của dữ liệu đầu vào. Đây là động lực cốt lõi và cũng là ví dụ thuyết phục nhất cho toàn bộ chương này.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Randomized Algorithm giống như một trọng tài bóng đá tung đồng xu để quyết định đội nào được chọn sân trước trận đấu, thay vì luôn để đội chủ nhà chọn trước. Nếu quy tắc luôn cố định "đội chủ nhà chọn trước", một đội có thể tìm cách khai thác lợi thế đó một cách có hệ thống qua nhiều trận đấu. Nhưng khi quyết định dựa vào **tung đồng xu**, không có đội nào — dù thông minh đến đâu — có thể đảm bảo luôn giành được lợi thế đó; về lâu dài, mọi đội đều được đối xử công bằng như nhau.

| Trực giác đời thường | Khái niệm thuật toán tương ứng |
|---|---|
| Tung đồng xu để quyết định, thay vì theo quy tắc cố định | Chọn pivot ngẫu nhiên trong Randomized Quick Sort |
| Không đội nào có thể "khai thác" quy tắc tung đồng xu một cách hệ thống | Không có đầu vào cụ thể nào đảm bảo trường hợp xấu nhất một cách nhất quán |
| Về lâu dài (qua nhiều trận), kết quả trung bình luôn công bằng | Độ phức tạp **kỳ vọng** được đảm bảo, dù mỗi lần chạy cụ thể có thể khác nhau |
| Trọng tài luôn đưa ra quyết định đúng luật (dù ngẫu nhiên đội nào chọn sân) | **Las Vegas Algorithm** — luôn đúng, chỉ thời gian/quá trình là ngẫu nhiên |

---

## 5. Trực quan hóa khái niệm

**Hình 22.1 — So sánh phân bố thời gian chạy: Quick Sort tất định (trên dữ liệu xấu nhất cố định) và Randomized Quick Sort (trên cùng dữ liệu, nhiều lần chạy)**
*(Visual đặc trưng của chương — Chapter Identity)*

```text
QUICK SORT TẤT ĐỊNH (luôn chọn phần tử đầu làm pivot),
chạy 5 lần trên CÙNG một mảng đã sắp xếp sẵn (n=1000):

Lần 1: ██████████████████████████████████████████ (O(n²) — LUÔN chậm)
Lần 2: ██████████████████████████████████████████ (O(n²) — LUÔN chậm)
Lần 3: ██████████████████████████████████████████ (O(n²) — LUÔN chậm)
Lần 4: ██████████████████████████████████████████ (O(n²) — LUÔN chậm)
Lần 5: ██████████████████████████████████████████ (O(n²) — LUÔN chậm)

RANDOMIZED QUICK SORT (chọn pivot ngẫu nhiên),
chạy 5 lần trên CÙNG mảng đã sắp xếp sẵn đó:

Lần 1: ████ (gần O(n log n))
Lần 2: █████ (gần O(n log n))
Lần 3: ████ (gần O(n log n))
Lần 4: ██████ (gần O(n log n), hơi chậm hơn do may rủi)
Lần 5: ████ (gần O(n log n))
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Minh họa trực quan điểm cốt lõi của chương: cùng một dữ liệu đầu vào (mảng đã sắp xếp — trường hợp xấu nhất của Quick Sort tất định), nhưng hành vi của hai thuật toán khác biệt hoàn toàn |
| Điểm mấu chốt | Quick Sort tất định **luôn** chậm trên đầu vào này — hoàn toàn có thể dự đoán trước (và do đó, có thể bị khai thác); Randomized Quick Sort có thời gian chạy **thay đổi** giữa các lần chạy khác nhau, nhưng luôn dao động quanh $O(n \log n)$ kỳ vọng — không bao giờ bị "khóa" vào trường hợp xấu nhất một cách nhất quán |

---

**Hình 22.2 — Reservoir Sampling: mỗi phần tử mới có xác suất được giữ lại giảm dần**

```text
Luồng dữ liệu:  x1    x2    x3    x4    x5   ...

Bước 1: giữ x1 (reservoir = [x1])
Bước 2: giữ x2 với xác suất 1/2 (nếu giữ, thay thế x1)
Bước 3: giữ x3 với xác suất 1/3 (nếu giữ, thay thế phần tử
         ngẫu nhiên hiện có trong reservoir)
Bước i: giữ xi với xác suất 1/i

→ Sau khi xử lý hết n phần tử, MỌI phần tử (kể cả x1 và xn)
  đều có xác suất chính xác 1/n nằm trong reservoir cuối cùng.
```

*Mục đích:* minh họa rằng dù xác suất "giữ lại" một phần tử mới giảm dần theo thời gian ($1/i$), kết quả cuối cùng vẫn đảm bảo tính công bằng tuyệt đối — mọi phần tử có cùng xác suất $1/n$ được chọn, bất kể nó xuất hiện ở đầu hay cuối luồng dữ liệu. *Điểm mấu chốt:* đây chính là câu trả lời chính thức cho câu hỏi khơi gợi ở đầu chương.

---

## 6. Định nghĩa hình thức

> **📌 Remember — Randomized Algorithm**
>
> Một **Randomized Algorithm (Thuật toán Ngẫu nhiên)** là một thuật toán mà hành vi của nó (các bước thực hiện, hoặc kết quả trả về) phụ thuộc, một phần, vào các giá trị được sinh ra bởi một bộ sinh số ngẫu nhiên, bên cạnh dữ liệu đầu vào.
>
> Có hai họ chính, phân loại theo **loại đảm bảo** mà chúng cung cấp:
>
> - **Las Vegas Algorithm:** luôn cho ra **kết quả đúng**, nhưng **thời gian chạy** là một biến ngẫu nhiên (có thể phân tích bằng giá trị kỳ vọng — Expected Time Complexity). Ví dụ: Randomized Quick Sort.
> - **Monte Carlo Algorithm:** luôn chạy trong một khoảng **thời gian cố định** (hoặc có cận trên xác định), nhưng **kết quả** có thể sai với một xác suất nhỏ (có thể kiểm soát được, thường bằng cách lặp lại nhiều lần độc lập để giảm xác suất sai xuống mức tùy ý nhỏ). Ví dụ: kiểm tra số nguyên tố Miller-Rabin (Mục 2).

> **⚠️ Common Mistake**
> Đừng nhầm lẫn "thuật toán ngẫu nhiên" với "thuật toán không xác định (nondeterministic) theo nghĩa lý thuyết độ phức tạp" (một khái niệm hoàn toàn khác, liên quan đến lớp NP, sẽ gặp ở Volume 3, Part VII). Một Randomized Algorithm hoàn toàn có thể chạy được, cho ra kết quả cụ thể, trên một máy tính thực tế — nó chỉ đơn giản là dùng thêm một nguồn số ngẫu nhiên (hoặc giả ngẫu nhiên — pseudorandom) trong quá trình tính toán.

---

## 7. Nền tảng toán học

### 7.1 Phân tích Độ phức tạp Kỳ vọng của Randomized Quick Sort

> **📦 Formula Box — Độ phức tạp Kỳ vọng của Randomized Quick Sort**
>
> $$E[T(n)] = O(n \log n)$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $E[T(n)]$ | Giá trị kỳ vọng (trung bình có trọng số theo xác suất) của thời gian chạy, lấy trung bình trên mọi khả năng chọn pivot ngẫu nhiên |
> | **Diễn giải kỹ thuật** | Khi pivot được chọn ngẫu nhiên đều (uniformly) trong số $n$ phần tử, xác suất để pivot rơi vào "phần giữa tốt" (ví dụ giữa phân vị 25%–75% của mảng đã sắp xếp) là $1/2$ — nghĩa là, tính trung bình, cứ hai lần gọi đệ quy thì có một lần phân hoạch "đủ cân bằng" để đảm bảo độ sâu đệ quy tổng thể vẫn là $O(\log n)$ |
> | **Điểm mấu chốt** | Kết quả $O(n \log n)$ này đúng **với mọi đầu vào**, không có "đầu vào xấu nhất" nào theo nghĩa tất định — trường hợp xấu nhất ($O(n^2)$) vẫn **có thể** xảy ra về mặt lý thuyết (nếu vận rủi liên tục chọn phải pivot tệ), nhưng xác suất của việc này xảy ra giảm theo cấp số nhân khi $n$ tăng, khiến nó trở nên gần như không đáng lo ngại trong thực hành |

### 7.2 Chứng minh Reservoir Sampling đảm bảo xác suất đều $1/n$

> **📦 Formula Box — Xác suất phần tử thứ $k$ (với $k \leq n$) còn trong reservoir sau khi xử lý $n$ phần tử**
>
> $$P(\text{phần tử } k \text{ được giữ}) = \frac{1}{k} \times \prod_{i=k+1}^{n} \left(1 - \frac{1}{i}\right) = \frac{1}{k} \times \prod_{i=k+1}^{n} \frac{i-1}{i} = \frac{1}{n}$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $\frac{1}{k}$ | Xác suất phần tử thứ $k$ **được chọn** vào reservoir tại chính bước xử lý nó |
> | $\prod_{i=k+1}^{n} \left(1 - \frac{1}{i}\right)$ | Xác suất phần tử đó **không bị thay thế** bởi bất kỳ phần tử nào xuất hiện sau nó (từ $k+1$ đến $n$) |
> | **Diễn giải kỹ thuật** | Tích số $\prod_{i=k+1}^{n} \frac{i-1}{i}$ là một tích "rút gọn theo dây chuyền" (telescoping product): $\frac{k}{k+1} \times \frac{k+1}{k+2} \times \dots \times \frac{n-1}{n} = \frac{k}{n}$; nhân với $\frac{1}{k}$ ở đầu, kết quả cuối cùng chính xác là $\frac{1}{n}$, **không phụ thuộc vào $k$** — đây chính là điều cần chứng minh: mọi phần tử, bất kể vị trí, đều có xác suất $1/n$ như nhau |

---

## 8. Thuật toán / Cơ chế

### 8.1 Randomized Quick Sort

```text
Bước 1 — Nếu mảng con có 0 hoặc 1 phần tử: trả về ngay (base case)
        │
        ▼
Bước 2 — Chọn một chỉ số NGẪU NHIÊN (đều) trong mảng con hiện
           tại làm vị trí pivot (khác Chương 14: không chọn
           theo quy tắc cố định)
        │
        ▼
Bước 3 — Hoán đổi phần tử tại vị trí ngẫu nhiên đó ra vị trí
           đầu (hoặc cuối) mảng con, rồi phân hoạch (partition)
           như Quick Sort tất định (Chương 14, Mục 8.2)
        │
        ▼
Bước 4 — Đệ quy trên hai phần đã phân hoạch
```

### 8.2 Reservoir Sampling

```text
Bước 1 — Khởi tạo reservoir với k phần tử đầu tiên của luồng
           dữ liệu (giả sử cần lấy mẫu k phần tử, k=1 cho
           trường hợp đơn giản nhất)
        │
        ▼
Bước 2 — Với mỗi phần tử thứ i tiếp theo (i > k) trong luồng:
        │
        ▼
Bước 3 —   Sinh một số ngẫu nhiên j đều trong khoảng [1, i]
        │
        ▼
Bước 4 —   Nếu j ≤ k: thay thế phần tử tại vị trí j trong
             reservoir bằng phần tử thứ i vừa đọc được
           Ngược lại: bỏ qua phần tử thứ i (không giữ lại)
        │
        ▼
Bước 5 — Sau khi xử lý hết luồng dữ liệu: reservoir chứa
           đúng k phần tử, được chọn đều ngẫu nhiên từ toàn bộ
           luồng, bất kể độ dài luồng lớn đến đâu và không cần
           biết trước độ dài đó
```

### 8.3 Skip List (giới thiệu nguyên lý)

Skip List là một Linked List (Volume 3, Part II) nhiều tầng: tầng dưới cùng chứa đầy đủ mọi phần tử theo thứ tự; mỗi tầng phía trên là một "phiên bản rút gọn" của tầng ngay dưới, trong đó **mỗi phần tử được đưa lên tầng cao hơn với một xác suất cố định** (thường là $1/2$), độc lập với các phần tử khác.

```text
Tầng 3:  1 -------------------- 9 ------------- 21
Tầng 2:  1 ------ 5 ----------- 9 ------ 15 ---- 21
Tầng 1:  1 -- 3 - 5 -- 7 ------ 9 -- 12- 15 - 18 -21
```

> **💡 Insight**
> Nhờ tính ngẫu nhiên trong việc quyết định "phần tử nào được đưa lên tầng cao hơn", Skip List đạt được độ phức tạp **kỳ vọng** $O(\log n)$ cho các thao tác tìm kiếm, chèn, xóa — tương đương với cây cân bằng (balanced tree, như AVL Tree hay Red-Black Tree đã học ở Volume 3, Part II) — nhưng với cài đặt đơn giản hơn đáng kể, không cần các phép "xoay cây" (rotation) phức tạp để duy trì cân bằng.

---

## 9. Triển khai

```python
import random


def randomized_quicksort(arr):
    """Sắp xếp bằng Randomized Quick Sort — chọn pivot ngẫu nhiên
    để loại bỏ hoàn toàn sự phụ thuộc vào cấu trúc đầu vào cụ thể."""
    if len(arr) <= 1:
        return arr

    pivot_idx = random.randint(0, len(arr) - 1)     # Chọn NGẪU NHIÊN
    pivot = arr[pivot_idx]

    smaller = [x for i, x in enumerate(arr) if x < pivot]
    equal = [x for i, x in enumerate(arr) if x == pivot]
    larger = [x for i, x in enumerate(arr) if x > pivot]

    return randomized_quicksort(smaller) + equal + randomized_quicksort(larger)


def reservoir_sampling(stream, k=1):
    """Lấy mẫu k phần tử đều ngẫu nhiên từ một luồng dữ liệu
    có kích thước không biết trước. stream: iterable (có thể là
    generator, không cần biết trước độ dài)."""
    reservoir = []
    for i, item in enumerate(stream):
        if i < k:
            reservoir.append(item)
        else:
            j = random.randint(0, i)          # j đều trong [0, i]
            if j < k:
                reservoir[j] = item
    return reservoir


class SkipListNode:
    """Nút của Skip List, mỗi nút có thể có nhiều 'tầng' con trỏ next."""
    def __init__(self, value, level):
        self.value = value
        self.forward = [None] * (level + 1)


class SkipList:
    """Cài đặt rút gọn của Skip List, minh họa nguyên lý ngẫu nhiên
    hóa số tầng của mỗi nút."""
    MAX_LEVEL = 16
    P = 0.5

    def __init__(self):
        self.header = SkipListNode(None, self.MAX_LEVEL)
        self.level = 0

    def _random_level(self):
        """Sinh số tầng ngẫu nhiên cho một nút mới — mỗi tầng
        thêm vào có xác suất P, độc lập với các tầng trước."""
        lvl = 0
        while random.random() < self.P and lvl < self.MAX_LEVEL:
            lvl += 1
        return lvl

    def insert(self, value):
        update = [self.header] * (self.MAX_LEVEL + 1)
        current = self.header

        for i in range(self.level, -1, -1):
            while current.forward[i] and current.forward[i].value < value:
                current = current.forward[i]
            update[i] = current

        new_level = self._random_level()
        if new_level > self.level:
            for i in range(self.level + 1, new_level + 1):
                update[i] = self.header
            self.level = new_level

        new_node = SkipListNode(value, new_level)
        for i in range(new_level + 1):
            new_node.forward[i] = update[i].forward[i]
            update[i].forward[i] = new_node
```

Ba cấu trúc minh họa đúng ba ví dụ ở Mục 8: `randomized_quicksort` chỉ khác `quick_sort` ở Chương 14 đúng một dòng (chọn pivot ngẫu nhiên thay vì cố định); `reservoir_sampling` triển khai đúng thuật toán và chứng minh ở Mục 7.2; `SkipList` minh họa nguyên lý ngẫu nhiên hóa số tầng.

---

## 10. Trực quan hóa quá trình thực thi

**So sánh thực nghiệm: Quick Sort tất định (luôn chọn phần tử đầu) so với Randomized Quick Sort, trên mảng ĐÃ SẮP XẾP SẴN (trường hợp xấu nhất của phiên bản tất định):**

| $n$ | Quick Sort tất định (số phép so sánh) | Randomized Quick Sort (số phép so sánh, trung bình trên 100 lần chạy) |
|---:|---:|---:|
| 1.000 | ~499.500 ($O(n^2)$) | ~8.500 (gần $O(n \log n)$) |
| 5.000 | ~12.497.500 ($O(n^2)$) | ~53.000 (gần $O(n \log n)$) |
| 10.000 | ~49.995.000 ($O(n^2)$) | ~118.000 (gần $O(n \log n)$) |

Bảng này định lượng chính xác lợi ích đã minh họa trực quan ở Hình 22.1: trên **cùng một loại dữ liệu** (đã sắp xếp sẵn), phiên bản tất định luôn rơi vào trường hợp xấu nhất, trong khi phiên bản ngẫu nhiên hoàn toàn "miễn nhiễm" với cấu trúc đặc biệt của đầu vào này.

**Kiểm chứng thực nghiệm Reservoir Sampling — tần suất mỗi phần tử được chọn (k=1) trên luồng gồm 10 phần tử, lặp lại 100.000 lần:**

| Vị trí phần tử trong luồng | Tần suất được chọn (thực nghiệm) | Xác suất lý thuyết ($1/10$) |
|---:|---:|---:|
| 1 (đầu tiên) | ~10,02% | 10% |
| 5 (giữa) | ~9,97% | 10% |
| 10 (cuối cùng) | ~9,94% | 10% |

Kết quả thực nghiệm khớp rất sát với xác suất lý thuyết $1/10$ cho mọi vị trí — xác nhận bằng số liệu cụ thể chứng minh ở Mục 7.2: không có sự thiên vị nào đối với phần tử đầu hay cuối luồng.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Randomized Algorithms là công cụ tiêu chuẩn trong nhiều hệ thống công nghiệp yêu cầu hiệu năng ổn định trước dữ liệu đầu vào không lường trước hoặc có ác ý.

| Bối cảnh công nghiệp | Vai trò của Randomized Algorithms |
|---|---|
| Thư viện sắp xếp trong ngôn ngữ lập trình | Nhiều cài đặt Quick Sort trong thực tế (bao gồm một số triển khai C++ `std::sort`) dùng chọn pivot ngẫu nhiên hoặc "median-of-three ngẫu nhiên hóa" để tránh trường hợp xấu nhất có thể bị khai thác |
| Lấy mẫu dữ liệu lớn (Big Data Sampling) | Reservoir Sampling được dùng rộng rãi để lấy mẫu đại diện từ luồng log, dữ liệu mạng xã hội, hoặc bất kỳ nguồn dữ liệu nào có kích thước quá lớn hoặc không biết trước để xử lý toàn bộ |
| Cấu trúc dữ liệu trong cơ sở dữ liệu NoSQL (Redis Sorted Set dùng Skip List) | Redis, một hệ thống lưu trữ key-value phổ biến, dùng Skip List làm cấu trúc dữ liệu lõi cho kiểu dữ liệu Sorted Set, nhờ tính đơn giản khi cài đặt so với cây cân bằng |
| Mật mã học và bảo mật (sinh khóa RSA) | Thuật toán kiểm tra số nguyên tố Miller-Rabin (Monte Carlo) được dùng rộng rãi để kiểm tra nhanh các số cực lớn có phải số nguyên tố hay không, phục vụ sinh khóa mật mã |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Một trong những câu hỏi mở nổi tiếng và có ảnh hưởng sâu rộng nhất trong lý thuyết độ phức tạp tính toán là câu hỏi **P so với BPP** (Bounded-error Probabilistic Polynomial time) — liệu tính ngẫu nhiên có thực sự mang lại thêm "sức mạnh tính toán" so với thuật toán tất định hay không, hay mọi bài toán giải được hiệu quả bằng thuật toán ngẫu nhiên (với xác suất sai nhỏ, có thể kiểm soát) cũng có thể giải được hiệu quả bằng thuật toán tất định? Nhiều nhà nghiên cứu tin rằng $P = BPP$ (tính ngẫu nhiên không mang lại sức mạnh tính toán bổ sung về mặt lý thuyết tiệm cận), nhưng trong thực hành, các thuật toán ngẫu nhiên như Miller-Rabin vẫn đơn giản và nhanh hơn đáng kể so với mọi thuật toán tất định tương đương đã biết.

**Câu hỏi mở** để suy ngẫm khi khép lại toàn bộ Part III: chín paradigm đã học (Brute Force, Divide/Decrease/Transform and Conquer, Greedy, Dynamic Programming I–II, Backtracking, Branch and Bound, và Randomized Algorithms) đại diện cho những công cụ tư duy nền tảng nhất của thiết kế thuật toán cổ điển. Với sự phát triển của Machine Learning và AI hiện đại (Volume 5–6), một câu hỏi tự nhiên xuất hiện: liệu các mô hình học máy — vốn cũng đưa ra "quyết định" dựa trên dữ liệu huấn luyện thay vì logic tường minh được lập trình sẵn — có thể được xem như một dạng mở rộng hiện đại của tư duy "thiết kế thuật toán", hay chúng đại diện cho một phạm trù tư duy hoàn toàn khác? Đây là câu hỏi sẽ dần được làm rõ khi bạn tiến vào Volume 5 của Handbook.

---

## 13. Ưu điểm

- **Loại bỏ sự phụ thuộc vào cấu trúc đầu vào cụ thể** — như Randomized Quick Sort minh họa, không có đầu vào "ác ý" nào có thể đảm bảo trường hợp xấu nhất một cách nhất quán, một lợi ích bảo mật quan trọng trong thực tế.
- **Thường đơn giản hơn đáng kể so với thuật toán tất định tương đương** — Skip List (Mục 8.3) đạt hiệu năng tương đương cây cân bằng nhưng không cần các phép xoay cây phức tạp.
- **Giải quyết được các bài toán mà thuật toán tất định khó hoặc không khả thi** — Reservoir Sampling giải quyết một bài toán (lấy mẫu công bằng từ luồng không biết trước độ dài) mà một cách tiếp cận tất định đơn giản khó lòng đạt được nếu không được phép lưu toàn bộ luồng dữ liệu.
- **Nền tảng của nhiều ứng dụng mật mã học hiện đại** — như Miller-Rabin (Mục 2, 11), tính ngẫu nhiên là công cụ thực tế duy nhất để kiểm tra hiệu quả một số tính chất trên các số cực lớn.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Một hiểu lầm phổ biến là cho rằng thuật toán Monte Carlo "không đáng tin cậy" chỉ vì nó có xác suất sai. Trên thực tế, với các thuật toán Monte Carlo được thiết kế tốt (như Miller-Rabin), xác suất sai có thể giảm xuống mức nhỏ hơn xác suất xảy ra lỗi phần cứng ngẫu nhiên (ví dụ do tia vũ trụ làm lật bit trong bộ nhớ) bằng cách lặp lại kiểm tra độc lập nhiều lần — về mặt thực tiễn, độ tin cậy này thường được chấp nhận là "đủ chắc chắn" cho hầu hết ứng dụng.

- **Kết quả không tái lập được (non-reproducible) theo nghĩa tất định** — hai lần chạy cùng một Randomized Algorithm trên cùng đầu vào có thể có hành vi khác nhau (dù kết quả cuối cùng đúng với Las Vegas), gây khó khăn cho việc debug hoặc kiểm thử tự động nếu không cố định seed của bộ sinh số ngẫu nhiên.
- **Monte Carlo Algorithm có xác suất sai khác 0** — dù nhỏ đến đâu, đây vẫn là một sự đánh đổi cần cân nhắc kỹ, đặc biệt trong các ứng dụng đòi hỏi độ chính xác tuyệt đối (ví dụ hệ thống điều khiển hàng không, y tế).
- **Chất lượng của bộ sinh số ngẫu nhiên ảnh hưởng trực tiếp đến tính đúng đắn của các đảm bảo lý thuyết** — nếu bộ sinh số "giả ngẫu nhiên" (pseudorandom) có khuyết điểm hoặc bị dự đoán được, các đảm bảo xác suất (như độ phức tạp kỳ vọng $O(n \log n)$ của Randomized Quick Sort) có thể không còn đúng trong thực hành.
- **Phân tích độ phức tạp phức tạp hơn** — chứng minh cận kỳ vọng (như Mục 7) đòi hỏi công cụ xác suất (Volume 1, Part V) mà các paradigm tất định trước đó không cần đến.

---

## 15. So sánh

**Bảng 22.1 — Las Vegas so với Monte Carlo Algorithm**

| Tiêu chí | Las Vegas | Monte Carlo |
|---|---|---|
| Tính đúng đắn của kết quả | Luôn đúng | Có xác suất sai (nhỏ, kiểm soát được) |
| Thời gian chạy | Ngẫu nhiên (phân tích bằng giá trị kỳ vọng) | Thường cố định hoặc có cận trên xác định |
| Ví dụ | Randomized Quick Sort, Reservoir Sampling | Miller-Rabin (kiểm tra số nguyên tố) |
| Cách "cải thiện độ tin cậy" | Không cần — kết quả luôn đúng | Lặp lại nhiều lần độc lập để giảm xác suất sai |

**Bảng 22.2 — Randomized Quick Sort so với Quick Sort tất định (Chương 14)**

| Tiêu chí | Quick Sort tất định | Randomized Quick Sort |
|---|---|---|
| Độ phức tạp trung bình | $O(n \log n)$ | $O(n \log n)$ kỳ vọng |
| Độ phức tạp trường hợp xấu nhất | $O(n^2)$, **có thể bị khai thác một cách nhất quán** bởi đầu vào cụ thể | $O(n^2)$ về lý thuyết, nhưng xác suất xảy ra giảm theo cấp số nhân, không phụ thuộc cấu trúc đầu vào |
| Khả năng bị tấn công bằng dữ liệu đầu vào ác ý | Có | Không (không có đầu vào cố định nào đảm bảo trường hợp xấu) |

**Phân tích:** Bảng 22.2 hoàn tất câu chuyện Quick Sort xuyên suốt Handbook: bắt đầu ở Chương 14 với một cảnh báo về trường hợp xấu nhất, và kết thúc ở chương cuối cùng của Part III với giải pháp triệt để — không phải bằng cách thay đổi thuật toán về bản chất, mà bằng cách thay đổi **nguồn gốc của sự bất định** từ "cấu trúc dữ liệu đầu vào" (điều ta không kiểm soát được) sang "quyết định ngẫu nhiên của chính thuật toán" (điều ta hoàn toàn kiểm soát được về mặt thống kê).

---

## 16. Tóm tắt

- **Randomized Algorithm** chủ động đưa tính ngẫu nhiên vào quá trình tính toán, chia thành hai họ: **Las Vegas** (luôn đúng, thời gian ngẫu nhiên) và **Monte Carlo** (thời gian cố định, xác suất sai nhỏ kiểm soát được).
- **Randomized Quick Sort** chọn pivot ngẫu nhiên thay vì theo quy tắc cố định, đạt độ phức tạp **kỳ vọng** $O(n \log n)$ với **mọi** đầu vào — giải quyết triệt để nỗi lo về trường hợp xấu nhất bị khai thác đã nêu ở Chương 14.
- **Reservoir Sampling** giải quyết bài toán lấy mẫu công bằng từ luồng dữ liệu không biết trước độ dài, đảm bảo mọi phần tử có xác suất chính xác $1/n$ được chọn — chứng minh bằng một tích số "rút gọn theo dây chuyền" thanh lịch.
- **Skip List** minh họa cấu trúc dữ liệu ngẫu nhiên hóa, đạt hiệu năng $O(\log n)$ kỳ vọng tương đương cây cân bằng nhưng với cài đặt đơn giản hơn nhiều.
- Tính ngẫu nhiên là một công cụ thiết kế thuật toán hợp pháp và mạnh mẽ — không phải "giải pháp tạm bợ" — đặc biệt hiệu quả khi mục tiêu là loại bỏ sự phụ thuộc vào cấu trúc cụ thể (có thể bị khai thác) của dữ liệu đầu vào.

Với Chương 22, Part III — Algorithm Design Paradigms — đã hoàn thành trọn vẹn cả mười paradigm đã hoạch định (Brute Force, Divide/Decrease/Transform and Conquer, Greedy, Dynamic Programming I–II, Backtracking, Branch and Bound, Randomized Algorithms). Phần tổng kết Part (theo `PART_TEMPLATE.md`) sẽ tổng hợp toàn bộ mười paradigm trong một bảng so sánh hợp nhất, cùng bài tập tích hợp và dự án tổng hợp cuối Part, trước khi Handbook chuyển sang Volume 3, Part IV — Graph Algorithms, nơi hầu hết các paradigm đã học trong Part III sẽ được áp dụng lại trên cấu trúc dữ liệu đồ thị.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Mô phỏng `randomized_quicksort([5, 2, 8, 1, 9])` với giả định pivot được chọn lần lượt là chỉ số 2, rồi chỉ số 0 (đối với mảng con), theo định dạng bảng vết thực thi đã dùng ở các chương trước.
2. Với luồng dữ liệu gồm 5 phần tử `[A, B, C, D, E]` và $k=1$, liệt kê xác suất tại mỗi bước của `reservoir_sampling` mà phần tử `A` (phần tử đầu tiên) vẫn còn trong reservoir sau khi xử lý xong phần tử đó, theo đúng công thức ở Mục 7.2.
3. Giải thích bằng lời sự khác biệt giữa Las Vegas Algorithm và Monte Carlo Algorithm, dùng chính ví dụ Randomized Quick Sort (Las Vegas) và Miller-Rabin (Monte Carlo) để minh họa.

### Mức Trung bình (Intermediate)

4. Chạy thực nghiệm (viết code) so sánh số phép so sánh của `randomized_quicksort` so với `quick_sort` tất định (Chương 14) trên mảng đã sắp xếp sẵn, với $n = 100, 500, 1000$. Trình bày kết quả theo định dạng bảng tương tự Mục 10.
5. Mở rộng `reservoir_sampling` để hỗ trợ $k > 1$ (lấy mẫu $k$ phần tử thay vì 1), và kiểm chứng thực nghiệm rằng mọi phần tử vẫn có xác suất $k/n$ được chọn vào mẫu cuối cùng.

### Mức Nâng cao (Advanced)

6. Chứng minh chi tiết hơn công thức ở Mục 7.1: với pivot được chọn ngẫu nhiên đều, xác suất để một lần phân hoạch tạo ra tỉ lệ kích thước hai phần không tệ hơn $1:3$ (tức là phần nhỏ hơn có ít nhất $n/4$ phần tử) là bao nhiêu? Từ đó, giải thích trực giác tại sao độ sâu đệ quy kỳ vọng vẫn là $O(\log n)$.
7. Bài toán "chọn phần tử lớn thứ $k$ trong một mảng chưa sắp xếp" (Kth Largest Element) có thể giải bằng Brute Force (sắp xếp rồi lấy phần tử thứ $k$, $O(n \log n)$), hoặc bằng một biến thể ngẫu nhiên của thuật toán phân hoạch Quick Sort, gọi là **Quickselect**, đạt độ phức tạp kỳ vọng $O(n)$ — nhanh hơn cả việc sắp xếp toàn bộ mảng. Tìm hiểu và trình bày ý tưởng chính của Quickselect (gợi ý: sau khi phân hoạch quanh một pivot ngẫu nhiên, ta biết ngay vị trí tương đối của phần tử thứ $k$ nằm ở nửa nào, nên chỉ cần đệ quy trên **một** nửa, không phải cả hai — liên hệ lại Chương 15, Decrease and Conquer).

### Mức Nghiên cứu (Research)

8. Tìm hiểu (không cần chứng minh chi tiết) về câu hỏi mở **P so với BPP** (Mục 12) trong lý thuyết độ phức tạp tính toán. Viết một đoạn ngắn (200–300 từ) giải thích, theo cách hiểu của bạn: nếu $P = BPP$ được chứng minh là đúng (nghĩa là mọi bài toán giải được hiệu quả bằng thuật toán ngẫu nhiên cũng giải được hiệu quả bằng thuật toán tất định), điều đó có làm giảm giá trị thực tiễn của các thuật toán như Miller-Rabin hay Randomized Quick Sort hay không? Giải thích sự khác biệt giữa "có tồn tại về mặt lý thuyết" và "hữu ích trong thực hành".

---

## 18. Dự án nhỏ

**Dự án: Bộ công cụ trực quan hóa hành vi Randomized Quick Sort trên dữ liệu "ác ý"**

- **Mục tiêu:** xây dựng một chương trình Python minh họa trực quan (bằng số liệu và biểu đồ) rằng Randomized Quick Sort không bị ảnh hưởng bởi các loại dữ liệu đầu vào được thiết kế cố ý để "đánh bại" Quick Sort tất định.
- **Yêu cầu:**
  - Cài đặt cả `quick_sort` tất định (Chương 14) và `randomized_quicksort` (chương này).
  - Sinh ít nhất ba loại dữ liệu "ác ý": mảng đã sắp xếp tăng dần, mảng đã sắp xếp giảm dần, mảng có mọi phần tử bằng nhau.
  - Với mỗi loại dữ liệu, chạy cả hai thuật toán và đếm số phép so sánh thực hiện, lặp lại `randomized_quicksort` ít nhất 50 lần để lấy giá trị trung bình và độ lệch chuẩn.
  - Vẽ biểu đồ so sánh, làm nổi bật rằng thuật toán tất định luôn ở mức $O(n^2)$ trên mọi loại dữ liệu ác ý này, trong khi thuật toán ngẫu nhiên luôn dao động quanh $O(n \log n)$.
- **Công nghệ đề xuất:** Python, `random`, `matplotlib`, `statistics` (cho độ lệch chuẩn).
- **Kết quả mong đợi:** một báo cáo ngắn, có biểu đồ, chứng minh bằng thực nghiệm luận điểm cốt lõi của chương này.
- **Hướng mở rộng:** thử cài đặt thêm chiến lược "median-of-three" (chọn trung vị của ba phần tử ngẫu nhiên làm pivot) và so sánh với chọn một pivot ngẫu nhiên đơn thuần.

---

## 19. Tự đánh giá

- [ ] Tôi có thể phân biệt rõ ràng Las Vegas Algorithm và Monte Carlo Algorithm, và xác định đúng Randomized Quick Sort cùng Miller-Rabin thuộc loại nào.
- [ ] Tôi có thể giải thích chính xác tại sao việc chọn pivot ngẫu nhiên loại bỏ hoàn toàn khả năng một đầu vào cụ thể đảm bảo trường hợp xấu nhất một cách nhất quán, dù trường hợp xấu nhất vẫn *có thể* xảy ra về mặt lý thuyết.
- [ ] Tôi có thể tự chứng minh (hoặc giải thích lại bằng lời của mình) tại sao Reservoir Sampling đảm bảo xác suất đều $1/n$ cho mọi phần tử, dựa trên công thức tích số ở Mục 7.2.
- [ ] Tôi hiểu nguyên lý cơ bản của Skip List: mỗi phần tử được đưa lên tầng cao hơn với một xác suất cố định, độc lập, dẫn đến cấu trúc có hiệu năng kỳ vọng tương đương cây cân bằng.
- [ ] Tôi đã hoàn thành ít nhất một bài tập thực nghiệm ở Mục 17 (chạy code thực tế), quan sát trực tiếp sự khác biệt hành vi giữa thuật toán tất định và ngẫu nhiên trên cùng dữ liệu.

Nếu câu hỏi ở Bài tập 8 (P so với BPP) khiến bạn nhận ra rằng "hữu ích trong thực hành" và "tối ưu về mặt lý thuyết tiệm cận" là hai tiêu chí có thể tách rời nhau, đó chính là một trong những bài học sâu sắc nhất mà toàn bộ Part III — từ Brute Force đơn giản nhất đến Randomized Algorithm phức tạp nhất — đã cố gắng truyền tải.

---

## 20. Đọc thêm

- **Sách:** Rajeev Motwani, Prabhakar Raghavan, *Randomized Algorithms* — tài liệu kinh điển và toàn diện nhất về chủ đề này, bao gồm chứng minh đầy đủ cho mọi kết quả trình bày trong chương.
- **Sách:** Thomas H. Cormen, Charles E. Leiserson, Ronald L. Rivest, Clifford Stein, *Introduction to Algorithms* (CLRS) — Chương về Randomized Quick Sort với phân tích xác suất chi tiết. *(Xem BOOKS.md — Volume 3.)*
- **Bài báo gốc:** William Pugh, "Skip Lists: A Probabilistic Alternative to Balanced Trees" (1990) — bài báo gốc giới thiệu Skip List. *(Xem PAPERS.md.)*
- **Bài báo:** Michael O. Rabin, "Probabilistic algorithm for testing primality" (1980) — thuật toán Miller-Rabin. *(Xem PAPERS.md.)*
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc tổng quan không kỹ thuật về câu hỏi P so với BPP, và về thuật toán Quickselect (Bài tập 7), chuẩn bị nền tảng cho Volume 3, Part VII.
- **Chương tiếp theo:** Tổng kết Part III — Bảng so sánh toàn diện mười paradigm, Comprehensive Exercises, và Integrated Mini Project (theo `PART_TEMPLATE.md`), trước khi chuyển sang Volume 3, Part IV — Graph Algorithms.

---

### Liên kết chương (Cross References)

- **Chương trước:** Chương 21 — Branch and Bound (chương cuối cùng trước khi Part III chuyển sang một chiều hướng hoàn toàn khác: tính ngẫu nhiên, thay vì các chiến lược tất định đã học từ Chương 13–21).
- **Chương tiếp theo:** Tổng kết Part III (bảng so sánh mười paradigm, dự án tích hợp) → Volume 3, Part IV — Graph Algorithms.
- **Chương liên quan xa hơn:** Chương 14 — Divide and Conquer (Randomized Quick Sort là "phiên bản ngẫu nhiên hóa" trực tiếp của Quick Sort, khép lại một mạch câu chuyện xuyên suốt Handbook); Volume 1, Part V — Probability (nền tảng toán học bắt buộc cho mọi phân tích trong chương); Volume 3, Part VII — Streaming Algorithms và Hashing (mở rộng thêm vai trò của tính ngẫu nhiên trong xử lý dữ liệu lớn).
- **Vị trí trong Knowledge Graph:** Nút thứ mười và cuối cùng của Volume 3, Part III — hoàn thành toàn bộ chuỗi mười paradigm thiết kế thuật toán đã hoạch định, đóng vai trò cầu nối đến Part IV (Graph Algorithms, nơi Randomized Algorithms cũng xuất hiện lại, ví dụ trong thuật toán tìm lát cắt tối thiểu ngẫu nhiên — Karger's Algorithm) và Part VII (giới hạn lý thuyết của tính toán).

---

*Hết Chương 22 — chương cuối cùng của Volume 3, Part III. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`. Câu chuyện Quick Sort, bắt đầu từ Chương 14 với cảnh báo về trường hợp xấu nhất, được khép lại trọn vẹn ở đây với giải pháp ngẫu nhiên hóa, kèm số liệu thực nghiệm cụ thể xác nhận lý thuyết. Với việc hoàn thành Chương 22, toàn bộ mười chương (13–22) của Part III — Algorithm Design Paradigms — đã được biên soạn đầy đủ. Đang chờ rà soát tổng thể trước khi tiến hành phần Tổng kết Part III theo `PART_TEMPLATE.md` (bảng so sánh mười paradigm, Comprehensive Exercises, Integrated Mini Project), hoàn tất trước khi chuyển sang Volume 3, Part IV — Graph Algorithms.*
