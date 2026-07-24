# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 14h00–18h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.7, 1.2 và 1.8 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Hà Nội."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi? Ở mức nào phản hồi bắt đầu
kém mạch lạc?** (2–3 câu)
> *Temperature càng thấp thì phản hồi càng ổn định, nhất quán và ít sáng tạo; càng cao thì nội dung đa dạng và sáng tạo hơn. Ở mức khoảng 1.2 phản hồi bắt đầu có xu hướng thêm nhiều chi tiết sáng tạo, còn ở 1.8 đôi khi trở nên lan man hoặc kém mạch lạc.*

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho trợ lý soạn thảo hợp đồng pháp lý,
và bao nhiêu cho trợ lý viết slogan quảng cáo? Giải thích khác biệt.**
> *Với trợ lý soạn thảo hợp đồng pháp lý, đặt temperature = 0.0–0.2 sẽ đảm bảo câu trả lời nhất quán và chính xác. Với trợ lý viết slogan quảng cáo, đặt temperature = 1.0–1.3 để tăng tính sáng tạo và tạo ra nhiều ý tưởng đa dạng.*

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 20.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 2 lần,
mỗi lần trung bình ~500 token đầu ra.

**Ước tính chi phí mỗi ngày của model lớn so với model nhỏ cho workload này
(dựa trên bảng giá trong template). Nêu một trường hợp model lớn xứng đáng
với chi phí và một trường hợp model nhỏ là lựa chọn đúng:**
> *Có 20.000 người dùng × 2 lần gọi × 500 output token = 20 triệu output token/ngày. Dựa trên bảng giá trong template, chi phí model lớn sẽ cao hơn đáng kể so với model nhỏ (200 USD so với 120 USD). Model lớn phù hợp cho các tác vụ đòi hỏi suy luận phức tạp như phân tích tài liệu hoặc lập trình; model nhỏ phù hợp cho chatbot FAQ, phân loại văn bản hoặc tóm tắt đơn giản để tiết kiệm chi phí.*

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích máy học (machine learning) là gì?"** nhưng hai system prompt
khác nhau:
- "Bạn là một nhà thơ, trả lời mọi thứ bằng hình ảnh ví von, tránh thuật ngữ."
- "Bạn là kỹ sư phần mềm senior, trả lời chính xác, có ví dụ code khi phù hợp."

**Hai phản hồi khác nhau như thế nào (giọng văn, độ dài, mức kỹ thuật)?
Từ đó rút ra system prompt điều khiển được những khía cạnh nào của phản hồi?**
(3–4 câu)
> *Với system prompt "nhà thơ", phản hồi giàu hình ảnh, giàu cảm xúc và hầu như không dùng thuật ngữ kỹ thuật. Với system prompt "kỹ sư phần mềm senior", câu trả lời chính xác hơn, nhiều khái niệm chuyên môn và có thể kèm ví dụ code. Điều này cho thấy system prompt có thể điều khiển giọng văn, phong cách trình bày, mức độ kỹ thuật, độ dài và cách tổ chức câu trả lời.*

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~150 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Nếu dùng ước lượng thô để dự
toán ngân sách API cho ứng dụng tiếng Việt, bạn sẽ dự toán thiếu hay thừa —
và vì sao?**
> *Kết quả tiktoken và ước lượng theo công thức "số từ / 0.75" chênh khoảng 10–20% (tùy đoạn văn). Nếu chỉ dùng ước lượng thô để tính ngân sách API cho tiếng Việt thì có thể dự toán thiếu hoặc thừa vì token không tỷ lệ trực tiếp với số từ; dấu câu, ký tự Unicode và cách tokenizer tách từ đều ảnh hưởng đến số token.*

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Xét ba ứng dụng: (a) chatbot văn bản, (b) trợ lý giọng nói đọc to phản hồi,
(c) pipeline dịch tài liệu chạy ngầm ban đêm. Ứng dụng nào hưởng lợi nhiều
nhất từ streaming, ứng dụng nào không cần — và tại sao?** (1 đoạn văn)
> *Streaming mang lại lợi ích lớn nhất cho chatbot văn bản và đặc biệt là trợ lý giọng nói vì người dùng nhận được phản hồi gần như ngay lập tức, giảm cảm giác chờ đợi. Với chatbot, từng phần câu trả lời xuất hiện liên tục giúp trải nghiệm tự nhiên hơn; với trợ lý giọng nói, hệ thống có thể bắt đầu đọc ngay khi nhận được những token đầu tiên. Ngược lại, pipeline dịch tài liệu chạy ngầm ban đêm hầu như không cần streaming vì người dùng chỉ quan tâm đến kết quả cuối cùng.*

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**Khi API quá tải và hàng nghìn client cùng retry, exponential backoff giúp
gì so với delay cố định? Tra cứu thêm: kỹ thuật "jitter" (thêm độ trễ ngẫu
nhiên) giải quyết vấn đề gì còn sót lại?**
> *Exponential backoff giúp các client không retry đồng thời khi API quá tải, từ đó giảm áp lực lên máy chủ và tăng khả năng phục hồi của dịch vụ. Nếu tất cả client đều retry sau một khoảng thời gian cố định thì sẽ tạo ra các đợt truy cập dồn dập. Jitter bổ sung một khoảng thời gian ngẫu nhiên vào mỗi lần retry để tránh hiện tượng nhiều client vẫn gửi yêu cầu cùng lúc sau mỗi chu kỳ backoff.*

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Viết lại system prompt bạn dùng cho trợ lý của mình. Chỉ ra 2 chỗ trong
prompt mà nếu xóa đi, hành vi trợ lý sẽ thay đổi rõ rệt — và mô tả thay đổi
đó:**
> *System prompt: "Bạn là trợ lý AI thân thiện, trả lời ngắn gọn, chính xác, giải thích từng bước khi người dùng hỏi kiến thức kỹ thuật và luôn đưa ví dụ minh họa nếu phù hợp."

Nếu xóa phần "trả lời ngắn gọn", trợ lý sẽ có xu hướng trả lời dài hơn và nhiều chi tiết hơn. Nếu xóa phần "luôn đưa ví dụ minh họa nếu phù hợp", câu trả lời sẽ thiên về lý thuyết và khó hiểu hơn đối với người mới.*

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn giữ history 4 lượt cuối. Hãy mô tả một tình huống hội thoại
cụ thể mà giới hạn này khiến trợ lý trả lời sai/mất ngữ cảnh, và đề xuất một
cách khắc phục (ví dụ: tóm tắt các lượt cũ, tăng giới hạn có chọn lọc...):**
> *Giả sử người dùng mô tả yêu cầu của một dự án trong nhiều lượt hội thoại, sau đó tiếp tục hỏi về một chi tiết đã được nhắc ở lượt thứ năm trở về trước. Vì chỉ lưu 4 lượt gần nhất nên trợ lý sẽ quên thông tin quan trọng và có thể trả lời sai hoặc yêu cầu người dùng nhắc lại. Một cách khắc phục là định kỳ tóm tắt các lượt hội thoại cũ thành một bản ghi ngắn rồi đưa bản tóm tắt đó vào context, hoặc chỉ giữ lại những thông tin quan trọng thay vì lưu toàn bộ lịch sử.*

---

## Danh Sách Kiểm Tra Nộp Bài

- [x] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [x] Cả 4 checkpoint pytest đều pass
- [x] Tất cả 9 câu trong file này đã được trả lời
- [x] Đã copy bài làm vào folder `solution/`, push lên GitHub cá nhân và nộp link repo vào vlearn (theo hướng dẫn README)
