# 01 - Problem Scan & Quick-Assess (Vin Smart Future)

## Phase 1 — SCAN: Bảng Quét Cơ Hội (4 Lenses)

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Tốn thời gian | Điều phối viên tra cứu thủ công vị trí xe, tìm trạm sạc VinFast còn trụ trống hoặc gọi xe cứu hộ khi tài xế báo hết pin (mất 15–20 phút/lượt). |
| 2 | **Vinhomes** | Lặp lại | Ban quản lý phân loại và định tuyến thủ công hàng trăm phản ánh của cư dân từ App Resident về các bộ phận kỹ thuật, vệ sinh, an ninh (10–12 phút/phiếu). |
| 3 | **VinFast** | AI-upgrade | Khách hàng mô tả lỗi xe bằng tiếng Việt tự nhiên ("xe kêu lục cục bánh trước"), nhân viên CSKH phân loại sai mã lỗi kỹ thuật ban đầu gây chậm xử lý. |
| 4 | **Vinmec** | Stakeholder Pain | Bác sĩ mất 25–30 phút/bệnh nhân để đọc EHR và gõ tay tóm tắt hồ sơ xuất viện (Discharge Summary), gây quá tải hành chính. |
| 5 | **Vinpearl** | Tốn thời gian | Xử lý email đặt phòng/vé theo đoàn từ các công ty lữ hành: kiểm tra quỹ phòng trên PMS và soạn báo giá thủ công (30–40 phút/đoàn). |

---

## Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

### Quick Problem Card #1: Xanh SM — Hỗ trợ sự cố pin & Trạm sạc khẩn cấp
```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Điều phối hỗ trợ tài xế taxi điện Xanh SM │
│ gặp sự cố pin yếu tìm trạm sạc gần nhất hoặc cứu hộ.        │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau (Actor)? Tài xế (chờ đợi) & Điều phối viên (quá tải) │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Nhận cuộc gọi ──> 2. Tra GPS ──> 3. Tra trạm sạc trống │
│   ──> 4. Soạn tin chỉ dẫn ──> 5. Gọi xe cứu hộ nếu pin < 5% │
│                                                             │
│ Bước tốn nhất? Bước 3 & 4 (⏱ 10–12 phút/lượt)                │
│ AI hỗ trợ bước nào? Bước 3 & 4: Tra cứu và draft tin chỉ dẫn│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   - Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút.  │
│   - Tỷ lệ chỉ dẫn đúng loại cổng sạc đạt >= 98%.            │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

### Quick Problem Card #2: Vinhomes — Phân loại phản ánh cư dân
```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Phân loại và định tuyến tự động phản ánh  │
│ của cư dân gửi qua ứng dụng Vinhomes Resident.              │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)? Cư dân (chờ lâu) & Nhân viên CSKH BQL  │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Nhận phản ánh ──> 2. Đọc hiểu ──> 3. Gán nhãn nghiệp vụ│
│   ──> 4. Chuyển phiếu cho đội kỹ thuật/vệ sinh              │
│                                                             │
│ Bước tốn nhất? Bước 2 & 3 (⏱ 8–10 phút/phiếu)               │
│ AI hỗ trợ bước nào? Bước 2 & 3: Đọc hiểu và phân luồng      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   - Giảm thời gian định tuyến từ 12 tiếng ──> dưới 5 phút.  │
│   - Độ chính xác phân loại tự động đạt >= 92%.              │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

### Quick Problem Card #3: Vinmec — Tóm tắt hồ sơ xuất viện
```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Hỗ trợ Bác sĩ soạn thảo bản tóm tắt hồ sơ │
│ xuất viện từ bệnh án điện tử (EHR).                         │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị tại Vinmec             │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Mở EHR ──> 2. Đọc xét nghiệm ──> 3. Gõ tóm tắt chẩn    │
│   đoán & dặn dò ──> 4. Ký duyệt hồ sơ xuất viện             │
│                                                             │
│ Bước tốn nhất? Bước 2 & 3 (⏱ 20–25 phút/ca)                 │
│ AI hỗ trợ bước nào? Bước 2 & 3: Trích xuất EHR & draft nháp │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   - Giảm thời gian viết hồ sơ từ 25 phút ──> dưới 5 phút.   │
│   - Độ chính xác trích xuất lâm sàng: 100%.                 │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

---

## Lựa chọn cho Phase 3 (Deep-Dive)

- **Bài toán được chọn:** **Card #1 — Xanh SM (Sự cố pin & Trạm sạc)**.
- **Lý do:** Tác động trực tiếp đến vận hành thời gian thực, dữ liệu đầu vào rõ ràng, và ranh giới an toàn (`[DRAFT_ONLY]`, pin < 5% điều cứu hộ) phù hợp chuẩn kiểm thử của bài lab.
