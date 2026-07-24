# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng placeholder bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature thấp như 0.0, phản hồi ổn định hơn, ít thay đổi và thường đi thẳng vào ý chính. Khi tăng lên 1.0 hoặc 1.5, câu trả lời đa dạng và sáng tạo hơn, nhưng cũng dễ dài dòng hoặc kém nhất quán hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt khoảng 0.2 đến 0.4 vì chatbot hỗ trợ khách hàng cần trả lời ổn định, rõ ràng và ít bịa hơn. Mức này vẫn cho phép câu trả lời tự nhiên nhưng không quá ngẫu nhiên.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Theo bảng giá output trong template, GPT-4o là 0.010 USD/1K token còn GPT-4o-mini là 0.0006 USD/1K token, nên GPT-4o đắt hơn khoảng 16.7 lần. GPT-4o xứng đáng khi cần lập luận khó hoặc câu trả lời chất lượng cao; mini phù hợp cho hỏi đáp đơn giản, phân loại, tóm tắt ngắn hoặc chatbot chi phí thấp.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với vai giáo viên tiểu học, phản hồi thường ngắn hơn, dùng từ đơn giản và ví dụ gần gũi như sổ ghi chép hoặc trò chơi. Với vai chuyên gia tài chính, phản hồi dài hơn, dùng nhiều thuật ngữ như phi tập trung, ledger, consensus và tài sản số. System prompt ảnh hưởng trực tiếp đến giọng văn, mức độ chi tiết, cách chọn ví dụ và đối tượng mà model đang giải thích cho.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Với đoạn tiếng Việt khoảng 100 từ, số token thực tế thường cao hơn ước lượng đơn giản và có thể chênh khoảng 20-40% tùy câu chữ. Tiếng Việt thường tốn nhiều token vì có dấu, nhiều từ ghép được viết tách bằng khoảng trắng và bộ mã hóa có thể chia một từ tiếng Việt thành nhiều mảnh nhỏ.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi câu trả lời dài hoặc người dùng cần cảm giác phản hồi ngay, ví dụ chatbot, trợ lý lập trình hoặc giải thích từng bước. Non-streaming phù hợp hơn khi phản hồi ngắn, cần xử lý toàn bộ kết quả trước khi hiển thị, hoặc khi hệ thống cần kiểm tra/format output hoàn chỉnh trước khi gửi cho người dùng.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm áp lực lên API vì mỗi lần lỗi sẽ chờ lâu hơn trước khi thử lại. Nếu hàng nghìn client đều retry sau đúng 1 giây, chúng có thể cùng lúc gửi lại request và làm server tiếp tục quá tải. Backoff giúp phân tán các lần thử lại và tăng cơ hội thành công.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona tôi chọn là trợ giảng AI thân thiện. System prompt: "Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn, dễ hiểu bằng tiếng Việt và ưu tiên ví dụ thực tế." Tôi chọn "ngắn gọn, dễ hiểu" để câu trả lời không lan man, và "bằng tiếng Việt" để phù hợp với người học trong lớp.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là trợ lý chỉ giữ 3 lượt hội thoại gần nhất nên dễ quên ngữ cảnh dài. Một cải thiện là lưu tóm tắt cuộc trò chuyện cũ vào một biến `summary`, rồi gửi `summary` cùng history ngắn trong mỗi request. Cách này giữ được bối cảnh quan trọng mà không làm token tăng quá nhanh.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
