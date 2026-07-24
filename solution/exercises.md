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
Khi temperature tăng từ 0.0 lên 1.8, phản hồi có xu hướng đa dạng và sáng tạo hơn nhưng đồng thời ít ổn định hơn. Ở temperature 0.0, câu trả lời thường ngắn gọn, rõ ràng và gần như giống nhau giữa các lần chạy; mức 0.7 vẫn mạch lạc nhưng tự nhiên hơn. Từ khoảng 1.2 trở lên, nội dung bắt đầu có nhiều cách diễn đạt bất ngờ, còn ở 1.8 phản hồi có thể lan man, phóng đại hoặc kém mạch lạc.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho trợ lý soạn thảo hợp đồng pháp lý,
và bao nhiêu cho trợ lý viết slogan quảng cáo? Giải thích khác biệt.**
Tôi sẽ đặt temperature khoảng 0.0–0.2 cho trợ lý soạn thảo hợp đồng pháp lý vì loại công việc này cần tính chính xác, nhất quán và hạn chế việc mô hình tự sáng tạo thêm thông tin. Với trợ lý viết slogan quảng cáo, tôi sẽ chọn temperature khoảng 0.8–1.2 để tạo ra nhiều ý tưởng mới, đa dạng và giàu tính sáng tạo hơn. Sự khác biệt đến từ việc văn bản pháp lý ưu tiên độ tin cậy, còn slogan quảng cáo ưu tiên sự độc đáo và khả năng thu hút.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 20.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 2 lần,
mỗi lần trung bình ~500 token đầu ra.

**Ước tính chi phí mỗi ngày của model lớn so với model nhỏ cho workload này
(dựa trên bảng giá trong template). Nêu một trường hợp model lớn xứng đáng
với chi phí và một trường hợp model nhỏ là lựa chọn đúng:**
Mỗi ngày có:

20.000 người dùng × 2 lượt gọi = 40.000 lượt gọi
40.000 lượt × 500 token đầu ra = 20.000.000 token đầu ra/ngày
Tương đương 20.000 đơn vị 1.000 token

Giả sử bảng giá trong template là:

GPT-4o: 0,015 USD/1.000 output token
GPT-4o-mini: 0,0006 USD/1.000 output token

Chi phí đầu ra ước tính:

GPT-4o: 20.000 × 0,015 = 300 USD/ngày
GPT-4o-mini: 20.000 × 0,0006 = 12 USD/ngày

Như vậy, model lớn đắt hơn khoảng 288 USD mỗi ngày, tương đương khoảng 25 lần model nhỏ. Đây mới chỉ là chi phí output, chưa bao gồm token input.

Model lớn xứng đáng với chi phí trong các tình huống phức tạp và có ảnh hưởng lớn, chẳng hạn phân tích hợp đồng, giải quyết yêu cầu chuyên môn khó hoặc tạo câu trả lời cần khả năng suy luận và chất lượng cao. Model nhỏ phù hợp với các tác vụ thường xuyên và đơn giản như phân loại câu hỏi, tóm tắt ngắn, trả lời FAQ hoặc chatbot chăm sóc khách hàng cơ bản vì có tốc độ nhanh và chi phí thấp hơn đáng kể.

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
Hai phản hồi có sự khác biệt rõ rệt về phong cách. Với system prompt "nhà thơ", mô hình sử dụng giọng văn giàu hình ảnh, ví von bằng câu chuyện dạy một đứa trẻ nhận biết các loài chim và tránh các thuật ngữ kỹ thuật. Trong khi đó, với system prompt "kỹ sư phần mềm senior", câu trả lời dài hơn, sử dụng nhiều thuật ngữ chuyên môn như Supervised Learning, Classification, Regression và còn đưa ra ví dụ mã Python bằng scikit-learn. Qua đó có thể thấy system prompt có thể điều khiển phong cách diễn đạt, mức độ chuyên môn, độ chi tiết, độ dài và vai trò mà mô hình sẽ thể hiện trong câu trả lời.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~150 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Nếu dùng ước lượng thô để dự
toán ngân sách API cho ứng dụng tiếng Việt, bạn sẽ dự toán thiếu hay thừa —
và vì sao?**
Với đoạn văn đã chọn, hàm count_tokens sử dụng tiktoken đếm được 187 token, trong khi cách ước lượng của Part 1 (số từ / 0.75) cho kết quả 208 token. Hai kết quả chênh lệch khoảng 11,23%.

Trong trường hợp này, cách ước lượng thô dự toán thừa chi phí API vì số token ước lượng lớn hơn số token thực tế. Nguyên nhân là token không tương đương hoàn toàn với số từ; OpenAI sử dụng bộ mã hóa (tiktoken) để chia văn bản thành các token dựa trên từng chuỗi ký tự, dấu câu và cách mã hóa của mô hình. Do đó, muốn dự toán chi phí chính xác nên sử dụng tiktoken thay vì chỉ ước lượng từ số từ.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Xét ba ứng dụng: (a) chatbot văn bản, (b) trợ lý giọng nói đọc to phản hồi,
(c) pipeline dịch tài liệu chạy ngầm ban đêm. Ứng dụng nào hưởng lợi nhiều
nhất từ streaming, ứng dụng nào không cần — và tại sao?** (1 đoạn văn)
Trong ba ứng dụng, chatbot văn bản và trợ lý giọng nói hưởng lợi nhiều nhất từ streaming vì người dùng có thể nhìn thấy hoặc nghe phản hồi ngay khi mô hình bắt đầu sinh nội dung, giúp giảm cảm giác chờ đợi và cải thiện trải nghiệm sử dụng. Đặc biệt với trợ lý giọng nói, hệ thống có thể đọc từng phần câu trả lời ngay khi nhận được thay vì phải đợi toàn bộ phản hồi hoàn thành. Ngược lại, pipeline dịch tài liệu chạy ngầm ban đêm hầu như không cần streaming vì không có người dùng tương tác trực tiếp; điều quan trọng hơn là độ chính xác, khả năng xử lý hàng loạt và hoàn thành toàn bộ tài liệu.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**Khi API quá tải và hàng nghìn client cùng retry, exponential backoff giúp
gì so với delay cố định? Tra cứu thêm: kỹ thuật "jitter" (thêm độ trễ ngẫu
nhiên) giải quyết vấn đề gì còn sót lại?**
Exponential backoff giúp giảm tải cho máy chủ khi API bị quá tải bằng cách tăng dần thời gian chờ sau mỗi lần thử lại, thay vì tất cả client cùng gửi yêu cầu liên tục với khoảng thời gian cố định. Cách này làm giảm số lượng request đồng thời, giúp máy chủ có thời gian phục hồi và tăng khả năng các lần retry sau sẽ thành công. Tuy nhiên, nếu nhiều client cùng áp dụng một lịch backoff giống hệt nhau, chúng vẫn có thể retry cùng lúc. Kỹ thuật jitter khắc phục vấn đề này bằng cách thêm một khoảng thời gian ngẫu nhiên vào mỗi lần chờ, giúp phân tán các request theo thời gian và tránh hiện tượng nhiều client cùng gửi lại yêu cầu tại một thời điểm.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Viết lại system prompt bạn dùng cho trợ lý của mình. Chỉ ra 2 chỗ trong
prompt mà nếu xóa đi, hành vi trợ lý sẽ thay đổi rõ rệt — và mô tả thay đổi
đó:**
System prompt:

Bạn là một trợ lý AI thân thiện và kiên nhẫn. Hãy giải thích các khái niệm một cách rõ ràng, từng bước, sử dụng ví dụ minh họa khi cần. Nếu không chắc chắn về câu trả lời, hãy nói rõ mức độ không chắc chắn thay vì bịa thông tin.

Hai phần quan trọng trong prompt:

"Giải thích các khái niệm một cách rõ ràng, từng bước, sử dụng ví dụ minh họa."
Nếu xóa phần này, trợ lý có thể trả lời ngắn gọn hơn, ít giải thích chi tiết và ít đưa ra ví dụ, khiến người mới học khó hiểu.
"Nếu không chắc chắn về câu trả lời, hãy nói rõ mức độ không chắc chắn thay vì bịa thông tin."
Nếu xóa phần này, trợ lý có thể tự tin đưa ra những thông tin chưa chính xác hoặc suy đoán, làm tăng nguy cơ trả lời sai.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn giữ history 4 lượt cuối. Hãy mô tả một tình huống hội thoại
cụ thể mà giới hạn này khiến trợ lý trả lời sai/mất ngữ cảnh, và đề xuất một
cách khắc phục (ví dụ: tóm tắt các lượt cũ, tăng giới hạn có chọn lọc...):**
Giả sử người dùng đang lập kế hoạch du lịch. Ở bốn lượt đầu, họ cho biết sẽ đi Đà Nẵng, ngân sách 8 triệu đồng, thích hải sản và không muốn đi bằng máy bay. Sau nhiều lượt trao đổi tiếp theo về khách sạn và địa điểm tham quan, các thông tin ban đầu bị loại khỏi history vì chỉ lưu 4 lượt gần nhất. Khi người dùng hỏi: "Hãy gợi ý lịch trình phù hợp với yêu cầu ban đầu.", trợ lý có thể quên ngân sách hoặc phương tiện di chuyển và đưa ra lịch trình không còn phù hợp.

Một cách khắc phục là tóm tắt các thông tin quan trọng của những lượt cũ (ví dụ: điểm đến, ngân sách, sở thích và các ràng buộc) rồi lưu bản tóm tắt này vào history thay vì giữ toàn bộ hội thoại. Ngoài ra, có thể tăng giới hạn history hoặc chỉ giữ lại các lượt chứa thông tin quan trọng thay vì chỉ dựa vào số lượng lượt.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên GitHub cá nhân và nộp link repo vào vlearn (theo hướng dẫn README)
