# Ngày 1 — Bài Tập & Phản Ánh
## Nền Tảng LLM API | Phiếu Thực Hành

**Thời lượng:** 1:30 giờ  
**Cấu trúc:** Lập trình cốt lõi (60 phút) → Bài tập mở rộng (30 phút)

---

## Phần 1 — Lập Trình Cốt Lõi (0:00–1:00)

Chạy các ví dụ trong Google Colab tại: https://colab.research.google.com/drive/172zCiXpLr1FEXMRCAbmZoqTrKiSkUERm?usp=sharing

Triển khai tất cả TODO trong `template.py`. Chạy `pytest tests/` để kiểm tra tiến độ.

**Điểm kiểm tra:** Sau khi hoàn thành 4 nhiệm vụ, chạy:
```bash
python template.py
```
Bạn sẽ thấy output so sánh phản hồi của GPT-4o và GPT-4o-mini.

---

## Phần 2 — Bài Tập Mở Rộng (1:00–1:30)

### Bài tập 2.1 — Độ Nhạy Của Temperature
Gọi `call_openai` với các giá trị temperature 0.0, 0.5, 1.0 và 1.5 sử dụng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature = 0.0, phản hồi gần như giống nhau mỗi lần gọi, rất ổn định và tập trung vào sự thật phổ biến. Khi tăng lên 0.5 và 1.0, câu trả lời đa dạng hơn với nhiều chủ đề khác nhau nhưng vẫn mạch lạc. Ở mức 1.5, phản hồi trở nên sáng tạo hơn nhưng đôi khi kém chính xác hoặc lan man, cho thấy temperature cao làm tăng tính ngẫu nhiên trong việc chọn token.

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature khoảng 0.2-0.3 cho chatbot hỗ trợ khách hàng. Lý do là chatbot cần trả lời chính xác, nhất quán và đáng tin cậy — khách hàng cần câu trả lời đúng chứ không cần sáng tạo. Giá trị thấp nhưng không bằng 0 giúp tránh phản hồi quá cứng nhắc trong khi vẫn đảm bảo độ chính xác cao.

---

### Bài tập 2.2 — Đánh Đổi Chi Phí
Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**
> Tổng token mỗi ngày: 10.000 x 3 x 350 = 10.500.000 token (10.5M token). Chi phí GPT-4o output: 10.5M x $20.00/1M = $210/ngày. Chi phí GPT-4o-mini output: 10.5M x $0.60/1M = $6.30/ngày. GPT-4o đắt hơn khoảng 33 lần so với GPT-4o-mini ($20.00 / $0.60 = 33.3x cho output tokens). Trên quy mô lớn, sự chênh lệch này rất đáng kể — khoảng $6.300/tháng vs $189/tháng.

**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**
> GPT-4o xứng đáng khi cần suy luận phức tạp, ví dụ hệ thống tư vấn pháp luật hoặc phân tích y khoa, nơi độ chính xác ảnh hưởng trực tiếp đến quyết định quan trọng và sai sót có thể gây hậu quả nghiêm trọng. GPT-4o-mini phù hợp hơn cho các tác vụ đơn giản như chatbot FAQ, phân loại tin nhắn, hoặc tóm tắt nội dung ngắn — những việc không đòi hỏi suy luận sâu mà cần xử lý nhanh với chi phí thấp.

---

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất trong các ứng dụng chatbot tương tác trực tiếp với người dùng, nơi Time-To-First-Token (TTFT) quyết định trải nghiệm. Khi người dùng chờ phản hồi dài (500+ token), streaming cho phép họ bắt đầu đọc ngay trong vài trăm milliseconds thay vì chờ 5-15 giây — tạo cảm giác tự nhiên như đang trò chuyện với người thật. Ngược lại, non-streaming phù hợp hơn khi cần xử lý output hoàn chỉnh trước khi hiển thị, ví dụ sinh JSON để parse, tạo code cần validate, hoặc gọi API trong pipeline backend nơi không có người dùng trực tiếp chờ đợi. Non-streaming cũng đơn giản hơn về mặt kỹ thuật vì không cần xử lý từng chunk và dễ kiểm soát lỗi hơn.


## Danh Sách Kiểm Tra Nộp Bài
- [ ] Tất cả tests pass: `pytest tests/ -v`
- [ ] `call_openai` đã triển khai và kiểm thử
- [ ] `call_openai_mini` đã triển khai và kiểm thử
- [ ] `compare_models` đã triển khai và kiểm thử
- [ ] `streaming_chatbot` đã triển khai và kiểm thử
- [ ] `retry_with_backoff` đã triển khai và kiểm thử
- [ ] `batch_compare` đã triển khai và kiểm thử
- [ ] `format_comparison_table` đã triển khai và kiểm thử
- [ ] `exercises.md` đã điền đầy đủ
- [ ] Sao chép bài làm vào folder `solution` và đặt tên theo quy định 
