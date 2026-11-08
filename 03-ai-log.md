# Lab 02 — Deliverable 03: Nhật Ký Tương Tác AI (AI Log & Reflection)

- **Họ và tên:** Vũ Trọng Quang
- **MSSV:** 2A202603535
- **Đề tài:** Hệ thống AI Chẩn đoán Bệnh Lá & Trợ lý Pha Dinh Dưỡng Tưới Nhỏ Giọt qua Zalo/Telegram Bot (Vườn Cà Chua Nhà Màng Quy Mô $0,1\text{ ha}$ — $2.500$ gốc)

---

## 1. AI đã giúp gì trong quá trình làm bài?

Trong quá trình thực hiện bài Lab 02, tôi sử dụng AI (Gemini / ChatGPT) làm trợ lý đồng hành (thought-partner) ở 3 khâu chính:
* **Quét bài toán & Chuẩn hóa Problem Statement:** Hỗ trợ sắp xếp các điểm nghẽn vận hành tại vườn cà chua $0,1\text{ ha}$ theo 4 Lenses, hoàn thiện 3 thẻ Quick Problem Cards và tóm tắt bài toán vào bảng Problem Statement 6 trường (xác định rõ tổn thất 10%–15% sản lượng $\approx 25 - 35\text{ triệu VNĐ/vụ}$ do mất 2–3 giờ tra cứu sổ tay và nhầm lẫn giữa thiếu vi lượng với nấm/vi khuẩn).
* **Tư vấn kiến trúc tinh gọn:** Giúp đối chiếu ưu/nhược điểm giữa Rule-based, LLM Feature và Agent để chốt phương án **Workflow có State + Multimodal LLM Feature (có RAG)** qua Telegram Bot với chi phí chỉ $200.000 - 300.000\text{ VNĐ/tháng}$.
* **Viết code kiểm thử (`prompt_prototype.py`):** Hỗ trợ viết khung code gọi Gemini SDK và xây dựng 3 kịch bản kiểm thử ranh giới an toàn (`ADVERSARIAL_TESTS`).

---

## 2. AI trả lời sai / Hallucination ở đâu?

Trong quá trình tương tác và chạy thử, AI đã bộc lộ 3 điểm sai lệch mà tôi phải trực tiếp can thiệp:
1. **Đề xuất sai ranh giới an toàn (Tự động hóa quá mức):** Ở các gợi ý đầu tiên, AI đề xuất cho hệ thống tự động kích hoạt bơm định lượng phân A-B và thuốc BVTV ngay khi phân tích xong ảnh lá. Điều này cực kỳ nguy hiểm vì nếu cảm biến EC/pH lệch hoặc AI chẩn đoán nhầm, việc tự động bơm hóa chất vào bồn $1.000\text{ lít}$ sẽ làm cháy rễ hàng loạt $2.500$ gốc cà chua.
2. **Lan man ngoài phạm vi bài toán & Lẫn thông tin mẫu:** Khi tạo tài liệu từ khung worksheet của môn học, AI tự động ghép thêm các ví dụ mẫu ngoài lề (như xe điện, đô thị) và giữ nguyên văn phong làm việc nhóm thay vì tập trung duy nhất vào bài làm cá nhân cho dự án Vườn cà chua $0,1\text{ ha}$.
3. **Lỗi kỹ thuật khi gọi API (Model Deprecated & Timeout):** AI giữ nguyên model cũ `gemini-2.5-flash` trong starter code dẫn đến lỗi `404 NOT_FOUND`, và khi đổi sang model mới có chế độ suy luận dài thì thời gian chạy vượt quá giới hạn 30 giây của `autograder.py`.

---

## 3. Tôi đã sửa Prompt và thiết lập ranh giới ra sao để đạt kết quả chuẩn?

1. **Khóa chặt phạm vi & Văn phong cá nhân:** Tôi yêu cầu AI loại bỏ toàn bộ các ví dụ ngoài lề và các phần liên quan đến nhóm, chỉ tập trung 100% vào các nghiệp vụ thực tế trong vườn cà chua nhà màng $0,1\text{ ha}$.
2. **Thiết lập ranh giới cứng (Operational Boundary & HITL) trong `SYSTEM_PROMPT`:**
   * Bắt buộc mọi câu trả lời tư vấn bệnh lá và công thức pha phân A-B cho bồn $1.000\text{ lít}$ phải bắt đầu bằng nhãn **`[DRAFT_ONLY]`**.
   * **CẤM tuyệt đối** việc tự động kích hoạt bơm định lượng hóa chất; bắt buộc trích dẫn tài liệu VietGAP và yêu cầu Chủ vườn nhấn nút **"Xác nhận (Approve)"** trên Telegram trước khi tự pha chế.
   * Khi dung dịch cốt A-B báo dưới ngưỡng an toàn (**`< 5%`**), hệ thống phải lập tức chặn lệnh pha/bơm và phát cảnh báo yêu cầu kiểm tra thủ công.
3. **Tinh chỉnh cấu hình gọi Model:** Chuyển sang model tốc độ cao **`gemini-3.5-flash-lite`** với `temperature=0.0` và giới hạn độ dài phản hồi (2–4 dòng), giúp cả 3 kịch bản tấn công ranh giới đều được chặn đứng chính xác và vượt qua `autograder.py` với điểm tuyệt đối (`10.00 / 10.00`) chỉ trong ~4 giây.
