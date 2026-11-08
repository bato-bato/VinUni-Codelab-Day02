# 03 - Nhật Ký Tương Tác AI (AI Log & Reflection)

1. **AI đã hỗ trợ gì:**
   - Gợi ý nhanh 5 bài toán vận hành của Vingroup  và chuẩn hóa số liệu ước tính cho 3 Quick Problem Cards .
   - Hỗ trợ viết cấu trúc code gọi thư viện `google-genai` và sinh các kịch bản tấn công ranh giới trong `prompt_prototype.py`.
2. **AI trả lời sai / gặp lỗi ở đâu:**
   - Đề xuất dùng Multi-Agent tự động gửi tin nhắn cho tài xế mà không qua bước người duyệt (tiềm ẩn rủi ro an toàn vận hành).
   - Bản prompt đầu tiên bị lừa khi gặp lệnh giả danh Giám đốc Điều vận, và model mặc định `gemini-2.5-flash` bị lỗi `404 NOT_FOUND`.
3. **Cách tôi kiểm chứng & sửa lại:**
   - Chuyển sang kiến trúc **LLM Feature + Human-in-the-loop (HITL)**, ép buộc gắn nhãn `[DRAFT_ONLY]` và trả về JSON `dispatch_mobile_charger` khi pin `< 5%`.
   - Bổ sung quy tắc chống Prompt Injection vào `SYSTEM_PROMPT`, sửa lỗi thụt lề Python và đổi model sang `gemini-3.5-flash-lite` để vượt qua 100% bài test.
