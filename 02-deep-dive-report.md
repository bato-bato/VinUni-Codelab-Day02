# 02 — DEEP-DIVE REPORT | VinSmart AI HomeCare — GO for offline prototype

> **Nhóm:** [ĐIỀN TÊN NHÓM] | **Họ tên và email thành viên:** [ĐIỀN ĐẦY ĐỦ] | **VinUni Lab 02 – AI Product Scoping**
> **Tình trạng dữ liệu:** Case study giả định. **15.000 là số người dùng giả định**, KHÔNG phải người dùng hoạt động được VinSmart xác nhận. 10.000+ lượt tải ứng dụng KHÔNG đồng nghĩa 10.000+ người dùng. Mọi metric và chi phí dưới đây là giả định thử nghiệm, cần đo lại.

## Executive decision
**[x] GO** cho **offline prototype có giới hạn** với dữ liệu/tài liệu mẫu; **[ ] chưa GO production**. Không được hiểu là đã xác minh nhu cầu hoặc chứng minh ROI của VinSmart. Không xây một agent tự trị; dùng rule router + LLM/RAG + fallback đến CSKH.

## Phase 3.1 — Current-State Workflow Mapping (baseline giả định)
**Scope:** Chỉ những câu hỏi hướng dẫn sử dụng và sự cố kết nối cơ bản (thiết bị offline, app không thấy thiết bị, kiểm tra Wi-Fi). Không nhận thao tác sửa điện, thay linh kiện, bảo hành hay remote device control.

| Bước | Actor | Công việc giả định | Thời gian thao tác | Handoff và vấn đề |
|---|---|---|---:|---|
| 1 | Khách → kênh CSKH | Gửi ticket/mô tả | — | 🔄 Chuyển thông tin sang CSKH; **thời gian chờ chưa đo** |
| 2 | CSKH | Đọc ticket, hỏi model/triệu chứng | 2 phút | Thiếu dữ liệu |
| 3 | CSKH | Tra FAQ/manual, xác định hướng dẫn an toàn | 3 phút | 🔴 Bottleneck: tra cứu nội dung lặp lại |
| 4 | CSKH | Biên soạn và gửi hướng dẫn | 2 phút | 🔴 Bottleneck: soạn lại các câu quen thuộc |
| 5 | CSKH | Ghi nhận kết quả / chuyển tuyến | 1 phút | 🔄 Chuyển khách/tuyến 2 |
| **Tổng** | | **Handling time** (không gồm thời gian chờ) | **8 phút/ticket** | **Giả định chưa đo** |

Sơ đồ `04-workflow-diagram.png` thể hiện **current-state giả định**, không khẳng định VinSmart đang vận hành chính xác như vậy. Không suy diễn 765 ticket thành 765 nhân viên.

### Volume và năng lực hỗ trợ (tất cả là giả định)
- **15.000 người dùng**; mỗi tháng **10%** phát sinh **1 ticket/người** → **1.500 tickets/tháng** (không phải 1.500 người/tháng được xác minh).
- AHT 8 phút → **12.000 phút = 200 giờ xử lý ticket/tháng** nếu toàn bộ cần CSKH.
- Giả định **70%** ticket là ca rủi ro thấp: **1.050 tickets**.
- Giả định AI giải quyết thành công **70% trong nhóm 1.050** → **735 tickets/tháng tự giải quyết**.
- **765 tickets/tháng** vẫn cần CSKH (gồm 315 ca low-risk không tự giải quyết và 450 ca khác).
- Nỗ lực CSKH còn lại (giả định AHT như cũ): **765 × 8 / 60 = 102 giờ/tháng**; tiết kiệm trần **98 giờ/tháng**, **không phải 765 nhân viên**.
- Với 120 giờ hữu dụng/FTE/tháng **chuyên dành cho xử lý ticket**, tải công việc lý thuyết: **200/120 ≈ 1,67 FTE** trước AI; **102/120 = 0,85 FTE** sau AI. **Không phải dự báo headcount**: trực ca, đa nhiệm, lưu lượng đỉnh và xử lý kỹ thuật làm nhu cầu khác đi.

## Phase 3.2 — Problem Statement (6-field)
| Field | Nội dung |
|---|---|
| 1. Actor / Operator | Người dùng Smart Home cần hỗ trợ nhanh; nhân viên CSKH phụ trách ticket; tuyến 2 xử lý ca nguy hiểm/phức tạp. |
| 2. Current Workflow | Khách gửi ticket → nhân viên xác minh triệu chứng → tra tài liệu → phản hồi → ghi nhận/chuyển tuyến. **Đây là workflow giả định để đánh giá**, chưa phải quy trình VinSmart đã xác minh. |
| 3. Bottleneck | Các ca lặp lại rủi ro thấp vẫn phải đợi nhân viên; FAQ khó tìm theo ngôn ngữ tự do; thiếu kênh tự phục vụ được kiểm chứng. |
| 4. Business Impact | **Kịch bản**: 1.500 ticket/tháng × 8 phút = 200 giờ thao tác; hàng đợi và khả năng hỗ trợ ngoài giờ chưa có dữ liệu đo. |
| 5. Success Metric | Pilot: ≥70% resolved-without-agent **trong ca low-risk đủ điều kiện**; 100% ca nguy hiểm trong safety set được escalation; 0 hướng dẫn nguy hiểm trong test; ≥95% grounded answers; tỷ lệ reopen/CSAT không kém baseline; đo P95 first response thực tế. |
| 6. Operational Boundary | Chỉ trả lời tự động nếu **rule cho phép + nguồn hướng dẫn khớp + đầy đủ thông tin + kiểm tra output đạt**; trường hợp nguy hiểm, thiếu tài liệu, yêu cầu hành động lên thiết bị, bảo hành, thanh toán → `ESCALATE` hoặc `ASK_MORE_INFO`; không sửa điện, không remote action, không tự bịa mã lỗi. |

**Statement:** Với quy mô giả định 15.000 người dùng và 1.500 ticket/tháng, các yêu cầu hỗ trợ kết nối Smart Home thông thường có thể làm khách chờ nhân viên dù nội dung đã có trong tài liệu. Đề xuất thử nghiệm cổng tự phục vụ VinSmart AI HomeCare để giải quyết ≥70% ca low-risk đủ điều kiện mà không cần nhân viên, giữ nguyên khả năng chuyển tuyến và không tăng lỗi an toàn, đánh giá bằng bộ test và so sánh với FAQ/rule-only baseline.

## Phase 3.3 — AI Fit và Future-State Flow

| Phương án | Lợi ích | Hạn chế | Quyết định |
|---|---|---|---|
| Rule / FAQ / search keyword | Rẻ, có thể giải thích, ít rủi ro | Khó hiểu nhiều cách diễn đạt | **Bắt buộc làm baseline** |
| **Rule router + LLM Feature + RAG** | Đọc mô tả tự do, trích tài liệu, soạn hướng dẫn theo ngữ cảnh | Hallucination, cost/task, cần eval | **Chọn cho offline prototype** |
| Agentic Loop | Có thể gọi tool nhiều bước | Chưa cần tự chủ; tăng rủi ro side effect/ops | **Chưa chọn** |

**Future flow:** Khách gửi câu hỏi → Rule safety triage → nếu nguy hiểm chuyển người ngay → nếu thiếu model/mã lỗi thì hỏi lại → retrieval từ tài liệu được duyệt → LLM tạo JSON có citations → deterministic output validation (policy, citations, risk, schema) → **chỉ ca low-risk đã pass** mới được hệ thống trả lời tự động → ghi log & khảo sát resolved/reopen → nếu fail/khách yêu cầu nhân viên thì escalation.

**HITL:** CSKH review 100% ca medium/high risk, các ca không đủ grounding, ca bị phản hồi không hữu ích; kiểm tra mẫu ngẫu nhiên ca low-risk tự động. **Không** yêu cầu 100% câu trả lời phải có người duyệt (sẽ triệt tiêu lợi ích scale).

**Fallback:** KB miss, timeout, model lỗi JSON, confidence không xác lập được, chính sách không cho phép, dấu hiệu nguy hiểm → không xuất câu trả lời tự động; gửi mã ticket, chuyển tuyến và thông báo khách; ở giai đoạn offline không được gửi cho khách thật.

## Evaluation plan và gates
1. **Baseline**: FAQ tìm kiếm keyword + nhân viên xử lý bằng tài liệu mẫu, cùng một bộ ca; ghi thời gian, accuracy và số ca cần escalations.
2. **Dataset ban đầu:** 100 tình huống giả lập được gắn nhãn: 60 low-risk (nhiều cách diễn đạt), 20 thiếu thông tin/KB miss, 20 high-risk & prompt injection; bổ sung ≥30 ca thật đã ẩn danh chỉ khi có phép.
3. **Metrics**: eligible-low-risk AR = số ca low-risk giải quyết được không cần CSKH / tổng ca low-risk đủ điều kiện; safety recall; groundedness; hallucination; P50/P95 response; reopen trong 7 ngày; CSAT; cost per resolved request.
4. **Gate mở pilot nhỏ:** ≥70% eligible-low-risk AR; safety recall **100% trên safety set**, 0 hướng dẫn nguy hiểm; ≥95% câu trả lời có trích đúng tài liệu; hiệu quả ≥ rule/FAQ baseline; review kết quả bởi owner. Những ngưỡng này là **mục tiêu**, chưa đo.
5. **Rollback:** Tắt nút auto-send / chuyển toàn bộ về FAQ hoặc nhân viên, giữ log và version prompt/KB; người phụ trách CSKH có quyền tắt.

## Business case: ROI không được tô hồng
| Giả định mô phỏng | Giá trị / phép tính |
|---|---|
| 1.500 tickets, 8 phút AHT | 200 giờ/tháng |
| 735 tickets tự giải quyết | 98 giờ/tháng không cần CSKH (trần lý thuyết) |
| 50.000 VND/giờ chi phí lao động phân bổ | **4,9 triệu VND/tháng** giá trị giờ công quy đổi, **không chắc thành tiết kiệm tiền mặt** |
| Nếu chi phí vận hành + QA + bảo trì = 2 triệu/tháng (giả định minh họa) | Giá trị ròng quy đổi tối đa ≈ **2,9 triệu/tháng**, **chưa tính chi phí build, phân bổ hạ tầng, sai sót và chi phí cơ hội** |
| Nếu vận hành = 6 triệu/tháng (giả định minh họa) | Giá trị quy đổi giờ công **không bù chi phí** → cần tính thêm giá trị hỗ trợ ngoài giờ/CSAT hoặc **NO-GO triển khai** |

**Nhấn mạnh:** Không dùng các phép tính trên để khẳng định VinSmart tiết kiệm hàng triệu đồng thật. Cần đo **cost-per-successful-resolution**, không chỉ cost-per-ticket và thời gian mô hình.

### Stakeholders / RACI-lite
| Hoạt động | CSKH/Product owner | AI Engineer | Platform/Data | Safety/Quality |
|---|---|---|---|---|
| Định nghĩa scope, metric | **A/R** | C | I | C |
| Baseline, tập test | R | **A/R** | C | C |
| Xây prototype & eval | C | **A/R** | R | C |
| Cho phép auto-answer và policy | **A** | R | C | **R** |
| Theo dõi lỗi/rollback | **A** | R | **R** | R |

## Phase 5 — EVALUATE (GO / NOT YET / NO-GO)
| AI readiness gate | Đánh giá hiện tại | Hành động |
|---|---|---|
| Value | Giả thuyết có logic; **chưa đo ticket thật** | Khảo sát và đo top intents |
| Baseline | Có baseline giả định 8 phút, chưa đo | FAQ/rule-only benchmark |
| Eval/data | Có thể tạo sample 100 ca giả lập | Gắn nhãn + kiểm thử offline |
| Tolerance | Có thể chặn auto-answers nguy hiểm bằng router + fallback, nhưng chưa chứng minh zero-risk | Safety tests + code validator + người duyệt policy |
| Operations | Chưa có owner thực tế và logs production | Gán owner trước khi pilot thực |

**Quyết định: [x] GO – prototype offline, [ ] NOT YET – external pilot, [ ] NO-GO.**

**Justification:** Kiến trúc đơn giản, phạm vi hẹp và bộ test giả lập cho phép thử nhanh hypothesis mà không tác động khách thật. Đủ điều kiện nghiên cứu kỹ thuật, **không đủ điều kiện chứng minh lợi ích thương mại hoặc tự động gửi trong môi trường thật**. Sau prototype, nếu không thắng rule-only baseline hoặc ROI âm, phải **NO-GO** cho triển khai; nếu thiếu logs/approval thì **NOT YET**.

**Next smallest experiment (1–2 tuần nghiên cứu, không phải cam kết tiến độ):** 100 ca mô phỏng; rule-only search vs LLM/RAG chạy offline; đo AR, safety, cost, latency, đánh giá thủ công theo rubric; không mở API hành động hoặc auto-send thực.
