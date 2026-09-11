# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Tôi nghĩ với các temperature 0.0 thì khá chặt chẽ không sáng tạo. với 0.5 thì khá chung chung còn 1.0 và 1.5 thì câu trả lời khá sáng tạo

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt chatbot 0.3 tại vì 0.3 tôi thường dùng với tác vụ cho sản phẩm cần nhất quán và ít bịa

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Theo bảng giá thì 4o đắt gấp 16.7 lần , nếu làm 1 tác vụ như thiết kế system khó thì nên dùng 4o thông minh hơn còn nếu làm các tác vụ đơn giản như lọc dữ liệu các thứ đơn giản nhưng request lớn thì nên dùng mini

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Cái thằng chuyên gia tài chính nó sẽ giải thích chi tiết và đúng đắn hơn, còn cái thằng giáo viên tiểu học thì giải thích lan man. 

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Đoạn văn tôi thử có 117 từ. tiktoken đếm được 139 token, trong khi cách ước lượng `số từ / 0.75` cho ra 156 token, chênh lệch khoảng 10.9%. Công thức số từ / 0.75 chỉ là ước lượng, còn số token thật phụ thuộc vào tokenizer; tiếng Việt có dấu và cách tách từ/sub-token khác tiếng Anh nên số token có thể cao hơn hoặc khác đáng kể.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming phù hợp nhất với chatbot hoặc các tác vụ sinh câu trả lời dài vì người dùng có thể thấy nội dung xuất hiện dần thay vì phải chờ model sinh xong toàn bộ response. Điều này làm cảm giác phản hồi nhanh hơn dù tổng thời gian có thể không giảm. Non-streaming phù hợp hơn khi response ngắn hoặc khi chương trình cần nhận toàn bộ kết quả trước rồi mới xử lý, ví dụ parse JSON.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp tăng dần thời gian chờ sau mỗi lần request thất bại, nhờ đó giảm áp lực lên API khi server đang quá tải. Nếu hàng nghìn client đều retry với delay cố định 1 giây thì chúng có thể cùng gửi request trở lại một lúc, gây hiện tượng thundering herd và làm server tiếp tục quá tải

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
>“Bạn là trợ giảng AI, giải thích ngắn gọn, chính xác bằng tiếng Việt, ưu tiên trực giác trước rồi mới đi vào thuật ngữ kỹ thuật.” Tôi dùng từ “ngắn gọn” để tránh câu trả lời quá dài và tốn token không cần thiết. Tôi yêu cầu “trực giác trước thuật ngữ kỹ thuật” để người học hiểu bản chất trước khi đi sâu vào chi tiết.


### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Tôi nghĩ hạn chế lớn nhất là cái history quá ngắn. Đề xuất nếu context dài thì nên tóm tắt lại hoặc lưu vào memory và retrieval lại khi cần

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
