# Lab 02 — Deliverable 01: Problem Scan & Quick-Assess Cards (Bài Làm Cá Nhân)

- **Họ và tên:** Vũ Trọng Quang
- **MSSV:** 2A202603535
- **Đơn vị / Dự án:** VinUni Living Lab — Smart Agriculture Pilot (Vườn Cà Chua Nhà Màng 0,1 ha)

---

## 🏛️ Bối cảnh thực tế (Context)

Trong khuôn khổ mô hình **Living Lab (Mô hình Pilot chuẩn)** của **VinUniversity** và hệ sinh thái **Vin Smart Future**, tôi tiến hành quét (Scan) các điểm nghẽn vận hành thực tế bằng **4 Lenses** (Lặp lại, Tốn thời gian, AI có thể tốt hơn, Pain từ người khác) tại **Vườn Cà Chua Công Nghệ Cao 0,1 ha ($1.000\text{ m}^2 \approx 2.500$ gốc)** và các đơn vị vận hành liên quan. Mục tiêu cá nhân là lựa chọn một bài toán có **ROI dương ngay từ vụ/tháng đầu tiên**, chi phí hạ tầng tối ưu (Serverless), có dữ liệu chuẩn (VietGAP/SOP) để kiểm chứng (Eval) và thiết lập ranh giới an toàn **Human-in-the-Loop (HITL)** chặt chẽ.

---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Sử dụng **4 Lenses** quét qua hoạt động vận hành thực địa tại **Vườn Cà Chua Công Nghệ Cao 0,1 ha (Living Lab)** và các đơn vị thành viên:

| # | Dự án / Đơn vị | Lens | Mô tả ngắn bài toán (Bottleneck & Tổn thất) |
|---|---|---|---|
| **1** | **Vườn Cà Chua 0,1 ha (Living Lab)** | **Tốn thời gian & AI có thể tốt hơn** | Chủ vườn chăm sóc $2.500$ gốc cà chua nhà màng ($1.000\text{ m}^2$) mỗi sáng phải đi kiểm tra lá và đo bút EC/pH thủ công; khi lá xoăn/đốm hoặc đổi thời tiết phải lật sổ tay giấy/hỏi diễn đàn trồng trọt mất **2 – 3 giờ** để tìm thuốc BVTV và tính lại công thức phân A-B cho bồn $1.000\text{ lít}$, dễ nhầm lẫn thiếu vi lượng với nấm/vi khuẩn gây mất **10% – 15% sản lượng** ($\approx 25 - 35\text{ triệu VNĐ/vụ}$). |
| **2** | **Vườn Cà Chua 0,1 ha (Living Lab)** | **Lặp lại** | Ghi chép nhật ký canh tác VietGAP thủ công trên sổ giấy (lượng nước tưới, chỉ số EC/pH, ngày cách ly thuốc BVTV) cuối tuần phải nhập lại vào Excel để truy xuất nguồn gốc, tốn **4 – 5 giờ/tuần** và hay quên ghi ngày cách ly thu hoạch. |
| **3** | **Xanh SM (GSM)** | **Tốn thời gian** | Điều phối viên (Dispatcher) xử lý thủ công các cuộc gọi khẩn cấp từ tài xế báo sự cố hết pin giữa đường: phải tra cứu định vị GPS, tìm trụ sạc VinFast trống phù hợp cổng sạc và soạn tin nhắn hướng dẫn hoặc gọi xe cứu hộ pin di động (mất **15 phút/lượt**, ~80 ca/ngày). |
| **4** | **VinFast** | **Lặp lại** | Đối chiếu hóa đơn sạc điện và đối soát sản lượng điện tiêu thụ hằng tuần từ hàng nghìn trụ sạc nhượng quyền/đối tác ngoài với hệ thống ERP tài chính. |
| **5** | **Vinhomes** | **AI có thể tốt hơn** | Phân loại và điều hướng tự động các phản ánh/khiếu nại kèm hình ảnh của cư dân trên App Vinhomes Resident (hỏng đèn, thấm nước, tiếng ồn) đến đúng bộ phận kỹ thuật tòa nhà (hiện xử lý rập khuôn, SLA phản hồi mất tới **12 giờ**). |
| **6** | **Vinmec** | **Pain từ người khác** | Bác sĩ lâm sàng mất **20 – 30 phút/bệnh nhân** để tổng hợp kết quả xét nghiệm, chẩn đoán hình ảnh và ghi chú điều trị thành bản tóm tắt hồ sơ xuất viện (Discharge Summary) bằng ngôn ngữ dễ hiểu cho người bệnh. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Từ danh sách 6 bài toán trên, tôi chọn **Top 3 bài toán tiềm năng nhất** (#1 — Vườn Cà Chua 0,1 ha Chẩn đoán bệnh lá & Pha dinh dưỡng; #3 — Xanh SM Xử lý sự cố sạc pin thực địa; #5 — Vinhomes Phân loại phản ánh cư dân) để đánh giá nhanh qua **Quick Problem Cards**:

## 🍅 Card #1 — Hệ thống AI Chẩn đoán Bệnh Lá & Trợ lý Pha Dinh Dưỡng Tưới Nhỏ Giọt (Vườn Cà Chua 0,1 ha)

```text
┌───────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1 (BÀI TOÁN CHỌN DEEP-DIVE)                           │
│                                                                           │
│ Bài toán (1 câu): Chủ vườn cà chua nhà màng 0,1 ha (2.500 gốc) mất 2-3 giờ│
│ mỗi khi phát hiện lá bất thường hoặc thời tiết đổi mùa để tra cứu sổ tay   │
│ chẩn đoán bệnh và tính toán lại công thức pha phân A-B cho bồn 1.000 lít. │
│                                                                           │
│ Công ty / Đơn vị: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  [ ] Vinmec      │
│                   [x] Khác: VinUni Living Lab / Smart Agri (Pilot 0,1 ha) │
│                                                                           │
│ Ai đang đau (Actor)? Chủ vườn kiêm người vận hành trực tiếp (chăm sóc     │
│ 2.500 gốc cà chua mỗi ngày) & Kỹ sư nông nghiệp hỗ trợ từ xa.             │
│                                                                           │
│ Workflow thủ công hiện tại (5 bước):                                      │
│   1. Đi kiểm tra 2.500 gốc & đo bút EC/pH, xem nhiệt/ẩm nhà màng (30p)    │
│   ──> 2. Phát hiện lá xoăn/đốm hoặc nắng đổi sang mưa, chụp ảnh (5p)      │
│   ──> 3. Lật sổ tay giấy / tra cứu diễn đàn đoán bệnh (60-90p) 🔴         │
│   ──> 4. Tính tay lượng phân A-B & ml thuốc BVTV cho bồn 1.000L (30-60p)🔴│
│   ──> 5. Pha chế thủ công vào bồn & mở van tưới nhỏ giọt (15p)            │
│                                                                           │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 120 - 180 phút/ca)         │
│   - Nhầm lẫn giữa thiếu vi lượng (Ca, Mg, Bo) và nhiễm nấm/vi khuẩn.      │
│   - Tính sai tỷ lệ phân A-B theo bức xạ nhiệt hoặc lỡ "thời điểm vàng"    │
│     cách ly dịch bệnh mốc sương / héo xanh.                               │
│                                                                           │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4                          │
│   - Qua Zalo/Telegram Bot: Nhận ảnh chụp lá + chỉ số EC/pH ──> Vision LLM │
│     phân tích triệu chứng ──> RAG truy xuất giáo trình VietGAP chuẩn ──>  │
│     Trả về phác đồ có trích dẫn trang & tính sẵn số gam phân/ml thuốc     │
│     cần pha cho bồn 1.000 lít (kèm nút "Approve" HITL).                   │
│                                                                           │
│ Đo thành công bằng gì (Metric có số)?                                     │
│   - System Metric: Top-3 Accuracy chẩn đoán >= 85% trên bộ test 100 ảnh   │
│     lá thực tế; Trích xuất đúng 100% liều lượng phân/thuốc (Groundedness).│
│   - Business KPI: Giảm thời gian ra quyết định từ 2 giờ ──> dưới 2 phút/ca│
│     Giảm tỷ lệ cây chết/nhổ bỏ xuống dưới 3%/vụ (bảo vệ 25-35 triệu/vụ).  │
│                                                                           │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] Workflow có State +          │
│                                              Multimodal LLM Feature (RAG) │
└───────────────────────────────────────────────────────────────────────────┘
```

---

## 🚕 Card #2 — Xanh SM: Xử lý sự cố sạc pin thực địa & Điều phối cứu hộ

```text
┌───────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                     │
│                                                                           │
│ Bài toán (1 câu): Tài xế Xanh SM báo cáo sự cố hết pin giữa đường cần     │
│ điều phối viên tra cứu trụ sạc trống phù hợp hoặc điều xe cứu hộ pin.     │
│                                                                           │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM (GSM)  [ ] Vinhomes          │
│                     [ ] Vinmec   [ ] Khác: ______________________________ │
│                                                                           │
│ Ai đang đau (Actor)? Tài xế Xanh SM (chờ đợi) & Điều phối viên (quá tải). │
│                                                                           │
│ Workflow thủ công hiện tại (5 bước):                                      │
│   1. Tài xế gọi tổng đài báo sắp cạn pin (2p)                             │
│   ──> 2. Dispatcher tra cứu tọa độ GPS của xe trên bản đồ nội bộ (2p)     │
│   ──> 3. Mở Dashboard tìm trạm sạc VinFast còn trụ trống phù hợp (5p) 🔴  │
│   ──> 4. Soạn tin nhắn chỉ đường gửi qua App tài xế (5p) 🔴               │
│   ──> 5. Gọi đội xe cứu hộ pin di động nếu pin < 5% (1p)                  │
│                                                                           │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 10 - 12 phút/lượt)         │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4                          │
│   (Auto-pull GPS & trụ trống -> LLM draft SMS hướng dẫn gắn [DRAFT_ONLY]  │
│   hoặc tự động đề xuất dispatch_mobile_charger nếu pin < 5%).             │
│                                                                           │
│ Đo thành công bằng gì (Metric có số)?                                     │
│   - Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút/lượt.           │
│   - Tỷ lệ chỉ dẫn đúng loại cổng sạc & an toàn ngưỡng pin đạt >= 98%.     │
│                                                                           │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent       │
└───────────────────────────────────────────────────────────────────────────┘
```

---

## 🏢 Card #3 — Vinhomes: Phân loại & Điều hướng phản ánh kỹ thuật cư dân

```text
┌───────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                     │
│                                                                           │
│ Bài toán (1 câu): Nhân viên CSKH Ban Quản lý Vinhomes phải đọc và phân    │
│ loại thủ công hàng trăm phản ánh kèm ảnh chụp của cư dân mỗi ngày.        │
│                                                                           │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes                │
│                     [ ] Vinmec   [ ] Khác: ______________________________ │
│                                                                           │
│ Ai đang đau (Actor)? Cư dân (chờ phản hồi lâu) & CSKH Ban quản lý tòa nhà.│
│                                                                           │
│ Workflow thủ công hiện tại (4 bước):                                      │
│   1. Cư dân gửi ticket kèm ảnh lên App Vinhomes Resident                  │
│   ──> 2. CSKH mở từng ticket đọc nội dung & xem ảnh đính kèm (5p) 🔴      │
│   ──> 3. Phân loại mức độ khẩn cấp & gán cho tổ kỹ thuật (Điện/Nước) (5p) │
│   ──> 4. Soạn tin nhắn xác nhận tiến độ xử lý gửi lại cư dân (5p) 🔴      │
│                                                                           │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 4 (⏱ 10 - 15 phút/ticket)       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 & 4                       │
│   (Multimodal LLM đọc ảnh sự cố + phân loại bộ phận + draft phản hồi).    │
│                                                                           │
│ Đo thành công bằng gì (Metric có số)?                                     │
│   - Giảm thời gian phân loại và phản hồi lần đầu từ 12 giờ ──> dưới 5 phút│
│   - Độ chính xác phân luồng đúng tổ kỹ thuật đạt >= 90%.                  │
│                                                                           │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent       │
└───────────────────────────────────────────────────────────────────────────┘
```

---

# 🗳️ Quyết định lựa chọn bài toán để Deep-Dive (Cá nhân)

Tôi quyết định lựa chọn **Card #1 — Hệ thống AI Chẩn đoán Bệnh Lá & Trợ lý Pha Dinh Dưỡng Tưới Nhỏ Giọt qua Zalo/Telegram Bot cho Vườn Cà Chua Quy Mô 0,1 ha ($1.000\text{ m}^2 \approx 2.500$ gốc cà chua nhà màng)** để thực hiện **Phase 3 (Deep-Dive)** và **Phase 5 (Evaluate)** vì 3 lý do cốt lõi:

1. **ROI dương ngay trong tháng đầu tiên trên quy mô Living Lab ($0,1\text{ ha}$):** Cà chua công nghệ cao (cherry/beef nhà màng) đạt doanh thu **150 – 250 triệu VNĐ/vụ**. Chỉ cần cứu được **10% – 15% sản lượng** khỏi bệnh mốc sương/héo xanh và tối ưu độ ngọt (Brix), giá trị bảo vệ đạt **25 – 35 triệu VNĐ/vụ**, trong khi kiến trúc Serverless (Telegram/Zalo Bot + Vision-RAG API) chỉ tốn **200.000 – 300.000 VNĐ/tháng** tiền API token.
2. **Độ sẵn sàng dữ liệu & Khả năng kiểm chứng (Eval Readiness 5/5):** Có sẵn bộ **100 ảnh lá cà chua thực tế** (lá khỏe, thiếu vi lượng, nhiễm nấm/vi khuẩn), **30 kịch bản pha phân A-B** theo giai đoạn sinh trưởng và **3 tài liệu kỹ thuật trồng cà chua nhà màng chuẩn VietGAP** để đo lường lặp lại được (`Top-3 Accuracy >= 85%`, `Groundedness = 100%`).
3. **Ranh giới an toàn (Operational Boundary & HITL) cực kỳ rõ ràng:** AI chỉ phân tích ảnh, trích dẫn đúng trang giáo trình VietGAP và tính sẵn công thức nháp (`[DRAFT_ONLY]`), **tuyệt đối không tự động kích hoạt bơm định lượng hóa chất**, đảm bảo an toàn sinh học 100% cho vườn.
