# Lab 02 — Deliverable 01: Problem Scan & Quick-Assess Cards (Bài Làm Cá Nhân)

- **Họ và tên:** Vũ Trọng Quang
- **MSSV:** 2A202603535
- **Dự án:** Hệ thống AIỨng Dụng Cho Vườn Cà Chua Nhà Màng Quy Mô 0,1 ha ($1.000\text{ m}^2 \approx 2.500$ gốc — Living Lab)

---

## 🌱 Bối cảnh thực tế (Context)

Dự án tập trung vào việc chuyển đổi vườn cà chua công nghệ cao quy mô **$0,1\text{ ha}$ ($1.000\text{ m}^2 \approx 2.500$ gốc cà chua cherry/beef nhà màng)** thành **Mô hình Pilot chuẩn (Living Lab)**. Doanh thu mỗi vụ đạt **150 – 250 triệu VNĐ**, nhưng toàn bộ quy trình kiểm tra sức khỏe cây trồng, chẩn đoán bệnh lá và tính toán pha chế dinh dưỡng tưới nhỏ giọt hiện đang phụ thuộc hoàn toàn vào kinh nghiệm thủ công của chủ vườn.

Tôi sử dụng **4 Lenses** (Lặp lại, Tốn thời gian, AI có thể tốt hơn, Pain từ người khác) để quét toàn diện các điểm nghẽn vận hành trong chính vườn cà chua $0,1\text{ ha}$ này, từ đó chọn ra bài toán mang lại **ROI dương ngay trong tháng đầu tiên** với kiến trúc Serverless tinh gọn.

---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội trong Vườn Cà Chua 0,1 ha (Cá nhân)

Quét các quy trình vận hành hằng ngày tại vườn cà chua nhà màng $0,1\text{ ha}$ ($2.500$ gốc) qua **4 Lenses**:

| # | Hạng mục trong Vườn Cà Chua 0,1 ha | Lens | Mô tả ngắn bài toán (Bottleneck & Tổn thất) |
|---|---|---|---|
| **1** | **Chẩn đoán bệnh lá & Pha dinh dưỡng tưới nhỏ giọt A-B** | **Tốn thời gian & AI có thể tốt hơn** | Mỗi sáng chủ vườn đi kiểm tra $2.500$ gốc và đo bút EC/pH thủ công; khi thấy lá xoăn/đốm hoặc thời tiết đổi nắng sang mưa phải lật sổ tay giấy/tra cứu mất **2 – 3 giờ** để tìm thuốc BVTV và tính lại công thức phân A-B cho bồn $1.000\text{ lít}$. Chẩn đoán mắt thường hay nhầm giữa thiếu vi lượng và nhiễm nấm/vi khuẩn, gây thiệt hại **10% – 15% sản lượng** ($\approx 1 - 1,5\text{ tấn}$ quả, mất **25 – 35 triệu VNĐ/vụ**) và tốn thêm **20% chi phí thuốc BVTV**. |
| **2** | **Giám sát vi khí hậu nhà màng & Cảnh báo nguy cơ nấm bệnh** | **Lặp lại** | Chủ vườn phải xem nhiệt kế/ẩm kế thủ công nhiều lần trong ngày; khi độ ẩm nhà màng vượt ngưỡng **$> 85\%$** (đặc biệt lúc giao mùa hoặc ban đêm) rất dễ bùng phát bệnh mốc sương và héo xanh nếu không kịp thời nhận cảnh báo để bật quạt thông gió đối lưu. |
| **3** | **Ghi chép sổ nhật ký canh tác VietGAP & Theo dõi ngày cách ly** | **Lặp lại & Tốn thời gian** | Hằng ngày sau khi pha phân A-B và phun thuốc BVTV, chủ vườn phải ghi chép tay vào sổ giấy (lượng phân, tên hoạt chất, ngày cách ly thu hoạch) để phục vụ truy xuất nguồn gốc VietGAP, tốn **3 – 4 giờ/tuần** tổng hợp và dễ quên mốc thời gian cách ly an toàn. |
| **4** | **Đánh giá độ ngọt (Brix) & Phân loại chất lượng quả thu hoạch** | **AI có thể tốt hơn** | Khi thu hoạch cà chua cherry/beef, việc phân loại quả theo màu sắc độ chín và ước lượng độ ngọt (Brix) bằng mắt thường thiếu đồng nhất, làm giảm tỷ lệ quả đạt chuẩn loại 1 cung cấp cho cửa hàng thực phẩm sạch. |
| **5** | **Đếm bẫy dính vàng phát hiện sớm bọ phấn trắng & ruồi đục lá** | **Pain từ người khác** | Người vận hành đi kiểm tra từng tấm bẫy dính màu vàng treo trong nhà màng $1.000\text{ m}^2$ rất mỏi mắt và dễ bỏ sót mật độ bọ phấn trắng tăng đột biến — tác nhân chính truyền bệnh virus xoăn vàng lá cà chua (*TYLCV*). |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Từ 5 điểm nghẽn tại vườn cà chua $0,1\text{ ha}$ nêu trên, tôi chọn **Top 3 bài toán sát sườn nhất** (#1 — Chẩn đoán bệnh lá & Pha dinh dưỡng A-B; #2 — Cảnh báo vi khí hậu độ ẩm $> 85\%$; #3 — Trợ lý ghi nhật ký VietGAP & kiểm soát ngày cách ly) để đánh giá nhanh:

## 🍅 Card #1 — Hệ thống AI Chẩn đoán Bệnh Lá & Trợ lý Pha Dinh Dưỡng Tưới Nhỏ Giọt (BÀI TOÁN CHỌN DEEP-DIVE)

```text
┌───────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1 (BÀI TOÁN CHỌN DEEP-DIVE)                           │
│                                                                           │
│ Bài toán (1 câu): Chủ vườn cà chua nhà màng 0,1 ha (2.500 gốc) mất 2-3 giờ│
│ mỗi khi phát hiện lá bất thường hoặc thời tiết đổi mùa để tra cứu sổ tay   │
│ chẩn đoán bệnh và tính toán lại công thức pha phân A-B cho bồn 1.000 lít. │
│                                                                           │
│ Quy mô / Mô hình: Vườn Cà Chua Nhà Màng 0,1 ha (1.000 m2 - Living Lab)    │
│                                                                           │
│ Ai đang đau (Actor)? Chủ vườn kiêm người vận hành trực tiếp (chăm sóc     │
│ 2.500 gốc cà chua mỗi ngày) & Kỹ sư nông nghiệp (Domain SME).             │
│                                                                           │
│ Workflow thủ công hiện tại (5 bước):                                      │
│   1. Đi kiểm tra 2.500 gốc & đo bút EC/pH thủ công (30p)                  │
│   ──> 2. Phát hiện lá xoăn/đốm hoặc nắng đổi sang mưa, chụp ảnh (5p)      │
│   ──> 3. Lật sổ tay giấy / tra cứu để đoán bệnh & tìm thuốc (60-90p) 🔴   │
│   ──> 4. Tính tay lượng phân A-B & ml thuốc BVTV cho bồn 1.000L (30-60p)🔴│
│   ──> 5. Pha chế thủ công vào bồn & mở van tưới nhỏ giọt (15p)            │
│                                                                           │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 120 - 180 phút/ca)         │
│   - Nhầm lẫn giữa thiếu vi lượng và nhiễm nấm/vi khuẩn.                   │
│   - Tính toán lại lượng phân theo thời tiết chậm trễ mất 2-3 giờ, làm lỡ  │
│     "thời điểm vàng" cách ly dịch bệnh.                                   │
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

## 🌡️ Card #2 — Hệ thống Cảnh báo Vi Khí Hậu Nhà Màng (Ngưỡng Độ Ẩm > 85%)

```text
┌───────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                     │
│                                                                           │
│ Bài toán (1 câu): Độ ẩm nhà màng 1.000 m2 vượt ngưỡng > 85% vào ban đêm   │
│ hoặc khi trời mưa khiến nấm mốc sương bùng phát nếu chủ vườn không kịp    │
│ phát hiện để bật quạt đối lưu và điều chỉnh chế độ tưới.                  │
│                                                                           │
│ Quy mô / Mô hình: Vườn Cà Chua Nhà Màng 0,1 ha (1.000 m2 - Living Lab)    │
│                                                                           │
│ Ai đang đau (Actor)? Chủ vườn kiêm người vận hành trực tiếp.              │
│                                                                           │
│ Workflow thủ công hiện tại (4 bước):                                      │
│   1. Đi xem đồng hồ nhiệt/ẩm kế treo giữa nhà màng (10p)                  │
│   ──> 2. Ghi nhận độ ẩm vượt 85% và đối chiếu bảng nguy cơ nấm bệnh (15p) │
│   ──> 3. Đi bật công tắc quạt thông gió đối lưu & giảm lượng tưới (10p)   │
│   ──> 4. Kiểm tra lại độ ẩm sau 1 giờ (10p)                               │
│                                                                           │
│ Bước nào tốn thời gian/lỗi nhất? Bước 1 & 2 (Không thể canh 24/24 giờ)    │
│ AI / Hệ thống hỗ trợ ở bước nào? Bước 1 & 2                               │
│   (Cảm biến nhiệt/ẩm tự động đẩy cảnh báo qua Telegram Bot ngay khi       │
│   độ ẩm > 85% để kích hoạt quy trình kiểm tra lá phòng bệnh).             │
│                                                                           │
│ Đo thành công bằng gì (Metric có số)?                                     │
│   - Cảnh báo tức thì 100% các đợt độ ẩm > 85% trong vòng < 30 giây.       │
│                                                                           │
│ Quick Architecture: [ ] No AI  [x] Rule / Workflow có State  [ ] LLM      │
└───────────────────────────────────────────────────────────────────────────┘
```

---

## 📓 Card #3 — Trợ lý Ghi Nhận Nhật Ký Canh Tác VietGAP & Kiểm Soát Ngày Cách Ly

```text
┌───────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                     │
│                                                                           │
│ Bài toán (1 câu): Chủ vườn mất 3-4 giờ mỗi tuần để chép lại lịch sử pha   │
│ phân A-B, chỉ số EC/pH và ngày cách ly thuốc BVTV từ sổ tay lên bảng biểu.│
│                                                                           │
│ Quy mô / Mô hình: Vườn Cà Chua Nhà Màng 0,1 ha (1.000 m2 - Living Lab)    │
│                                                                           │
│ Ai đang đau (Actor)? Chủ vườn & Kỹ sư nông nghiệp kiểm tra VietGAP.       │
│                                                                           │
│ Workflow thủ công hiện tại (4 bước):                                      │
│   1. Pha phân A-B / phun thuốc BVTV tại bồn 1.000 lít                     │
│   ──> 2. Ghi tạm vào sổ tay giấy tại nhà màng (5p/lần)                    │
│   ──> 3. Cuối tuần ngồi nhập lại vào bảng theo dõi VietGAP (180p/tuần) 🔴 │
│   ──> 4. Tự tính nhẩm ngày kết thúc cách ly thuốc trước khi hái quả (15p) │
│                                                                           │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 3 - 4 giờ/tuần)            │
│ Hệ thống hỗ trợ ở bước nào? Bước 2, 3 & 4                                 │
│   (Mọi lượt hỏi đáp và bấm "Approve" phác đồ trên Telegram Bot đều tự     │
│   động lưu log sang Google Sheets kèm cảnh báo ngày cách ly thu hoạch).   │
│                                                                           │
│ Đo thành công bằng gì (Metric có số)?                                     │
│   - Giảm thời gian ghi chép nhật ký từ 3 giờ/tuần ──> 0 phút (Auto-log).  │
│   - 100% các lần phun thuốc được tự động tính ngày cách ly an toàn.       │
│                                                                           │
│ Quick Architecture: [ ] No AI  [x] Rule / Auto-Logging  [x] LLM Feature   │
└───────────────────────────────────────────────────────────────────────────┘
```

---

# 🗳️ Quyết định lựa chọn bài toán để Deep-Dive (Cá nhân)

Tôi quyết định chọn **Card #1 — Hệ thống AI Chẩn đoán Bệnh Lá & Trợ lý Pha Dinh Dưỡng Tưới Nhỏ Giọt qua Zalo/Telegram Bot** (đồng thời tích hợp luôn tầng Rule cảnh báo độ ẩm $> 85\%$ của Card #2 và cơ chế Auto-Logging vào Google Sheets của Card #3) làm bài toán **Deep-Dive** vì:

1. **Giải quyết trực diện điểm nghẽn gây thiệt hại kinh tế lớn nhất:** Cứu **10% – 15% sản lượng** ($\approx 1 - 1,5\text{ tấn}$ quả/vụ, tương đương **25 – 35 triệu VNĐ/vụ**) khỏi bệnh mốc sương/héo xanh, giảm **20% chi phí thuốc BVTV** và tối ưu độ ngọt (Brix) trên quy mô $0,1\text{ ha}$ ($2.500$ gốc).
2. **Kiến trúc Serverless siêu tiết kiệm:** Sử dụng giao diện Zalo/Telegram Bot kết nối trực tiếp với **Vision LLM + RAG API**, chi phí vận hành chỉ **200.000 – 300.000 VNĐ/tháng** (tiền API token), giúp **ROI dương ngay từ tháng đầu tiên**.
3. **Đạt 5/5 tiêu chí AI Readiness & Feasibility Check:** Có sẵn baseline sổ tay giấy (mất 2–3 giờ), bộ Eval 100 ảnh lá thực tế + 30 kịch bản pha phân + 3 giáo trình kỹ thuật trồng cà chua nhà màng chuẩn, cơ chế **Human-in-the-Loop (HITL)** cấm tự động kích hoạt bơm hóa chất, và Rollback về bảng PDF gốc trong 10 giây.
