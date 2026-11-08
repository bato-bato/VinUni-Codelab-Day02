# 01 — Problem Scan (Phase 1 SCAN + Phase 2 QUICK-ASSESS)

**Tên nhóm:** [Field-Space]
**Thành viên:** [Lê Thái Dương – duonglt3737@gmail.com] 

> **Lưu ý trung thực:** Các con số về thời gian, tần suất và tỷ lệ trong tài liệu này là **giả định để minh họa**, chưa phải số liệu đo thực tế từ Vingroup. Khi có dữ liệu thật, cần thay thế (xem mục "Cần xác minh" cuối file).

---

## 🔍 Phase 1 — SCAN: 5 bài toán qua 4 Lenses

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|-----------|------|---------------------|
| 1 | **Vinhomes** | Lặp lại + Tốn thời gian | Phân loại và định tuyến yêu cầu của cư dân (sửa chữa, phí quản lý, thẻ xe) về đúng bộ phận. Nhân viên lễ tân/CSKH phải đọc văn bản tự do, đoán loại việc, rồi chuyển tay. |
| 2 | **Vinmec** | Tốn thời gian | Bác sĩ phải gõ lại ghi chú khám và viết tóm tắt vào hồ sơ. AI có thể soạn nháp để bác sĩ duyệt. |
| 3 | **Xanh SM** | Pain từ người khác | Khách hàng khiếu nại về cước phí hoặc đồ thất lạc, CSKH phải tra chuyến đi rồi soạn phản hồi thủ công, khách chờ lâu. |
| 4 | **VinFast** | AI có thể tốt hơn | Chủ xe hỏi lịch bảo dưỡng, bảo hành, đặt lịch xưởng. Phản hồi hiện còn rập khuôn và chậm, có thể dùng LLM tra cứu tài liệu nội bộ (RAG). |
| 5 | **Vinpearl / VinWonders** | Lặp lại | Khách hỏi lặp đi lặp lại về chính sách đổi/hoàn vé và phòng, giờ mở cửa, điều kiện áp dụng. Nhân viên trả lời đi trả lời lại cùng một nội dung. |

Độ phủ lens: Lặp lại (#1, #5), Tốn thời gian (#1, #2), AI có thể tốt hơn (#4), Pain từ người khác (#3).

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3 từ danh sách: **#1 (Vinhomes), #2 (Vinmec), #3 (Xanh SM).**

### Card #1 — Vinhomes: Phân loại và định tuyến yêu cầu của cư dân

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Yêu cầu của cư dân (văn bản tự do) phải được      │
│ phân loại và chuyển đúng bộ phận, hiện làm thủ công.        │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Lễ tân/CSKH tòa nhà (quá tải, chuyển nhầm);    │
│ cư dân (chờ lâu); bộ phận kỹ thuật/kế toán (nhận nhầm việc) │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Cư dân gửi yêu cầu (app/hotline/tin nhắn/trực tiếp)    │
│   → 2. Lễ tân/CSKH tiếp nhận, ghi nhận                      │
│   → 3. Đọc, hiểu, phân loại, đánh giá mức khẩn              │
│   → 4. Chuyển cho bộ phận (kỹ thuật/kế toán/an ninh)        │
│   → 5. Bộ phận xử lý (hoặc trả lại nếu nhận nhầm)           │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4                   │
│ (⏱ ~6 phút/lượt, cộng vòng trả lại khi chuyển nhầm)         │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 (phân loại,    │
│ tóm tắt, gợi ý bộ phận), nhân viên duyệt ở bước 4           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Thời gian từ lúc nhận đến đúng bộ phận: ~8 phút ──> ≤2 phút │
│ ở 80% yêu cầu thuộc top 5 loại; chuyển nhầm: ~20% ──> ≤5%.  │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
│ (Rule bắt việc khẩn + form cố định; LLM chỉ phân loại)      │
└─────────────────────────────────────────────────────────────┘
```

### Card #2 — Vinmec: Soạn nháp tóm tắt ghi chú khám cho bác sĩ duyệt

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Bác sĩ mất nhiều thời gian chuyển ghi chú khám    │
│ thành tóm tắt trong hồ sơ bệnh án.                          │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Bác sĩ/điều dưỡng (làm giấy tờ ngoài giờ)      │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Khám và ghi chú nhanh                                  │
│   → 2. Nhập lại vào hồ sơ điện tử                           │
│   → 3. Viết tóm tắt/kết luận                                │
│   → 4. Rà soát và ký                                        │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3                   │
│ (⏱ ~12 phút/ca, giả định)                                   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3: soạn NHÁP     │
│ từ ghi chú, bác sĩ sửa và ký (không tự ký, không chẩn đoán) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian làm hồ sơ ~12 phút ──> ≤5 phút/ca; 100% nháp │
│ có bác sĩ duyệt; tỷ lệ phải sửa nội dung lâm sàng ≤ X%      │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### Card #3 — Xanh SM: Xử lý khiếu nại cước phí và đồ thất lạc

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: CSKH phải tra chuyến đi và soạn phản hồi thủ công │
│ cho khiếu nại cước phí/đồ thất lạc.                         │
│ Công ty thành viên: [x] Xanh SM                             │
│                                                             │
│ Ai đang đau? Khách hàng (chờ lâu); CSKH (quá tải)           │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Khách gửi khiếu nại                                    │
│   → 2. CSKH đọc, phân loại                                  │
│   → 3. Tra chuyến đi, lộ trình, cước                        │
│   → 4. Soạn phản hồi                                        │
│   → 5. Duyệt hoàn tiền (nếu có)                             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4                   │
│ (⏱ ~10 phút/lượt, giả định)                                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 và 4: phân     │
│ loại, tóm tắt, soạn nháp phản hồi                           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Thời gian xử lý ~15 phút ──> ≤6 phút; 0 lần AI tự hoàn tiền │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Lý do chọn Card #1 để Deep-Dive

| Tiêu chí | Card #1 Vinhomes | Card #2 Vinmec | Card #3 Xanh SM |
|---------|------------------|----------------|-----------------|
| Tần suất và khối lượng | Cao, lặp lại mỗi ngày | Cao | Trung bình |
| Rủi ro nếu AI sai | Thấp nếu chỉ phân loại (nhân viên duyệt) | **Cao** (nội dung lâm sàng) | Trung bình đến cao (liên quan tiền hoàn) |
| Có thể bắt đầu bằng rule | **Có**, nhiều phần là rule | Ít | Một phần |
| Dữ liệu để đánh giá | Dễ tạo tập yêu cầu có nhãn | Khó (dữ liệu bệnh án nhạy cảm) | Cần dữ liệu chuyến đi |
| Đo được kết quả | Dễ (thời gian, tỷ lệ chuyển nhầm) | Khó hơn | Trung bình |

**Lý do loại các thẻ khác:**
- **Card #2 (Vinmec):** Rủi ro lâm sàng cao và dữ liệu bệnh án nhạy cảm, cần quy trình pháp lý và kiểm soát chặt hơn trước khi làm prototype. Để dành cho giai đoạn sau.
- **Card #3 (Xanh SM):** Liên quan trực tiếp đến hoàn tiền, cần tích hợp dữ liệu chuyến đi. Phạm vi lớn hơn.

**Về rủi ro của Vinhomes:** Thông tin về phí quản lý hoặc tranh chấp căn hộ nếu AI trả lời sai có thể dẫn đến khiếu nại nặng. Vì vậy giải pháp được **thu hẹp thành phân loại và định tuyến**, AI **không trả lời nội dung** về phí, pháp lý hay cam kết thời hạn.

---

## ❓ Cần xác minh trước khi nộp / khi có dữ liệu thật

- Khối lượng yêu cầu mỗi ngày mỗi tòa nhà (giả định ~150).
- Thời gian trung bình phân loại và chuyển yêu cầu (giả định ~8 phút), tỷ lệ chuyển nhầm (giả định ~20%).
- SLA nội bộ hiện tại (giả định: đến đúng bộ phận trong 15 phút).
- Kênh tiếp nhận thực tế và công cụ ghi nhận yêu cầu đang dùng.
