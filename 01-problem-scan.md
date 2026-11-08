# 01 — PROBLEM SCAN | VinSmart AI HomeCare (15.000 người dùng giả định)

> **Nhóm:** [ĐIỀN TÊN NHÓM] | **Họ tên và email thành viên:** [ĐIỀN ĐẦY ĐỦ] | **VinUni Lab 02 – AI Product Scoping**
> **Tình trạng dữ liệu:** Case study giả định. **15.000 là số người dùng giả định**, KHÔNG phải người dùng hoạt động được VinSmart xác nhận. 10.000+ lượt tải ứng dụng KHÔNG đồng nghĩa 10.000+ người dùng. Mọi metric và chi phí dưới đây là giả định thử nghiệm, cần đo lại.

## Phase 1 — SCAN: 4 lenses, ít nhất 5 cơ hội

| # | Công ty / bối cảnh | Lens | Cơ hội / bottleneck **cần xác minh** |
|---|---|---|---|
| 1 | VinSmart – Smart Home (giả định) | Repetitive; Stakeholder pain | Khách hỏi lặp lại về lỗi Wi-Fi / thiết bị offline, phải chờ nhân viên dù có hướng dẫn chuẩn. **CHỌN** |
| 2 | VinSmart – Smart Home (giả định) | Time-consuming | CSKH hỏi lại model, phiên bản app, mã lỗi trước khi hướng dẫn. |
| 3 | VinSmart – Smart Home (giả định) | AI-upgrade | Khó tìm đúng mục FAQ/manual tương ứng câu hỏi ngôn ngữ tự nhiên. |
| 4 | Vinhomes | Repetitive | Phân loại phản ánh cư dân và chuyển đúng nhóm xử lý. |
| 5 | VinFast | Time-consuming | Tóm tắt mô tả triệu chứng của khách trước khi tiếp nhận bảo dưỡng. |
| 6 | Vinpearl | AI-upgrade | Tra cứu chính sách và trả lời câu hỏi đặt dịch vụ có nhiều cách diễn đạt. |

Các mục trên là **giả thuyết discovery**, không phải kết luận các đơn vị chưa có công cụ tự động hóa.

## Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

### Quick Problem Card #1 — Tự phục vụ lỗi kết nối Smart Home (được chọn)
- **Bài toán (1 câu):** Người dùng cần hướng dẫn xử lý lỗi kết nối đơn giản nhưng trong kịch bản hiện tại vẫn phải mở ticket để nhân viên tra FAQ và phản hồi.
- **Công ty:** VinSmart (mục Khác của worksheet; cần giảng viên xác nhận phạm vi).
- **Actor:** Người dùng Smart Home; nhân viên CSKH; nhân viên kỹ thuật tuyến 2.
- **Workflow hiện tại giả định:** (1) Khách mở ticket → (2) chờ tiếp nhận → (3) CSKH xác minh model, triệu chứng → (4) tra tài liệu và gửi hướng dẫn → (5) khách phản hồi/đóng ticket hoặc chuyển tuyến.
- **Bottleneck:** Chờ nhân viên và khâu tra cứu/soạn câu trả lời; **8 phút thao tác nhân viên/ticket** là giả định, không bao gồm thời gian chờ hàng đợi chưa đo.
- **AI Step:** Rule kiểm tra rủi ro → LLM/RAG truy tài liệu được duyệt → khách tự phục vụ khi ca đủ điều kiện; escalation nếu nguy hiểm/thiếu nguồn.
- **Metric:** Đạt **≥70% automated resolution** trên tập ca rủi ro thấp; chất lượng an toàn 0 vi phạm nghiêm trọng trên tập test; giảm P95 first-response time sau khi có baseline.
- **Quick architecture:** [x] Rule + LLM Feature + RAG; [ ] Agentic Loop; [ ] Rule-only (đối chứng).

### Quick Problem Card #2 — Thu thập thông tin sự cố trước khi chuyển CSKH
- **Actor:** Khách hàng và CSKH.
- **Workflow giả định:** Khách mô tả tự do → CSKH hỏi model/phiên bản → trao đổi bổ sung → tạo ticket hoàn chỉnh → chuyển bộ phận.
- **Bottleneck:** Thiếu dữ liệu đầu vào khiến phải hỏi nhiều vòng; giả định **3 phút/ticket** cho việc hỏi bổ sung.
- **AI step:** Trích xuất và yêu cầu thông tin thiếu theo biểu mẫu đã duyệt, không tự phán đoán mã lỗi.
- **Metric mục tiêu:** ≥95% ticket đầu vào có đủ model và triệu chứng trên bộ test; giảm số vòng hỏi lại.
- **Architecture:** [x] LLM Feature + rule validation.

### Quick Problem Card #3 — Tự động kiểm tra hồ sơ bảo hành
- **Actor:** Nhân viên bảo hành, khách hàng.
- **Workflow giả định:** Nhận ticket → nhập serial → so điều kiện → yêu cầu giấy tờ thiếu → chuyển xét duyệt.
- **Bottleneck:** Thiếu hồ sơ và nhập lại dữ liệu; thời gian **5 phút/ticket** giả định.
- **AI step:** Trích xuất thông tin từ mô tả, rule kiểm tra trường thiếu. Không tự đưa kết luận bảo hành.
- **Metric mục tiêu:** ≥95% trường thiếu được phát hiện trên tập test; 0 quyết định bảo hành không được duyệt.
- **Architecture:** [x] Rule + LLM extraction; không cần Agent.

## Quyết định chọn use case
Chọn Card #1 vì tạo ra **business outcome có thể đo** là tỷ lệ tự giải quyết trên ca rủi ro thấp, hỗ trợ 24/7 và giảm hàng đợi. Card #2 hữu ích nhưng chủ yếu cải thiện nhập liệu; Card #3 có dữ liệu bảo hành nhạy cảm và cần tích hợp nghiệp vụ/approval phức tạp hơn.

## Câu hỏi discovery cần xác thực
1. Số active users và ticket/tháng thật là bao nhiêu? 2. Hiện đã có FAQ, bot, IVR, ticketing hay hỗ trợ 24/7 chưa? 3. Phân bổ top intents/rủi ro? 4. First response P50/P95 và AHT thực tế? 5. Tỷ lệ tự giải quyết hiện tại, CSAT, reopen rate, cost/ticket? 6. Có quyền dùng tài liệu, logs đã ẩn danh và chủ sở hữu dữ liệu không?
