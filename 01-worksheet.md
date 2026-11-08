# Lab 02 — Worksheet: AI Product Scoping (Bài Làm Cá Nhân)

- **Họ và tên:** Vũ Trọng Quang
- **MSSV:** 2A202603535
- **Dự án cá nhân:** Hệ thống AI Chẩn đoán Bệnh Lá & Trợ lý Pha Dinh Dưỡng Tưới Nhỏ Giọt qua Zalo/Telegram Bot (Vườn Cà Chua Quy Mô $0,1\text{ ha}$ — Living Lab)

---

# 🔍 Phase 1 — SCAN (Cá nhân)

### 📝 Danh sách bài toán quét qua 4 Lenses:
| # | Dự án / Đơn vị | Lens | Mô tả ngắn bài toán |
|---|----------------|------|---------------------|
| 1 | **Vườn Cà Chua Nhà Màng 0,1 ha (Living Lab)** | Tốn thời gian & AI có thể tốt hơn | Chủ vườn ($2.500$ gốc cà chua nhà màng) mất 2–3 giờ mỗi khi lá xoăn/đốm hoặc đổi thời tiết để lật sổ tay giấy/tra cứu đoán bệnh và tính lại công thức phân A-B cho bồn $1.000\text{ lít}$, gây thiệt hại 10%–15% sản lượng ($25 - 35\text{ triệu VNĐ/vụ}$). |
| 2 | **Vườn Cà Chua Nhà Màng 0,1 ha (Living Lab)** | Lặp lại | Ghi chép nhật ký canh tác VietGAP thủ công trên sổ giấy (EC/pH, lượng tưới, ngày cách ly thuốc BVTV) và nhập lại vào bảng tính cuối tuần tốn 4–5 giờ/tuần. |
| 3 | **Xanh SM (GSM)** | Tốn thời gian | Điều phối viên xử lý thủ công sự cố tài xế báo hết pin giữa đường: tra cứu GPS, tìm trụ sạc VinFast trống phù hợp và soạn SMS hướng dẫn hoặc gọi cứu hộ pin (mất 15 phút/lượt). |
| 4 | **VinFast** | Lặp lại | Đối chiếu hóa đơn sạc điện và đối soát sản lượng điện tiêu thụ hằng tuần từ hàng nghìn trụ sạc đối tác ngoài với hệ thống ERP. |
| 5 | **Vinhomes** | AI có thể tốt hơn | Phân loại và điều hướng tự động các phản ánh/khiếu nại kèm ảnh chụp của cư dân trên App Vinhomes Resident đến đúng tổ kỹ thuật tòa nhà (SLA hiện mất 12 giờ). |
| 6 | **Vinmec** | Pain từ người khác | Bác sĩ mất 20–30 phút/bệnh nhân để tổng hợp xét nghiệm và bệnh án thành bản tóm tắt hồ sơ xuất viện (Discharge Summary). |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân)

*(Chi tiết cả 3 thẻ Quick Problem Cards được trình bày đầy đủ tại file [`01-problem-scan.md`](01-problem-scan.md)).*

```text
┌───────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1 (BÀI TOÁN CHỌN DEEP-DIVE)                           │
│                                                                           │
│ Bài toán (1 câu): Hệ thống AI Chẩn đoán Bệnh Lá & Trợ lý Pha Dinh Dưỡng   │
│ Tưới Nhỏ Giọt qua Zalo/Telegram Bot cho Vườn Cà Chua Nhà Màng 0,1 ha.     │
│ Đơn vị triển khai: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes                 │
│                    [ ] Vinmec   [x] Khác: Vườn Cà Chua 0,1 ha (Living Lab)│
│                                                                           │
│ Ai đang đau (Actor)? Chủ vườn kiêm người vận hành trực tiếp (2.500 gốc).  │
│                                                                           │
│ Workflow thủ công hiện tại (5 bước):                                      │
│   1. Kiểm tra lá & đo bút EC/pH (30p) ──> 2. Phát hiện lá bệnh/đổi mùa(5p)│
│   ──> 3. Lật sổ tay tra cứu (60-90p) ──> 4. Tính phân A-B bồn 1m3(30-60p) │
│   ──> 5. Pha chế thủ công & mở van tưới (15p)                             │
│                                                                           │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 120 - 180 phút/ca)         │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4 (Vision LLM + RAG API)   │
│                                                                           │
│ Đo thành công bằng gì (Metric có số)?                                     │
│   Top-3 Accuracy >= 85% (100 ảnh test); Groundedness = 100%;              │
│   Giảm thời gian ra quyết định từ 2 giờ ──> dưới 2 phút/ca; Cây chết < 3% │
│                                                                           │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM (Vision+RAG) + Workflow  │
└───────────────────────────────────────────────────────────────────────────┘
```

---

# 🏗️ Phase 3 — DEEP-DIVE (Cá nhân)

## 3.1. Current-State Workflow Mapping
* 🔴 **Bottleneck:** Bước 3 (Lật sổ tay giấy/tra cứu đoán bệnh — dễ nhầm thiếu vi lượng vs nấm/vi khuẩn, mất 60–90 phút) & Bước 4 (Tính tay lượng phân A-B và ml thuốc BVTV cho bồn $1.000\text{ lít}$ theo thời tiết, mất 30–60 phút).
* 🔄 **Handoff:** Chuyển giao thủ công từ quan sát mắt thường/bút đo EC-pH sang tra cứu sổ tay rồi sang đong pha tại bồn $1.000\text{ lít}$.
* Thời gian vận hành trung bình: **Tổng cộng = 140 – 200 phút/lượt (2 – 3 giờ/ca)**.

## 3.2. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Chủ vườn kiêm người vận hành trực tiếp (chăm sóc $2.500$ gốc cà chua mỗi ngày trên diện tích $0,1\text{ ha}$). |
| **2. Current Workflow** | Mỗi sáng đi kiểm tra lá và đo bút EC/pH thủ công $\rightarrow$ Khi thấy lá xoăn/đốm hoặc thời tiết đổi nắng sang mưa $\rightarrow$ Lật sổ tay giấy hoặc hỏi hội trồng trọt để chỉnh công thức phân A-B và tìm thuốc đặc trị. |
| **3. Bottleneck** | Chẩn đoán bằng mắt thường hay nhầm lẫn giữa thiếu vi lượng và nhiễm nấm/vi khuẩn; tính toán lại lượng phân theo thời tiết chậm trễ mất 2 – 3 giờ, làm lỡ "thời điểm vàng" cách ly dịch bệnh. |
| **4. Business Impact** | Phát hiện và xử lý sai làm lây lan ổ bệnh, gây thiệt hại 10% – 15% sản lượng ($\approx 1 - 1,5\text{ tấn}$ quả/vụ, tương đương mất **25 – 35 triệu VNĐ**) và tốn thêm **20% chi phí thuốc BVTV**. |
| **5. Success Metric** | • **System Metric:** Độ chính xác chẩn đoán (Top-3 Accuracy) đạt $\ge 85\%$ trên bộ test 100 ảnh lá thực tế; Trích xuất đúng $100\%$ liều lượng phân/thuốc theo tài liệu gốc (Groundedness).<br>• **Business KPI:** Giảm thời gian ra quyết định xử lý từ 2 giờ xuống **dưới 2 phút/ca**; giảm tỷ lệ cây chết/nhổ bỏ xuống **dưới 3%/vụ**. |
| **6. Operational Boundary** | • **Được phép:** Nhận ảnh chụp lá + chỉ số EC/pH qua Bot $\rightarrow$ Phân tích bệnh, trích dẫn phác đồ VietGAP và tính sẵn số gam phân/ml thuốc cần pha cho bồn $1.000\text{ lít}$ (kèm nhãn `[DRAFT_ONLY]`).<br>• **Không được phép:** Tự động kích hoạt bơm định lượng hóa chất. Chủ vườn phải nhấn nút "Xác nhận (Approve)" trên điện thoại và tự pha chế (Human-in-the-Loop). |

## 3.3. Future-State Flow & AI Fit
* **Xác định mức AI Fit (AI-Fit Matrix):** [x] **Workflow có State + Multimodal LLM Feature (có RAG)** (Đúng chuẩn Decision Shortcut — Trang 17).
* **Future-State Flow:**
  * **Tầng Rule/Workflow:** Cảm biến nhiệt độ/độ ẩm nhà màng gửi cảnh báo tự động qua Telegram khi vượt ngưỡng dễ phát sinh nấm bệnh (độ ẩm $> 85\%$).
  * 🔵 **AI Step:** Chủ vườn gửi ảnh lá bệnh + câu hỏi vào Bot $\rightarrow$ **Vision LLM** phân tích triệu chứng hình ảnh $\rightarrow$ **RAG** truy xuất cơ sở dữ liệu giáo trình trồng cà chua chuẩn $\rightarrow$ Trả về phác đồ điều trị có trích dẫn trang tài liệu rõ ràng và gam phân/ml thuốc cho bồn $1.000\text{ lít}$ (`[DRAFT_ONLY]`).
  * 🟢 **Human Step (HITL):** Hệ thống hiển thị kèm ảnh mẫu bệnh trong giáo trình và trang trích dẫn; Chủ vườn đối chiếu, nhấn nút **"Xác nhận (Approve)"** trên điện thoại và tự pha chế.
  * ↩️ **Fallback:** Mọi lượt hỏi đáp lưu log tự động trên Google Sheets; nếu API lỗi hoặc độ tin cậy thấp, lập tức chuyển về tra cứu bảng PDF gốc trong **10 giây** và tham vấn Kỹ sư Nông nghiệp (Domain SME).

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE (Cá nhân)
*Đã hoàn thiện tại [`starter-code/prompt_prototype.py`](starter-code/prompt_prototype.py) và vượt qua toàn bộ kiểm thử ranh giới an toàn (`[DRAFT_ONLY]`, ngưỡng `< 5%`, cấm tự động bật bơm hóa chất).*

---

# 🏁 Phase 5 — EVALUATE (Cá nhân)

### AI Readiness Checklist (Đạt 5/5 tiêu chí YES — Trang 21 & 35):
1. [x] Có sẵn dữ liệu mẫu/logs sạch để test? *(Có bộ 100 ảnh lá cà chua thực tế + 30 kịch bản pha phân + 3 giáo trình kỹ thuật chuẩn).*
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? *(Có: HITL Approve, nhãn `[DRAFT_ONLY]`, cấm tự động kích hoạt bơm, đối chiếu ảnh mẫu + trang trích dẫn, Fallback về PDF gốc trong 10 giây).*
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? *(Có: Tương tác qua Zalo/Telegram Bot quen thuộc, chi phí chỉ 200.000–300.000 VNĐ/tháng).*

### Quyết định cuối cùng:
* [x] **🟢 GO (CHÍNH THỨC TRIỂN KHAI PILOT):** Bắt đầu phát triển với scope hẹp tại Vườn Cà Chua Nhà Màng $0,1\text{ ha}$.
* [ ] **NOT YET**
* [ ] **NO-GO**

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> Bài toán có điểm nghẽn rõ ràng, dữ liệu giáo trình và ảnh mẫu sẵn có, kiến trúc Serverless Telegram Bot + Vision-RAG API cực kỳ tiết kiệm ($200.000 - 300.000\text{ VNĐ/tháng}$) giúp bảo vệ $25 - 35\text{ triệu VNĐ/vụ}$ và mang lại **ROI dương ngay ở quy mô $0,1\text{ ha}$**, đồng thời tạo ra sản phẩm mẫu (MVP) hoàn chỉnh trong 7 ngày tới (Ngày 1–2: Data & Baseline 50 ảnh + 3 tài liệu; Ngày 3–5: Build MVP Bot + Vision-RAG; Ngày 6–7: Gate Eval 50 ca đối chiếu với Kỹ sư nông nghiệp đạt $\ge 85\%$ đồng thuận và phản hồi $< 10\text{s}$).

---

# 📝 Phase 6 — REFLECTION (Cá nhân)
*Chi tiết nhật ký phản tư cá nhân được ghi nhận tại file [`03-ai-log.md`](03-ai-log.md).*
