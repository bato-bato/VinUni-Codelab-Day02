# Lab 02 — Deliverable 02: Deep-Dive Report & Evaluation (Bài Làm Cá Nhân)
## KẾ HOẠCH ỨNG DỤNG AI: VƯỜN CÀ CHUA QUY MÔ 0,1 HA (1-PAGE ARTIFACT PACK - PHƯƠNG ÁN GO)

- **Họ và tên:** Vũ Trọng Quang
- **MSSV:** 2A202603535
- **Định vị chiến lược:** Biến vườn $0,1\text{ ha}$ ($1.000\text{ m}^2 \approx 2.500$ gốc cà chua nhà màng) thành **Mô hình Pilot chuẩn (Living Lab)** với kiến trúc AI tinh gọn (**Telegram Bot + Vision-RAG API**), vừa mang lại **ROI dương ngay trong vụ đầu**, vừa làm bàn đạp nhân rộng cho các hộ trồng lân cận.
- **Use Case đề xuất:** **Hệ thống AI Chẩn đoán Bệnh Lá & Trợ lý Pha Dinh Dưỡng Tưới Nhỏ Giọt qua Zalo/Telegram Bot.**

---

## 1. TẠI SAO QUY MÔ 0,1 HA HOÀN TOÀN ĐẠT TIÊU CHUẨN "GO"?

Theo khung **Gate Criteria (Trang 12)** và **Feasibility Check (Trang 35)** của **VinUniversity**, dự án hoàn toàn khả thi để **GO (Triển khai xây dựng)** khi định hình đúng bài toán:

1. **Biên lợi nhuận cao trên diện tích nhỏ:** Cà chua công nghệ cao (cà chua cherry/beef nhà màng) trên $1.000\text{ m}^2$ cho doanh thu từ **150 – 250 triệu VNĐ/vụ**. Chỉ cần cứu được **15% sản lượng** khỏi bệnh mốc sương/héo xanh và tối ưu độ ngọt (Brix), giá trị mang lại đã đạt **25 – 35 triệu VNĐ/vụ**.
2. **Kiến trúc Serverless siêu tiết kiệm:** Thay vì làm ứng dụng di động đắt đỏ, hệ thống sử dụng giao diện **Telegram/Zalo Bot** kết nối trực tiếp với **Vision LLM + RAG API**. Chi phí vận hành chỉ khoảng **200.000 – 300.000 VNĐ/tháng** (tiền API token), giúp **ROI dương ngay từ tháng đầu tiên**.

---

# 🏗️ Phase 3 — DEEP-DIVE (Cá nhân)

## 3.1. Current-State Workflow Mapping

Quy trình kiểm tra bệnh lá và điều chỉnh công thức dinh dưỡng A-B thủ công mỗi sáng của Chủ vườn ($2.500$ gốc cà chua nhà màng):

```text
┌────────────────────┐     🔄     ┌────────────────────┐     🔄     ┌────────────────────┐
│ Bước 1             │  Handoff   │ Bước 2             │  Handoff   │ Bước 3 🔴          │
│ Đi kiểm tra 2.500  │ ─────────→ │ Phát hiện lá xoăn/ │ ─────────→ │ Lật sổ tay giấy /  │
│ gốc & đo bút EC/pH │            │ đốm hoặc thời tiết │            │ tra cứu diễn đàn   │
│ thủ công           │            │ đổi nắng sang mưa  │            │ đoán bệnh & tìm    │
│                    │            │                    │            │ thuốc đặc trị      │
│ Ai: Chủ vườn       │            │ Ai: Chủ vườn       │            │ Ai: Chủ vườn       │
│ ⏱ 30 phút          │            │ ⏱ 5 phút           │            │ ⏱ 60 - 90 phút 🔴  │
│ In: Quan sát + Bút │            │ In: Triệu chứng lá │            │ In: Ảnh/Mô tả mắt  │
│ Out: Số đo EC/pH   │            │ Out: Ảnh chụp ĐT   │            │ Out: Phán đoán cảm │
└────────────────────┘            └────────────────────┘            │      tính          │
                                                                    └─────────┬──────────┘
                                                                              │ 🔄 Handoff
                                                                              ▼
                                  ┌────────────────────┐     🔄     ┌────────────────────┐
                                  │ Bước 5             │  Handoff   │ Bước 4 🔴          │
                                  │ Pha hóa chất thủ   │ ◄───────── │ Tính tay lại lượng │
                                  │ công vào bồn 1.000L│            │ phân A-B & ml thuốc│
                                  │ & mở van tưới/phun │            │ theo thời tiết cho │
                                  │                    │            │ bồn 1.000 lít      │
                                  │ Ai: Chủ vườn       │            │ Ai: Chủ vườn       │
                                  │ ⏱ 15 phút          │            │ ⏱ 30 - 60 phút 🔴  │
                                  │ In: Phân A-B/Thuốc │            │ In: Sổ tay + Máy   │
                                  │ Out: Tưới 2.500 gốc│            │ Out: Số gam/ml pha │
                                  └────────────────────┘            └────────────────────┘

🔴 = Bottleneck (Điểm nghẽn cổ chai)
🔄 = Handoff (Điểm chuyển giao dữ liệu thủ công giữa Quan sát thực địa -> Sổ tay -> Bồn pha)
⏱ Tổng thời gian vận hành hiện tại = 140 – 200 phút/ca (mất 2 – 3 giờ mỗi khi có ca bệnh hoặc đổi thời tiết).
```

*(Xem thêm sơ đồ trực quan hóa chi tiết tại file [`04-workflow-diagram.png`](04-workflow-diagram.png)).*

---

## 3.2. PROBLEM STATEMENT (Định nghĩa bài toán chuẩn Gate 1)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | **Chủ vườn kiêm người vận hành trực tiếp** (chăm sóc $2.500$ gốc cà chua nhà màng trên diện tích $0,1\text{ ha} = 1.000\text{ m}^2$ mỗi ngày). |
| **2. Current Workflow** | Mỗi sáng đi kiểm tra lá và đo bút EC/pH thủ công $\rightarrow$ Khi thấy lá xoăn/đốm hoặc thời tiết đổi nắng sang mưa $\rightarrow$ Lật sổ tay giấy hoặc hỏi diễn đàn trồng trọt để chỉnh công thức phân A-B và tìm thuốc đặc trị. |
| **3. Bottleneck (Điểm nghẽn)** | Chẩn đoán bằng mắt thường hay nhầm lẫn giữa **thiếu vi lượng** (Ca, Mg, Bo, Fe) và **nhiễm nấm/vi khuẩn** (mốc sương *Phytophthora infestans*, héo xanh *Ralstonia solanacearum*); tính toán lại lượng phân theo thời tiết chậm trễ mất **2 – 3 giờ**, làm lỡ **"thời điểm vàng"** cách ly dịch bệnh. |
| **4. Business Impact (Tổn thất hiện tại)** | Phát hiện và xử lý sai làm lây lan ổ bệnh, gây thiệt hại **10% – 15% sản lượng** ($\approx 1 - 1,5\text{ tấn}$ quả/vụ, tương đương mất **25 – 35 triệu VNĐ/vụ**) và tốn thêm **20% chi phí thuốc BVTV**. |
| **5. Success Metric (Chỉ số thành công để qua Gate)** | • **System Metric:** Độ chính xác chẩn đoán (**Top-3 Accuracy**) đạt $\ge 85\%$ trên bộ test 100 ảnh lá thực tế; Trích xuất đúng $100\%$ liều lượng phân/thuốc theo tài liệu gốc (**Groundedness = 100%**).<br>• **Business KPI:** Giảm thời gian ra quyết định xử lý từ **2 giờ xuống dưới 2 phút/ca**; giảm tỷ lệ cây chết/nhổ bỏ xuống **dưới 3%/vụ**. |
| **6. Operational Boundary (Ranh giới vận hành & HITL)** | • **Được phép:** Nhận ảnh chụp lá + chỉ số EC/pH qua Bot $\rightarrow$ Phân tích bệnh, trích dẫn phác đồ VietGAP và tính sẵn số gam phân/ml thuốc cần pha cho bồn $1.000\text{ lít}$ (luôn gắn thẻ `[DRAFT_ONLY]`).<br>• **Không được phép (CẤM):** **Tuyệt đối không tự động kích hoạt bơm định lượng hóa chất.** Chủ vườn phải nhấn nút **"Xác nhận (Approve)"** trên điện thoại và tự pha chế (**Human-in-the-Loop — HITL**). Nếu dung dịch cốt bồn pha/nồng độ cảnh báo vượt ngưỡng an toàn (hoặc mức dự trữ dung dịch chuẩn $< 5\%$), hệ thống phải chặn lệnh và yêu cầu kiểm tra/bổ sung thủ công ngay lập tức. |

---

## 3.3. LỰA CHỌN KIẾN TRÚC & FUTURE-STATE FLOW (Rule / LLM Feature / Agent?)

### 👉 Quyết định kiến trúc: **WORKFLOW CÓ STATE + MULTIMODAL LLM FEATURE (CÓ RAG)**
*(Đúng chuẩn Decision Shortcut — Trang 17)*

| Phương án kiến trúc | Phân tích mức độ phù hợp (Trade-offs) | Quyết định |
|---|---|---|
| **1. Rule-based / State Workflow** | Rất tốt để cảnh báo ngưỡng vật lý cố định (ví dụ: nhiệt độ/độ ẩm nhà màng vượt ngưỡng $> 85\%$ hoặc kiểm tra ngưỡng an toàn EC/pH, dung dịch cốt $< 5\%$), nhưng **không thể** phân tích triệu chứng hình ảnh vết bệnh trên lá cà chua hay tổng hợp ngữ cảnh ngôn ngữ tự nhiên. | ✅ Dùng cho **Tầng thu thập & điều khiển cơ bản** |
| **2. Multimodal LLM Feature (có RAG)** | Kết hợp **Vision LLM** (nhận diện triệu chứng đốm lá, xoăn lá, gân lá từ ảnh chụp) + **RAG** (truy xuất chính xác phác đồ VietGAP và bảng công thức phân A-B theo giai đoạn sinh trưởng). Chi phí cực rẻ ($200.000 - 300.000\text{ VNĐ/tháng}$), độ trễ thấp ($< 10\text{s}$), kiểm soát chặt chẽ đầu ra dạng JSON có trích dẫn. | ✅ **LỰA CHỌN CỐT LÕI** cho **Tầng thông minh** |
| **3. Autonomous Agentic Loop** | Không cần thiết và tiềm ẩn rủi ro cao nếu để Agent tự gọi tool điều khiển phần cứng (bơm định lượng phân/thuốc) khi cảm biến sai lệch hoặc LLM ảo giác, dễ gây ngộ độc dinh dưỡng/cháy rễ hàng loạt cho $2.500$ gốc cà chua. | ❌ **LOẠI BỎ** (Quá phức tạp & rủi ro sinh học) |

### Kiến trúc 2 tầng chi tiết:
1. **Tầng thu thập & điều khiển cơ bản (Rule/Workflow):** Cảm biến nhiệt độ/độ ẩm nhà màng gửi cảnh báo tự động qua Telegram khi vượt ngưỡng dễ phát sinh nấm bệnh (độ ẩm $> 85\%$).
2. **Tầng thông minh (Multimodal LLM + RAG):** Khi chủ vườn gửi ảnh lá bệnh + chỉ số EC/pH + câu hỏi vào Bot $\rightarrow$ Hệ thống gọi **Vision LLM** phân tích triệu chứng hình ảnh $\rightarrow$ **Truy xuất (RAG)** vào cơ sở dữ liệu giáo trình trồng cà chua chuẩn VietGAP $\rightarrow$ Trả về phác đồ điều trị có trích dẫn trang tài liệu rõ ràng và số gam phân A-B / ml thuốc cần pha cho bồn $1.000\text{ lít}$.

### Sơ đồ Quy trình Tương lai (Future-State Flow):

```text
┌────────────────────┐     ┌────────────────────┐     ┌────────────────────┐     ┌────────────────────┐
│ Bước 1             │     │ Bước 2 🔵 AI Step  │     │ Bước 3 🔵 AI Step  │     │ Bước 4 🟢 HITL     │
│ Cảm biến báo ẩm    │     │ Vision LLM phân    │     │ RAG truy xuất giáo │     │ Chủ vườn đối chiếu │
│ >85% (Rule) hoặc   │ ──→ │ tích triệu chứng   │ ──→ │ trình VietGAP &    │ ──→ │ ảnh mẫu + trang    │
│ Chủ vườn gửi ảnh   │     │ trên ảnh lá + đọc  │     │ tính gam phân A-B/ │     │ trích dẫn, bấm nút │
│ lá + EC/pH vào Bot │     │ chỉ số EC/pH       │     │ ml thuốc (bồn 1m³) │     │ "Approve" & tự pha │
│ ⏱ 30 giây          │     │ ⏱ 4 giây           │     │ ⏱ 4 giây           │     │ ⏱ 60 giây          │
└────────────────────┘     └────────────────────┘     └─────────┬──────────┘     └─────────┬──────────┘
                                                                │                          │
                                                                ▼                          ▼
                                                      ┌────────────────────┐     ┌────────────────────┐
                                                      │ ↩️ Fallback 1      │     │ Bước 5             │
                                                      │ Nếu API lỗi/độ tin │     │ Tự động ghi Log ca │
                                                      │ cậy <85%: Mở ngay  │     │ bệnh & công thức   │
                                                      │ bảng PDF gốc trong │     │ vào Google Sheets  │
                                                      │ 10s & hỏi Kỹ sư SME│     │ ⏱ Tự động (1 giây) │
                                                      └────────────────────┘     └────────────────────┘

🔵 = AI Step (Vision LLM + RAG Pipeline, gắn nhãn [DRAFT_ONLY], tổng thời gian phản hồi < 10 giây)
🟢 = Human Step / HITL (Chủ vườn đối chiếu ảnh mẫu VietGAP, bấm "Xác nhận (Approve)" và tự pha chế)
↩️ = Fallback (Chuyển về bảng PDF gốc trong 10 giây nếu API lỗi; Escalate cho Kỹ sư Nông nghiệp khi ca bệnh lạ)
⏱ Tổng thời gian ra quyết định mới = Dưới 2 phút/ca (Giảm > 98% thời gian chờ so với 2 - 3 giờ trước đây).
```

---

## 4. STAKEHOLDER & RACI-LITE (Ma trận trách nhiệm)

| Hoạt động | Chủ vườn (Biz / Operator) | Kỹ sư Nông nghiệp (Domain SME) | AI Builder (Dev / Maker) |
|---|:---:|:---:|:---:|
| **Define problem + metric** | **A / R** | **C** | **C** |
| **Chốt baseline + bộ Eval 100 ca** | **C** | **R** *(Gán nhãn chuẩn)* | **A / R** |
| **Xây dựng Bot + RAG Pipeline** | **I** | **C** | **A / R** |
| **Chốt HITL / Duyệt phác đồ** | **A / R** *(Người duyệt cuối)* | **C** *(Khi ca bệnh lạ)* | **I** |
| **Go-live & Rollback Owner** | **A** | **I** | **R** *(Tắt bot/về sổ tay)* |

*(Chú thích: **R** = Responsible, **A** = Accountable, **C** = Consulted, **I** = Informed).*

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE & BOUNDARY STRESS-TEST (Cá nhân)

Tôi đã xây dựng bản mẫu kỹ thuật tại [`starter-code/prompt_prototype.py`](starter-code/prompt_prototype.py) chạy trên **Google Gemini Flash** để kiểm chứng các ranh giới vận hành (**Operational Boundaries**) của Vườn Cà Chua $0,1\text{ ha}$:

1. **Quy tắc 1 (`[DRAFT_ONLY]` & Cấm tự động kích hoạt bơm — HITL):** Mọi phản hồi tư vấn phác đồ điều trị lá bệnh và tính số gam phân A-B / ml thuốc cho bồn $1.000\text{ lít}$ đều **bắt buộc** bắt đầu bằng thẻ `[DRAFT_ONLY]`, yêu cầu Chủ vườn nhấn **"Approve"** và tuyệt đối cấm tự động kích hoạt bơm định lượng hóa chất.
2. **Quy tắc 2 (Ngưỡng nguy hiểm `< 5%` dung dịch cốt bồn pha):** Khi mức dung dịch cốt phân A-B trong bồn báo dưới ngưỡng an toàn (`< 5%`), hệ thống **tuyệt đối từ chối** lệnh pha/bơm tự động và bắt buộc yêu cầu kiểm tra, châm thêm dung dịch thủ công tại chỗ.
3. **Kết quả Stress-Test với 3 Adversarial Inputs:** Cả 3 kịch bản tấn công (ép tự động bật bơm hóa chất khi dung dịch cốt `< 5%`, ép bỏ qua thẻ `[DRAFT_ONLY]` khi tính phân A-B theo thời tiết, và ép kê liều thuốc BVTV gấp 3 lần ngoài giáo trình VietGAP) đều bị chặn đứng (**100% Passed, 0 Failed**).

---

# 🏁 Phase 5 — EVALUATE: AI READINESS & GATE DECISION (Cá nhân)

## 5.1. AI Readiness & Feasibility Check (Đạt 5/5 tiêu chí YES — Trang 21 & 35)

* ☑️ **1. Value (Có giá trị thực?): YES** — Bảo vệ **25 – 35 triệu VNĐ** doanh thu mỗi vụ (cứu 10% – 15% sản lượng và giảm 20% chi phí thuốc BVTV) với chi phí vận hành chỉ vài trăm nghìn đồng ($200.000 - 300.000\text{ VNĐ/tháng}$).
* ☑️ **2. Baseline (Có mốc so sánh?): YES** — So sánh trực tiếp với tốc độ tra cứu sổ tay giấy (mất **30 – 120 phút**) và tỷ lệ đoán bệnh bằng mắt thường của chủ vườn.
* ☑️ **3. Eval (Đo lường lặp lại được?): YES** — Có sẵn bộ **100 ảnh lá cà chua thực tế** và **30 kịch bản pha phân** theo giai đoạn sinh trưởng để chạy test tự động (`Top-3 Accuracy >= 85%`, `Groundedness = 100%`).
* ☑️ **4. Tolerance (Chấp nhận sai số có HITL?): YES** — Hệ thống luôn hiển thị kèm **ảnh mẫu bệnh trong giáo trình** và **trang trích dẫn** để chủ vườn đối chiếu trước khi phun/tưới (**HITL**).
* ☑️ **5. Operations (Có Rollback & Logging?): YES** — Mọi lượt hỏi đáp đều lưu log tự động trên **Google Sheets**; nếu API lỗi, lập tức chuyển về tra cứu **bảng PDF gốc trong 10 giây**.

---

## 5.2. QUYẾT ĐỊNH TẠI GATE & NEXT EXPERIMENT

### Phán quyết tại Gate hiện tại:
* [x] **🟢 GO (CHÍNH THỨC TRIỂN KHAI PILOT):** Bắt đầu phát triển với scope hẹp tại Vườn Cà Chua Nhà Màng $0,1\text{ ha}$.
* [ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline)**
* [ ] **NO-GO (Không khả thi / Rule-based tốt hơn)**

### Lý do (Justification):
> Bài toán có điểm nghẽn rõ ràng, dữ liệu giáo trình và ảnh mẫu sẵn có, kiến trúc Bot + API cực kỳ tiết kiệm ($200.000 - 300.000\text{ VNĐ/tháng}$) giúp **ROI dương ngay ở quy mô $0,1\text{ ha}$**, đồng thời tạo ra sản phẩm mẫu (MVP) hoàn chỉnh.

### Next Experiment (Kế hoạch triển khai ngay trong 7 ngày tới):
* **Ngày 1 – 2 (Data & Baseline):** Thu thập **50 ảnh lá cà chua tại vườn** (cả lá khỏe, lá thiếu chất, lá bệnh) + số hóa **3 tài liệu kỹ thuật trồng cà chua nhà màng chuẩn** để lập bộ Eval.
* **Ngày 3 – 5 (Build MVP):** Kết nối **Telegram Bot** với **Gemini/GPT-4o Vision API** và kho tài liệu **RAG**; thiết lập cấu trúc trả lời bắt buộc có trích dẫn nguồn (**Structured Output** + thẻ `[DRAFT_ONLY]`).
* **Ngày 6 – 7 (Gate Eval):** Chạy thử nghiệm **50 ca trên Bot**, đối chiếu kết quả với **Kỹ sư nông nghiệp**. Nếu đạt $\ge 85\%$ đồng thuận và thời gian phản hồi $< 10\text{ giây}$ $\rightarrow$ **Chính thức Go-Live** áp dụng cho toàn vụ mùa.
