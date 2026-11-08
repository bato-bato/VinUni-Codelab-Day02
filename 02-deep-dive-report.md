# 02 - Deep-Dive Report: Điều Phối Sự Cố Pin & Trạm Sạc (Xanh SM)

## 1. Current-State Workflow (Quy trình hiện tại)

Quy trình xử lý sự cố khi tài xế Xanh SM báo xe sắp cạn kiệt pin hoặc lỗi trạm sạc:

```text
[Bước 1: Nhận cuộc gọi] ──(🔄 Handoff)──> [Bước 2: Tra cứu GPS] ──(🔄 Handoff)──> [Bước 3: Tra trạm sạc trống 🔴] ──> [Bước 4: Soạn tin chỉ dẫn 🔴] ──> [Bước 5: Gọi xe cứu hộ (nếu pin < 5%)]
- B1: Điều phối viên nhận cuộc gọi báo sự cố từ tài xế (⏱ 2 phút).
- B2: Mở hệ thống GPS nội bộ tra cứu vị trí xe (⏱ 2 phút).
- B3: Mở Dashboard VinFast tra cứu trạm sạc còn trụ trống phù hợp chuẩn xe (⏱ 5 phút 🔴 Bottleneck).
- B4: Soạn tin nhắn hướng dẫn lộ trình và gửi tài xế (⏱ 5 phút 🔴 Bottleneck).
- B5: Liên hệ điều xe cứu hộ sạc pin lưu động nếu pin < 5% (⏱ 1 phút).
⏱ Tổng thời gian xử lý thủ công: 15 phút/lượt.
```

---

## 2. Problem Statement (6-Field)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) tại Trung tâm Điều vận Xanh SM (GSM). |
| **2. Current Workflow** | Khi tài xế báo hết pin, điều phối viên thủ công kiểm tra GPS xe, dò trạm sạc VinFast còn trụ trống, gõ tin nhắn SMS chỉ đường và gọi cứu hộ nếu xe cạn pin (15 phút/ca). |
| **3. Bottleneck** | Bước 3 & 4 (mất 10 phút): Dò tìm thủ công trạm sạc tương thích cổng sạc và soạn văn bản hướng dẫn chi tiết bằng tiếng Việt. |
| **4. Business Impact** | ~80 sự cố/ngày tại Hà Nội; lãng phí ~20 giờ làm việc/ngày của điều phối viên; tăng tỷ lệ hủy chuyến và rò rỉ doanh thu ~15% do xe nằm đường. |
| **5. Success Metric** | - Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút/lượt.<br>- Tỷ lệ đề xuất đúng trạm sạc phù hợp cổng sạc đạt >= 98%. |
| **6. Operational Boundary** | **Được làm:** Đọc GPS, tra cứu trạng thái trụ sạc, draft tin nhắn hướng dẫn kèm tag bắt buộc `[DRAFT_ONLY]`.<br>**CẤM:** Tự động gửi tin khi chưa có người duyệt (HITL); cấm điều xe đến trạm > 5km khi pin < 5% (phải kích hoạt `dispatch_mobile_charger`). |

---

## 3. Future-State Flow & AI Fit

- **AI Fit:** Chọn **LLM Feature** (quy trình có cấu trúc nghiệp vụ rõ ràng, rủi ro cao nên dùng LLM để trích xuất và draft văn bản có người duyệt, không cần Agent tự trị hoàn toàn).
- **Quy trình tương lai (Future-State):**

```text
[B1: Tiếp nhận cuộc gọi] 
         │
         ▼
[B2: 🔵 AI Auto-pull GPS & trích xuất dữ liệu trạm sạc trống]
         │
         ▼
[B3: 🔵 AI soạn bản nháp [DRAFT_ONLY] hoặc lệnh cứu hộ pin]
         │
         ▼
[B4: 🟢 Điều phối viên review, chỉnh sửa và bấm Duyệt gửi (HITL)]
         │
         ├──► (Nếu AI sinh lỗi / chậm > 10s) ──► ↩️ Fallback: Điều phối viên tự thao tác thủ công như cũ.
```

---

## 4. Evaluate (Đánh giá & Quyết định)

### AI Readiness Checklist:
- [x] Có sẵn dữ liệu vị trí xe, trạng thái trụ sạc VinFast qua API nội bộ.
- [x] Rủi ro khi AI sai nằm trong tầm kiểm soát nhờ cơ chế Human-in-the-loop (HITL) và tag `[DRAFT_ONLY]`.
- [x] Đội ngũ điều phối viên sẵn sàng chuyển đổi để giảm tải áp lực giờ cao điểm.

### Quyết định: **GO (Triển khai Prototype)**
- **Lý giải (Justification):** Bài toán có điểm nghẽn rõ ràng, ROI cao (tiết kiệm 80% thời gian xử lý sự cố). Sử dụng kiến trúc LLM Feature có ranh giới nghiêm ngặt và con người duyệt (HITL) giúp loại bỏ triệt để rủi ro an toàn vận hành xe điện.

