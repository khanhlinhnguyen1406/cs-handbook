# MASTER COMPUTER SCIENCE HANDBOOK

## Volume 03 — Algorithms and Data Structures
### Part V — String Algorithms
## Chương 5.6 — Suffix Array
### (Suffix Array)

---

### Thông tin chương

| Trường | Giá trị |
|---|---|
| Chương | 5.6 |
| Thuộc Part | V — String Algorithms |
| Thuộc Volume | 03 — Algorithms and Data Structures |
| Thời gian đọc ước tính | 55–70 phút |
| Độ khó | ★★★★☆ |
| Kiến thức tiên quyết | Chương 5.5 — Trie (đặc biệt Mục 12, Bài tập 8 về Suffix Trie); Volume 03, Part I — Sorting (Merge Sort/Quick Sort), Divide and Conquer; Volume 03, Part III — Binary Search |
| Chương liên quan | 5.5 — Trie (nền tảng khái niệm hậu tố); 5.7 — Suffix Tree (cấu trúc thay thế, đánh đổi khác) |
| Từ khóa | Suffix Array, suffix, LCP Array, binary search on suffixes, substring search |

---

### Mục tiêu học tập

Sau khi hoàn thành chương này, người đọc có thể:

- Định nghĩa chính xác khái niệm **hậu tố (suffix)** của một chuỗi và cấu trúc **Suffix Array**.
- Xây dựng Suffix Array cho một chuỗi bằng phương pháp cơ bản (sắp xếp trực tiếp), phân tích độ phức tạp $O(n^2 \log n)$ của cách làm này.
- Giải thích ý tưởng của thuật toán xây dựng hiệu quả hơn ($O(n \log n)$) dựa trên kỹ thuật nhân đôi (doubling), ở mức độ trực giác.
- Sử dụng Suffix Array kết hợp Binary Search để giải bài toán tìm chuỗi con trong $O(m \log n)$.
- Định nghĩa **LCP Array** (Longest Common Prefix Array) và giải thích vai trò của nó trong việc tăng tốc các truy vấn trên Suffix Array.

---

### Câu hỏi khơi gợi

> *Ở Chương 5.5, Bài tập 8 đặt ra câu hỏi: nếu thêm mọi hậu tố của một chuỗi văn bản vào một Trie, cấu trúc thu được sẽ có mức độ chia sẻ tiền tố rất cao. Nhưng một Trie chứa hàng nghìn, hàng triệu hậu tố của một văn bản dài có thể tốn rất nhiều bộ nhớ với overhead con trỏ tại mỗi nút. Liệu có một cách biểu diễn "gọn nhẹ" hơn nhiều — có thể chỉ cần một mảng số nguyên duy nhất — vẫn cho phép ta trả lời nhanh các câu hỏi như "chuỗi con này có xuất hiện trong văn bản không" mà không cần xây dựng toàn bộ cấu trúc cây tốn kém?*

---

## 1. Tổng quan chương

Chương 5.5 kết thúc bằng việc khám phá **Suffix Trie** — một Trie chứa mọi hậu tố của một chuỗi duy nhất. Ý tưởng này rất mạnh về mặt lý thuyết, nhưng bộc lộ nhược điểm thực tế nghiêm trọng: với văn bản dài $n$ ký tự, số lượng nút trong Suffix Trie có thể lên tới $O(n^2)$ trong trường hợp xấu — mỗi hậu tố có độ dài trung bình $O(n)$, và có $n$ hậu tố, với mức chia sẻ tiền tố không đủ để bù đắp.

Chương này giới thiệu **Suffix Array** — một cấu trúc dữ liệu đơn giản hơn nhiều về mặt biểu diễn (chỉ là **một mảng số nguyên**), nhưng vẫn giữ được phần lớn sức mạnh của Suffix Trie cho các bài toán tìm kiếm chuỗi con. Ý tưởng cốt lõi: thay vì xây dựng cây, ta chỉ cần **liệt kê mọi hậu tố của chuỗi, sắp xếp chúng theo thứ tự từ điển (lexicographic order)**, rồi lưu lại **chỉ số bắt đầu** của mỗi hậu tố theo đúng thứ tự đã sắp xếp đó.

> **💡 Insight**
> Suffix Array và Suffix Trie giải quyết cùng một lớp bài toán bằng hai triết lý biểu diễn khác nhau: Trie tổ chức dữ liệu theo **cấu trúc cây tường minh** (tốn bộ nhớ nhưng thao tác trực quan), còn Suffix Array tận dụng **thứ tự sắp xếp** để "ngầm" mã hóa cùng thông tin đó trong một mảng phẳng — đổi lấy bộ nhớ tiết kiệm hơn đáng kể, với chi phí là một số truy vấn cần thêm bước Binary Search thay vì đi thẳng theo cây.

---

## 2. Bối cảnh lịch sử

| Thời điểm | Nhân vật / Sự kiện | Đóng góp |
|---|---|---|
| 1990 | Udi Manber, Gene Myers | Công bố khái niệm Suffix Array trong bài báo *"Suffix arrays: a new method for on-line string searches"*, đề xuất như một giải pháp thay thế nhẹ hơn cho Suffix Tree (khi đó đã được biết đến từ trước, xem Chương 5.7) |
| Thập niên 1990–2000 | Cộng đồng nghiên cứu Computational Biology | Suffix Array được ứng dụng rộng rãi trong các công cụ so khớp chuỗi DNA/protein quy mô lớn, nhờ tiết kiệm bộ nhớ hơn đáng kể so với Suffix Tree |
| 2003 | Karkkainen, Sanders (và các nhóm nghiên cứu độc lập khác) | Công bố các thuật toán xây dựng Suffix Array trong thời gian tuyến tính $O(n)$, cải thiện thêm so với thuật toán $O(n \log n)$ dựa trên kỹ thuật nhân đôi |

Động lực ban đầu cho sự ra đời của Suffix Array rất thực dụng: vào thời điểm 1990, Suffix Tree (dù mạnh mẽ về mặt lý thuyết) đòi hỏi lượng bộ nhớ lớn tới mức không khả thi cho các bộ gen sinh học có độ dài hàng triệu ký tự trên phần cứng thời bấy giờ. Manber và Myers đặt câu hỏi: liệu có cần **toàn bộ cấu trúc cây** hay chỉ cần **thứ tự sắp xếp của các hậu tố** là đủ để giải quyết phần lớn bài toán thực tế? Câu trả lời "chỉ cần thứ tự sắp xếp" chính là insight khai sinh ra Suffix Array.

---

## 3. Động lực

Hãy xem xét bài toán: cho một văn bản dài (ví dụ một bộ gen DNA hàng triệu ký tự), cần trả lời nhanh nhiều truy vấn dạng "chuỗi con $P$ có xuất hiện trong văn bản hay không?" — và số lượng truy vấn có thể lên tới hàng nghìn, hàng triệu lần trên cùng một văn bản cố định.

Nếu dùng KMP (Chương 5.2) hay Boyer–Moore (Chương 5.4) cho **mỗi** truy vấn riêng lẻ, mỗi lần tìm kiếm tốn ít nhất $O(n)$ — với hàng triệu truy vấn, tổng chi phí trở nên rất lớn. Ta muốn một cách tiếp cận khác: **đầu tư một lần duy nhất** để xây dựng một cấu trúc dữ liệu từ văn bản, sao cho **mỗi truy vấn sau đó** chỉ tốn thời gian tỷ lệ với độ dài pattern (thường ngắn hơn nhiều so với $n$), thay vì tỷ lệ với độ dài toàn bộ văn bản.

Quan sát mấu chốt: một chuỗi con $P$ xuất hiện trong văn bản $T$ **khi và chỉ khi** $P$ là tiền tố (prefix) của ít nhất một hậu tố (suffix) của $T$. Nếu ta sắp xếp **mọi hậu tố** của $T$ theo thứ tự từ điển, thì mọi hậu tố có chung tiền tố $P$ sẽ nằm **liền kề nhau** trong danh sách đã sắp xếp — cho phép dùng **Binary Search** để tìm nhanh phạm vi chứa các hậu tố bắt đầu bằng $P$, thay vì phải duyệt tuyến tính qua toàn bộ văn bản mỗi lần.

---

## 4. Trực giác

**Mô hình tinh thần (Mental Model) của chương này:**

> Hãy tưởng tượng bạn cắt một cuốn sách thành hàng nghìn mảnh giấy, mỗi mảnh bắt đầu từ một vị trí khác nhau trong sách và kéo dài đến hết trang cuối — đây chính là các "hậu tố". Sau đó, bạn **sắp xếp toàn bộ các mảnh giấy này theo thứ tự bảng chữ cái**, giống như sắp xếp từ trong từ điển. Suffix Array chỉ đơn giản là **danh sách số trang** (chỉ số bắt đầu) của các mảnh giấy đó, theo đúng thứ tự đã sắp xếp — bạn không cần giữ lại toàn bộ nội dung mảnh giấy, chỉ cần biết "mảnh xếp thứ $k$ bắt đầu từ trang nào trong sách gốc".

| Trực giác đời thường | Khái niệm thuật toán tương ứng |
|---|---|
| Mỗi mảnh giấy cắt từ một vị trí đến hết sách | Một **hậu tố (suffix)** của chuỗi $T$ |
| Sắp xếp các mảnh giấy theo thứ tự bảng chữ cái | Sắp xếp mọi hậu tố theo thứ tự **từ điển (lexicographic order)** |
| Danh sách "số trang" theo thứ tự đã sắp xếp | **Suffix Array** — mảng các chỉ số bắt đầu |
| Tìm nhanh mọi mảnh giấy bắt đầu bằng một cụm từ, nhờ chúng đã nằm liền kề sau khi sắp xếp | Dùng **Binary Search** trên Suffix Array để tìm phạm vi hậu tố có tiền tố là pattern cần tìm |

---

## 5. Trực quan hóa khái niệm

**Hình 5.6.1 — Liệt kê và sắp xếp mọi hậu tố của $T = \texttt{"banana"}$**

```text
Chỉ số   Hậu tố (chưa sắp xếp)
  0      banana
  1      anana
  2      nana
  3      ana
  4      na
  5      a

Sau khi sắp xếp theo thứ tự từ điển:

  Thứ tự    Hậu tố          Chỉ số gốc
    0       a               5
    1       ana             3
    2       anana           1
    3       banana          0
    4       na              4
    5       nana            2

→ Suffix Array của "banana" là: [5, 3, 1, 0, 4, 2]
```

| Trường thông tin | Nội dung |
|---|---|
| Mục đích | Minh họa trực quan hai bước xây dựng Suffix Array: liệt kê mọi hậu tố, sau đó sắp xếp và chỉ giữ lại chỉ số gốc |
| Điểm mấu chốt | Suffix Array **không lưu nội dung hậu tố** — chỉ lưu chỉ số bắt đầu. Để biết nội dung hậu tố thứ $k$, ta lấy `T[SuffixArray[k]:]` từ chuỗi gốc, tiết kiệm bộ nhớ đáng kể so với việc lưu tường minh mọi hậu tố (Suffix Trie) |

**Hình 5.6.2 — Vì sao hậu tố có chung tiền tố nằm liền kề sau khi sắp xếp**

```text
Suffix Array đã sắp xếp của "banana":
  a          ← bắt đầu bằng "a"
  ana        ← bắt đầu bằng "a"
  anana      ← bắt đầu bằng "a"
  banana
  na         ← bắt đầu bằng "n"
  nana       ← bắt đầu bằng "n"

Mọi hậu tố bắt đầu bằng "a" nằm ở các vị trí 0,1,2 (liền kề)
Mọi hậu tố bắt đầu bằng "n" nằm ở các vị trí 4,5   (liền kề)
```

---

## 6. Định nghĩa hình thức

> **📌 Remember — Suffix và Suffix Array**
>
> Cho chuỗi $T = t_0 t_1 \dots t_{n-1}$. Với mỗi chỉ số $i \in \{0, 1, \dots, n-1\}$, **hậu tố (suffix)** bắt đầu tại $i$ được định nghĩa là:
>
> $$\text{suffix}_i = t_i t_{i+1} \dots t_{n-1}$$
>
> **Suffix Array** của $T$, ký hiệu $SA$, là một hoán vị (permutation) của $\{0, 1, \dots, n-1\}$ sao cho:
>
> $$\text{suffix}_{SA[0]} < \text{suffix}_{SA[1]} < \dots < \text{suffix}_{SA[n-1]}$$
>
> theo quan hệ thứ tự từ điển (lexicographic order) giữa các chuỗi.

> **📌 Remember — LCP Array (Longest Common Prefix Array)**
>
> **LCP Array**, ký hiệu $LCP$, là một mảng độ dài $n-1$, trong đó:
>
> $$LCP[k] = \text{độ dài tiền tố chung dài nhất giữa } \text{suffix}_{SA[k]} \text{ và } \text{suffix}_{SA[k+1]}$$
>
> nghĩa là độ dài phần chung giữa hai hậu tố **liền kề nhau** trong Suffix Array đã sắp xếp.

---

## 7. Nền tảng toán học

### 7.1 Độ phức tạp xây dựng Suffix Array — phương pháp cơ bản

- **Ý nghĩa:** cách xây dựng đơn giản nhất là liệt kê toàn bộ $n$ hậu tố, rồi dùng một thuật toán sắp xếp tổng quát (ví dụ Merge Sort, Volume 03 Part III) để sắp xếp chúng.

> **📦 Formula Box — Độ phức tạp xây dựng Suffix Array (phương pháp cơ bản)**
>
> $$T_{\text{cơ bản}}(n) = O(n^2 \log n)$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $O(n \log n)$ | Số phép so sánh cần thiết của một thuật toán sắp xếp hiệu quả (Merge Sort/Quick Sort) trên $n$ phần tử |
> | $O(n)$ (nhân thêm) | Mỗi phép **so sánh hai hậu tố** (để xác định hậu tố nào "nhỏ hơn" theo thứ tự từ điển) có thể tốn tới $O(n)$ trong trường hợp xấu nhất, vì hậu tố có độ dài lên tới $n$ ký tự |
> | **Diễn giải kỹ thuật** | Đây là lý do phương pháp "liệt kê rồi sắp xếp trực tiếp" tuy đơn giản để hiểu và cài đặt, nhưng **không đủ hiệu quả** cho văn bản dài (ví dụ bộ gen hàng triệu ký tự) |

### 7.2 Ý tưởng cải tiến: kỹ thuật nhân đôi (Doubling Technique)

Thay vì so sánh trực tiếp toàn bộ hậu tố (tốn $O(n)$ mỗi lần so sánh), thuật toán nhân đôi xây dựng Suffix Array qua nhiều **vòng lặp**, mỗi vòng xử lý các hậu tố dựa trên $2^k$ ký tự đầu tiên ($k = 0, 1, 2, \dots$), tận dụng **kết quả sắp xếp của vòng trước** để sắp xếp nhanh hơn ở vòng sau, thay vì so sánh lại từ đầu. Sau $O(\log n)$ vòng lặp (vì $2^k$ tăng theo cấp số nhân, chỉ cần khoảng $\log_2 n$ vòng để $2^k \geq n$), mỗi vòng tốn $O(n \log n)$ (dùng sắp xếp hiệu quả với hàm so sánh $O(1)$ nhờ kỹ thuật đánh số thứ hạng — rank), tổng độ phức tạp trở thành:

> **📦 Formula Box — Độ phức tạp xây dựng Suffix Array (kỹ thuật nhân đôi)**
>
> $$T_{\text{doubling}}(n) = O(n \log n \cdot \log n) = O(n \log^2 n)$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $O(\log n)$ vòng lặp | Số vòng cần thiết để $2^k$ vượt qua độ dài $n$ |
> | $O(n \log n)$ mỗi vòng | Chi phí sắp xếp lại tại mỗi vòng, với hàm so sánh đã được tối ưu về $O(1)$ nhờ đánh số thứ hạng từ vòng trước |
> | **Diễn giải kỹ thuật** | Cải thiện đáng kể so với $O(n^2 \log n)$ của phương pháp cơ bản, dù vẫn chưa đạt mức tối ưu tuyệt đối |

> **🔬 Research Connection**
> Các thuật toán hiện đại hơn (ví dụ DC3/Skew Algorithm của Karkkainen và Sanders, 2003) có thể xây dựng Suffix Array trong thời gian **tuyến tính tuyệt đối** $O(n)$, dùng kỹ thuật đệ quy phức tạp hơn nhiều. Việc trình bày chi tiết các thuật toán này vượt ra ngoài phạm vi giáo trình của chương này; người đọc quan tâm sâu hơn có thể tham khảo Mục 20.

### 7.3 Độ phức tạp truy vấn tìm chuỗi con bằng Binary Search

> **📦 Formula Box — Độ phức tạp tìm kiếm chuỗi con bằng Suffix Array**
>
> $$T_{\text{tìm kiếm}}(m, n) = O(m \log n)$$
>
> | Thành phần | Ý nghĩa |
> |---|---|
> | $\log n$ | Số bước của Binary Search trên mảng $n$ hậu tố đã sắp xếp |
> | $m$ | Chi phí so sánh pattern (độ dài $m$) với một hậu tố tại mỗi bước Binary Search |
> | **Diễn giải kỹ thuật** | Sau khi đã đầu tư $O(n \log^2 n)$ (hoặc $O(n)$ với thuật toán nâng cao) để xây dựng Suffix Array **một lần duy nhất**, mỗi truy vấn tìm kiếm sau đó chỉ tốn $O(m \log n)$ — rất nhanh nếu cần thực hiện **nhiều** truy vấn trên cùng một văn bản cố định, đúng như động lực đã nêu ở Mục 3 |

---

## 8. Thuật toán

**8.1 — Xây dựng Suffix Array (phương pháp cơ bản, để dễ hiểu)**

```text
Đầu vào  — Chuỗi T độ dài n
Đầu ra   — Mảng SA chứa chỉ số các hậu tố theo thứ tự từ điển

Bước 1 — Với mỗi i từ 0 đến n-1:
                Tạo cặp (T[i:], i)   — hậu tố và chỉ số gốc của nó
        │
        ▼
Bước 2 — Sắp xếp toàn bộ n cặp theo thứ tự từ điển của hậu tố
        │
        ▼
Bước 3 — Trích xuất chỉ số gốc (thành phần thứ hai) từ mỗi cặp,
          theo đúng thứ tự đã sắp xếp
        │
        ▼
Bước 4 — Trả về mảng các chỉ số đó — chính là Suffix Array
```

**8.2 — Tìm kiếm chuỗi con bằng Binary Search trên Suffix Array**

```text
Đầu vào  — Text T, Suffix Array SA đã xây dựng, Pattern P
Đầu ra   — True nếu P xuất hiện trong T, False nếu không

Bước 1 — Đặt lo = 0, hi = n - 1  (n = độ dài T)
        │
        ▼
Bước 2 — Trong khi lo <= hi:
        │
        ▼
Bước 3 —   mid = (lo + hi) / 2
            suffix_giua = T[SA[mid]:]
        │
        ▼
Bước 4 —   Nếu P là tiền tố của suffix_giua:
                Trả về True (tìm thấy)
        │
        ▼
Bước 5 —   Ngược lại, nếu P < suffix_giua (theo thứ tự từ điển,
            chỉ so sánh trong phạm vi độ dài của P):
                hi = mid - 1
            Ngược lại:
                lo = mid + 1
        │
        ▼
Bước 6 — Nếu vòng lặp kết thúc mà chưa tìm thấy, trả về False
```

---

## 9. Triển khai

```python
def build_suffix_array(text: str) -> list[int]:
    """Xây dựng Suffix Array bằng phương pháp cơ bản (liệt kê + sắp xếp).

    Độ phức tạp: O(n^2 log n) — dùng cho mục đích giáo dục, minh họa
    rõ định nghĩa; văn bản rất dài nên dùng thuật toán nhân đôi (Mục 7.2)
    hoặc thư viện chuyên dụng.
    """
    n = len(text)
    # Tạo danh sách (hậu tố, chỉ số gốc), sau đó sắp xếp theo hậu tố
    suffixes = sorted(range(n), key=lambda i: text[i:])
    return suffixes


def suffix_array_search(text: str, suffix_array: list[int], pattern: str) -> bool:
    """Kiểm tra pattern có xuất hiện trong text hay không,
    dùng Binary Search trên Suffix Array đã xây dựng sẵn.

    Độ phức tạp: O(m log n), với m = len(pattern), n = len(text).
    """
    n = len(text)
    m = len(pattern)
    lo, hi = 0, n - 1

    while lo <= hi:
        mid = (lo + hi) // 2
        suffix_start = suffix_array[mid]
        current_suffix = text[suffix_start:suffix_start + m]

        if current_suffix == pattern:
            return True
        elif current_suffix < pattern:
            lo = mid + 1
        else:
            hi = mid - 1

    return False


def build_lcp_array(text: str, suffix_array: list[int]) -> list[int]:
    """Xây dựng LCP Array — độ dài tiền tố chung giữa các hậu tố
    liền kề trong Suffix Array đã sắp xếp.

    Cài đặt đơn giản (chưa tối ưu): O(n^2) trong trường hợp xấu nhất,
    dùng thuật toán Kasai's Algorithm để đạt O(n) (xem Mục 20).
    """
    n = len(text)
    lcp = [0] * (n - 1)

    for k in range(n - 1):
        suffix_a = text[suffix_array[k]:]
        suffix_b = text[suffix_array[k + 1]:]

        length = 0
        while (
            length < len(suffix_a)
            and length < len(suffix_b)
            and suffix_a[length] == suffix_b[length]
        ):
            length += 1

        lcp[k] = length

    return lcp
```

Hàm `build_suffix_array` dùng trực tiếp hàm `sorted()` có sẵn của Python với `key` là chính hậu tố — cách viết ngắn gọn nhưng ẩn chứa độ phức tạp $O(n^2 \log n)$ đã phân tích ở Mục 7.1, vì Python phải so sánh các chuỗi hậu tố (tốn tới $O(n)$ mỗi lần so sánh) trong quá trình sắp xếp $O(n \log n)$ lần. Hàm `suffix_array_search` triển khai đúng thuật toán 8.2. Hàm `build_lcp_array` minh họa khái niệm LCP Array bằng cách cài đặt trực tiếp theo định nghĩa (chưa tối ưu) — thuật toán Kasai's Algorithm để xây dựng LCP Array trong $O(n)$ được để lại như hướng mở rộng ở Mục 20.

---

## 10. Trực quan hóa quá trình thực thi

**10.1 — Xây dựng và LCP Array cho $T = \texttt{"banana"}$** (tiếp nối ví dụ ở Mục 5):

| Thứ tự $k$ | $SA[k]$ | Hậu tố $T[SA[k]:]$ | $LCP[k]$ (với hậu tố kế tiếp) |
|---:|---:|---|---:|
| 0 | 5 | `a` | 1 (chung `"a"` với hậu tố kế) |
| 1 | 3 | `ana` | 3 (chung `"ana"` với hậu tố kế) |
| 2 | 1 | `anana` | 0 (không chung ký tự đầu với `"banana"`) |
| 3 | 0 | `banana` | 0 |
| 4 | 4 | `na` | 2 (chung `"na"` với hậu tố kế) |
| 5 | 2 | `nana` | — (không có hậu tố kế tiếp) |

**10.2 — Vết thực thi Binary Search tìm chuỗi con $P = \texttt{"ana"}$** trong $SA = [5, 3, 1, 0, 4, 2]$ của `"banana"` ($n=6$):

| Bước | $lo$ | $hi$ | $mid$ | $SA[mid]$ | Hậu tố tại $mid$ (cắt theo độ dài $P$) | So với `"ana"` | Hành động |
|---|---:|---:|---:|---:|---|---|---|
| 1 | 0 | 5 | 2 | 1 | `"ana"` (từ `anana`) | **Bằng** | Trả về `True` ngay |

Chỉ cần **1 bước** Binary Search để tìm thấy `"ana"` — minh họa rõ ưu thế $O(m \log n)$ so với việc quét tuyến tính qua toàn bộ văn bản mỗi lần truy vấn.

**10.3 — So sánh thực nghiệm: nhiều truy vấn trên cùng một văn bản cố định**, giữa (a) chạy KMP riêng biệt cho mỗi truy vấn, và (b) xây dựng Suffix Array một lần rồi Binary Search cho mỗi truy vấn, trên văn bản dài $n=100.000$ ký tự với $1.000$ truy vấn pattern ngắn ($m \approx 10$):

| Phương pháp | Chi phí xây dựng (một lần) | Chi phí mỗi truy vấn | Tổng chi phí cho 1.000 truy vấn |
|---|---:|---:|---:|
| KMP lặp lại cho mỗi truy vấn | Không có | $O(n+m) \approx 100.010$ | $\approx 100.010.000$ |
| Suffix Array + Binary Search | $O(n \log^2 n) \approx 100.000 \times 17^2 \approx 28.900.000$ | $O(m \log n) \approx 10 \times 17 = 170$ | $\approx 28.900.000 + 170.000 \approx 29.070.000$ |

*(Số liệu là ước lượng minh họa dựa trên công thức Big-O để làm rõ xu hướng, không phải đo thực nghiệm chính xác tuyệt đối.)* Bảng trên cho thấy rõ: khi số lượng truy vấn đủ lớn trên cùng một văn bản cố định, chi phí đầu tư xây dựng Suffix Array một lần **được bù đắp** bởi chi phí truy vấn cực thấp sau đó — đúng như động lực đã nêu ở Mục 3.

---

## 11. Ứng dụng công nghiệp

> **🛠 Engineering Practice**
> Suffix Array đặc biệt hữu ích khi cùng một văn bản lớn cần chịu **rất nhiều truy vấn tìm kiếm chuỗi con** — chi phí xây dựng một lần được khấu hao (amortize) qua toàn bộ các truy vấn sau đó.

| Bối cảnh công nghiệp | Vai trò của Suffix Array |
|---|---|
| So khớp chuỗi DNA/protein (Bioinformatics) | Xây dựng Suffix Array cho toàn bộ bộ gen tham chiếu (reference genome) một lần, sau đó dùng để tìm nhanh vị trí của hàng triệu đoạn đọc (read) từ máy giải trình tự gen |
| Công cụ tìm kiếm toàn văn (Full-text Search) | Chỉ mục hóa (indexing) văn bản lớn để hỗ trợ tìm kiếm chuỗi con nhanh, đặc biệt trong các hệ thống không dùng chỉ mục dựa trên từ (word-based index) như Elasticsearch |
| Nén dữ liệu dựa trên biến đổi Burrows–Wheeler (BWT) | Suffix Array là thành phần trung tâm để xây dựng Burrows–Wheeler Transform, nền tảng của thuật toán nén `bzip2` |
| Phân tích văn bản lặp lại (Repeat Detection) | Kết hợp Suffix Array và LCP Array để tìm nhanh các đoạn chuỗi con lặp lại dài nhất trong một văn bản — ứng dụng trong phát hiện đạo văn và phân tích cấu trúc gen |

---

## 12. Góc nhìn nghiên cứu

> **🔬 Research Connection**
> Suffix Array, kết hợp với LCP Array, tạo thành một cặp cấu trúc dữ liệu có sức mạnh tương đương với Suffix Tree (Chương 5.7) cho phần lớn các bài toán thực tế, nhưng với footprint bộ nhớ nhỏ hơn đáng kể. Đây là một minh chứng quan trọng cho nguyên lý nghiên cứu: **hai cấu trúc dữ liệu có thể mạnh tương đương về mặt lý thuyết (asymptotic power) nhưng khác biệt lớn về hằng số ẩn (hidden constants) và mức sử dụng bộ nhớ thực tế** — sự khác biệt này thường quyết định cấu trúc nào được chọn dùng trong hệ thống công nghiệp thực tế.

Thuật toán DC3/Skew Algorithm (Karkkainen, Sanders, 2003) chứng minh Suffix Array có thể xây dựng trong $O(n)$ — kết quả lý thuyết quan trọng, dù trong thực hành nhiều hệ thống vẫn dùng các thuật toán $O(n \log n)$ đơn giản hơn để cài đặt, vì hằng số ẩn của DC3 khá lớn, khiến nó chỉ thực sự vượt trội khi $n$ cực kỳ lớn (ví dụ toàn bộ bộ gen người, hàng tỷ ký tự).

**Câu hỏi mở** để suy ngẫm trước khi bước sang Chương 5.7: Suffix Array biểu diễn thông tin về mọi hậu tố dưới dạng một **mảng phẳng đã sắp xếp**, trong khi Suffix Trie/Suffix Tree biểu diễn cùng thông tin đó dưới dạng **cấu trúc cây tường minh**. Liệu có những loại truy vấn mà cấu trúc cây làm được dễ dàng, nhưng Suffix Array — dù có thêm LCP Array — vẫn khó hoặc không thể làm hiệu quả bằng? (Gợi ý: hãy nghĩ về việc tìm chuỗi con chung dài nhất giữa hai văn bản khác nhau, hoặc đếm số lần một chuỗi con xuất hiện.)

---

## 13. Ưu điểm

- **Tiết kiệm bộ nhớ đáng kể** so với Suffix Trie/Suffix Tree — chỉ cần lưu một mảng số nguyên kích thước $n$, thay vì cấu trúc cây với overhead con trỏ tại mỗi nút.
- **Cấu trúc đơn giản, dễ tuần tự hóa (serialize)** để lưu trữ hoặc truyền qua mạng — một mảng số nguyên đơn giản hơn nhiều so với một cây với con trỏ.
- **Hiệu quả cho khối lượng truy vấn lớn trên cùng một văn bản cố định** — chi phí xây dựng một lần được khấu hao qua nhiều truy vấn.
- **Kết hợp với LCP Array** mở ra khả năng giải nhiều bài toán nâng cao (tìm chuỗi con lặp lại dài nhất, đếm số lần xuất hiện) hiệu quả.

---

## 14. Hạn chế

> **⚠️ Common Mistake**
> Một hiểu lầm phổ biến là nghĩ rằng Suffix Array luôn là lựa chọn "nhẹ và nhanh hơn" Suffix Tree trong mọi tình huống. Với các bài toán đòi hỏi **duyệt cây trực tiếp** (ví dụ tìm nhanh mọi vị trí xuất hiện của một pattern, không chỉ kiểm tra tồn tại), Suffix Tree có thể tự nhiên và hiệu quả hơn; Suffix Array cần thêm bước mở rộng phạm vi Binary Search sang hai phía sau khi tìm thấy một kết quả khớp để liệt kê đầy đủ mọi vị trí — không phức tạp nhưng cũng không "miễn phí".

- **Chi phí xây dựng ban đầu không hề rẻ** — phương pháp cơ bản $O(n^2 \log n)$ không khả thi cho văn bản rất dài; cần thuật toán nâng cao hơn ($O(n \log^2 n)$ hoặc $O(n)$), làm tăng độ phức tạp cài đặt đáng kể so với những gì trình bày trong chương này.
- **Không lưu trữ tường minh cấu trúc phân nhánh** như cây — một số bài toán (ví dụ tìm chuỗi con chung dài nhất giữa nhiều văn bản) cần thêm cấu trúc phụ trợ (LCP Array, RMQ — Range Minimum Query) để giải hiệu quả, làm tăng độ phức tạp tổng thể của giải pháp đầy đủ.
- **Không phù hợp nếu văn bản thay đổi thường xuyên** (dynamic text) — mọi thay đổi nhỏ trên văn bản gốc thường đòi hỏi xây dựng lại toàn bộ Suffix Array, không hỗ trợ cập nhật tăng dần (incremental update) một cách tự nhiên.

---

## 15. So sánh

**Bảng 5.6.1 — Suffix Trie, Suffix Array: hai cách biểu diễn cùng một loại thông tin**

| Tiêu chí | Suffix Trie/Tree (khái niệm, Chương 5.7) | Suffix Array (chương này) |
|---|---|---|
| Cách biểu diễn | Cấu trúc cây tường minh | Mảng số nguyên (chỉ số hậu tố đã sắp xếp) |
| Độ phức tạp không gian | Có thể $O(n^2)$ (Trie chưa nén) hoặc $O(n)$ (Tree đã nén, xem Chương 5.7) | $O(n)$ — luôn tuyến tính, với hằng số nhỏ (chỉ lưu số nguyên) |
| Tìm kiếm pattern độ dài $m$ | $O(m)$ — đi thẳng theo cây | $O(m \log n)$ — cần thêm Binary Search |
| Độ phức tạp xây dựng | $O(n)$ (với thuật toán chuyên biệt cho Suffix Tree, xem Chương 5.7) | $O(n \log^2 n)$ (doubling) hoặc $O(n)$ (DC3, phức tạp hơn nhiều để cài đặt) |
| Bộ nhớ thực tế (hằng số ẩn) | Lớn — nhiều con trỏ, overhead mỗi nút | Nhỏ — chỉ một mảng số nguyên phẳng |
| Dễ tuần tự hóa / lưu trữ | Khó hơn (cấu trúc cây với con trỏ) | Dễ (mảng phẳng) |

**Phân tích:** bảng trên cho thấy một đánh đổi kinh điển sẽ được phân tích đầy đủ hơn ở Chương 5.7: Suffix Tree tốt hơn về mặt **độ phức tạp thời gian tra cứu thuần túy** ($O(m)$ so với $O(m \log n)$), nhưng Suffix Array tốt hơn hẳn về mặt **bộ nhớ thực tế và độ đơn giản khi triển khai/lưu trữ**. Trong thực hành công nghiệp (đặc biệt Computational Biology, nơi văn bản có thể dài hàng tỷ ký tự), yếu tố bộ nhớ thực tế thường quan trọng hơn khác biệt logarit về mặt lý thuyết — đây là lý do Suffix Array, dù "yếu hơn" Suffix Tree về mặt Big-O thuần túy cho tra cứu, vẫn được ưa chuộng rộng rãi trong nhiều hệ thống thực tế.

---

## 16. Tóm tắt

- **Suffix Array** là một mảng chứa chỉ số bắt đầu của mọi hậu tố của một chuỗi, được sắp xếp theo thứ tự từ điển của chính các hậu tố đó.
- Một chuỗi con $P$ xuất hiện trong văn bản $T$ khi và chỉ khi $P$ là tiền tố của ít nhất một hậu tố của $T$ — và các hậu tố có chung tiền tố sẽ nằm **liền kề nhau** sau khi sắp xếp, cho phép dùng **Binary Search** để tìm kiếm hiệu quả.
- Xây dựng Suffix Array bằng phương pháp cơ bản (liệt kê + sắp xếp trực tiếp) tốn $O(n^2 \log n)$; kỹ thuật nhân đôi (doubling) cải thiện xuống $O(n \log^2 n)$; các thuật toán chuyên biệt hơn (DC3/Skew) đạt $O(n)$.
- Sau khi xây dựng, mỗi truy vấn tìm chuỗi con chỉ tốn $O(m \log n)$ — rất hiệu quả khi cần thực hiện nhiều truy vấn trên cùng một văn bản cố định.
- **LCP Array** ghi lại độ dài tiền tố chung giữa các hậu tố liền kề, là công cụ bổ trợ quan trọng để giải các bài toán nâng cao hơn (tìm chuỗi con lặp lại dài nhất, và nhiều bài toán khác nằm ngoài phạm vi chương này).
- Suffix Array đánh đổi tốc độ tra cứu thuần túy ($O(m \log n)$ thay vì $O(m)$) để đổi lấy footprint bộ nhớ nhỏ hơn đáng kể so với Suffix Tree.

Chương 5.7 sẽ giới thiệu **Suffix Tree** — cấu trúc cây tường minh và mạnh mẽ nhất của toàn bộ Part V, đạt tra cứu $O(m)$ thuần túy, và phân tích kỹ hơn đánh đổi giữa hai cấu trúc đã nêu ở Bảng 15.1.

---

## 17. Bài tập

### Mức Cơ bản (Basic)

1. Tự tay liệt kê và sắp xếp mọi hậu tố của chuỗi $T = \texttt{"abab"}$, từ đó viết ra Suffix Array tương ứng.
2. Với Suffix Array vừa xây dựng ở Bài tập 1, tính LCP Array tương ứng.
3. Giải thích bằng lời (không cần code) vì sao Suffix Array **không cần lưu trữ tường minh nội dung** của từng hậu tố.

### Mức Trung bình (Intermediate)

4. Chạy tay đầy đủ thuật toán Binary Search (Mục 8.2) để tìm pattern $P = \texttt{"ba"}$ trong Suffix Array của $T = \texttt{"banana"}$ đã xây dựng ở Mục 10.1. Trình bày từng bước theo định dạng Bảng 10.2.
5. Mở rộng hàm `suffix_array_search()` ở Mục 9 thành `count_occurrences()` — trả về **số lần** pattern xuất hiện trong text, bằng cách sau khi Binary Search tìm thấy một vị trí khớp, mở rộng sang cả hai phía (trái và phải) trong Suffix Array để đếm mọi hậu tố lân cận cũng có cùng tiền tố là pattern.

### Mức Nâng cao (Advanced)

6. Chứng minh chặt chẽ (không chỉ minh họa) khẳng định cốt lõi ở Mục 3: pattern $P$ xuất hiện tại vị trí $i$ trong $T$ khi và chỉ khi $P$ là tiền tố của $\text{suffix}_i$. Sử dụng chứng minh trực tiếp (Volume 01, Chương 1.4) cho cả hai chiều của phát biểu "khi và chỉ khi".
7. Dùng LCP Array đã xây dựng ở Bài tập 2 (hoặc từ hàm `build_lcp_array()` ở Mục 9), thiết kế một thuật toán tìm **chuỗi con lặp lại dài nhất** (Longest Repeated Substring) trong một văn bản. Gợi ý: giá trị lớn nhất trong LCP Array cho biết điều gì về hai hậu tố liền kề tương ứng?

### Mức Nghiên cứu (Research)

8. Tìm hiểu sơ lược về **thuật toán Kasai's Algorithm** — phương pháp xây dựng LCP Array trong thời gian tuyến tính $O(n)$, tận dụng chính Suffix Array đã có sẵn, thay vì so sánh trực tiếp từng cặp hậu tố liền kề như cài đặt đơn giản ở Mục 9 (vốn có độ phức tạp $O(n^2)$ trong trường hợp xấu nhất). Giải thích trực giác: insight nào cho phép Kasai's Algorithm tránh phải so sánh lại từ đầu mỗi cặp hậu tố — có điểm tương đồng nào với insight của thuật toán KMP ở Chương 5.2 hay không?

---

## 18. Dự án nhỏ

**Dự án: Công cụ tìm chuỗi con lặp lại trong văn bản (Repeated Substring Finder)**

**Mục tiêu:** vận dụng Suffix Array và LCP Array vào một bài toán phân tích văn bản thực tế, khác với bài toán tìm kiếm chuỗi con thuần túy.

**Yêu cầu:**

- Đọc một file văn bản `.txt` bất kỳ (ví dụ một đoạn văn dài, hoặc một chuỗi DNA giả lập).
- Xây dựng Suffix Array (dùng `build_suffix_array()` ở Mục 9) và LCP Array (dùng `build_lcp_array()`) cho văn bản đó.
- Dùng ý tưởng ở Bài tập 7 để tìm và in ra **chuỗi con lặp lại dài nhất** xuất hiện trong văn bản (xuất hiện ít nhất 2 lần, không tính chồng lấp).
- Mở rộng: tìm **top 5** chuỗi con lặp lại dài nhất (không trùng lặp lẫn nhau), thay vì chỉ một kết quả duy nhất.

**Công nghệ đề xuất:** Python thuần.

**Kết quả kỳ vọng:** một công cụ chạy được trên ít nhất 2 loại văn bản khác nhau (văn bản tự nhiên, và một chuỗi có cấu trúc lặp lại rõ ràng được tạo thủ công để kiểm chứng tính đúng đắn), cùng nhận xét về thời gian xây dựng Suffix Array so với độ dài văn bản đầu vào.

**Mở rộng (tùy chọn):** áp dụng công cụ này lên bộ dữ liệu DNA giả lập (chỉ gồm 4 ký tự A, T, G, C) để quan sát các đoạn lặp lại — liên hệ trực tiếp tới ứng dụng Computational Biology đã nêu ở Mục 11.

---

## 19. Tự đánh giá

- [ ] Tôi có thể tự tay liệt kê, sắp xếp mọi hậu tố, và viết ra Suffix Array cho một chuỗi ngắn (4–6 ký tự) mà không cần tham khảo tài liệu.
- [ ] Tôi hiểu rõ và có thể giải thích vì sao các hậu tố có chung tiền tố sẽ nằm liền kề nhau sau khi sắp xếp — đây là nền tảng cho việc dùng Binary Search.
- [ ] Tôi có thể cài đặt thuật toán tìm kiếm chuỗi con bằng Binary Search trên Suffix Array từ đầu, không cần tham khảo code mẫu.
- [ ] Tôi hiểu rõ ý nghĩa của LCP Array và có thể tự tay tính LCP Array cho một Suffix Array nhỏ đã cho.
- [ ] Tôi có thể giải thích rõ đánh đổi cốt lõi giữa Suffix Array và Suffix Trie/Tree (Mục 15) — không chỉ về mặt Big-O mà cả về bộ nhớ thực tế.

Nếu Bài tập 6 (chứng minh "khi và chỉ khi") vẫn còn khó khăn, đây là dấu hiệu nên ôn lại kỹ thuật chứng minh hai chiều ở Volume 01, Chương 1.4 trước khi tiếp tục sang Chương 5.7.

---

## 20. Đọc thêm

- **Bài báo gốc:** Udi Manber, Gene Myers, *"Suffix arrays: a new method for on-line string searches"*, SIAM Journal on Computing, 1993 (công bố đầy đủ, sau bản sơ khởi 1990). *(Xem PAPERS.md.)*
- **Bài báo:** Juha Karkkainen, Peter Sanders, *"Simple Linear Work Suffix Array Construction"* (thuật toán DC3/Skew Algorithm), 2003.
- **Sách:** Dan Gusfield, *Algorithms on Strings, Trees, and Sequences* — tài liệu tham khảo chuyên sâu và kinh điển nhất cho toàn bộ String Algorithms, bao gồm Suffix Array, Suffix Tree, và LCP Array.
- **Chủ đề mở rộng (không bắt buộc):** tìm đọc về thuật toán **Kasai's Algorithm** (xây dựng LCP Array trong $O(n)$) và ứng dụng Suffix Array trong **Burrows–Wheeler Transform**.
- **Chương tiếp theo:** Chương 5.7 — Suffix Tree.

---

### Liên kết chương (Cross References)

- **Chương trước:** 5.5 — Trie (Suffix Trie là cầu nối khái niệm trực tiếp dẫn tới Suffix Array, xem Mục 1–2 và Bài tập 8 của Chương 5.5).
- **Chương tiếp theo:** 5.7 — Suffix Tree (cấu trúc cây tường minh, đánh đổi bộ nhớ lớn hơn để đạt tra cứu $O(m)$ thuần túy, phân tích đầy đủ đánh đổi đã nêu ở Bảng 15.1).
- **Chương liên quan xa hơn:** Volume 03, Part I — Sorting và Divide and Conquer (nền tảng cho thuật toán nhân đôi ở Mục 7.2); Volume 03, Part III — Binary Search (nền tảng trực tiếp cho thuật toán tìm kiếm ở Mục 8.2); Volume 04 — Information Retrieval (ứng dụng chỉ mục hóa văn bản lớn).
- **Vị trí trong Knowledge Graph:** Nút thứ hai của nhóm "cấu trúc dữ liệu cho hậu tố của một văn bản" trong Part V, là bước đệm trực tiếp trước Chương 5.7 — chương cuối cùng và phức tạp nhất của Part V.

---

*Hết Chương 5.6. Chương này tuân thủ đầy đủ cấu trúc 20 mục của `OUTPUT.md` và chuẩn Presentation Layer của `WRITING_STANDARD.md`. Chương trình bày đầy đủ định nghĩa, thuật toán xây dựng cơ bản, và ý tưởng trực giác của kỹ thuật nhân đôi, đồng thời chỉ rõ ranh giới giữa nội dung cốt lõi (Suffix Array, LCP Array, Binary Search) và nội dung mở rộng tự học (DC3/Skew Algorithm, Kasai's Algorithm) — phù hợp với đối tượng độc giả của Handbook (READER_PERSONAL.md). Đang chờ rà soát trước khi tiếp tục sang Chương 5.7 — Suffix Tree, chương cuối cùng của Part V.*
