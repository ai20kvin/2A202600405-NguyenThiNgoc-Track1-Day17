# Day 17 Submission
**Student:** Nguyễn Thị Ngọc - 2A202600405
**Date:** 24 Apr
**Product idea:** TrustLayer AI đánh giá chatbot nội bộ



---

# 1. MVP của TrustLayer AI (làm liền)

## In Scope

Sau khi chatbot RAG trả lời xong, hệ thống tự động đánh giá độ tin cậy của câu trả lời và phân loại:

- Safe
- Warning
- High Risk
- Human Review Required

=> được log vào tài liệu báo cáo (có cấu trúc) cho những câu trả lời sai  
=> cập nhật vào knowledge base  
=> đảm bảo chatbot nội bộ lần sau có thể lấy đúng câu trả lời đã được sửa bởi con người

### MVP tập trung vào:

- Hallucination Detection
- Retrieval Quality Check
- Trust Score cho từng response
- Root-Cause Analysis cơ bản
- Dashboard đơn giản cho các câu trả lời rủi ro cao
- Xem xét thêm Value Proposition

## Out-of-Scope (khi có thời gian thì làm)

- Evaluate Multi-chatbot / AI products in the enterprise
- Multi-level approval workflow
- Audit Trail đầy đủ cấp enterprise
- Compliance reporting nâng cao
- Executive dashboard phức tạp

## Non-Goals

- Không tối ưu prompt để chatbot trả lời hay hơn
- Không cạnh tranh với LangSmith, RAGAS hay vector database
- Không xây chatbot mới
- Không thay thế hệ thống RAG hiện tại

---

# 2. PRD Skeleton (Cái gì và tại sao)

## Problem

Doanh nghiệp không biết chatbot RAG nội bộ (tất cả sản phẩm AI trong doanh nghiệp) đang trả lời đúng hay sai sau khi go-live, dẫn đến hallucination, lỗi nghiệp vụ và mất niềm tin vào AI.

## Target User

Leader/Manager (Head of AI / CIO Office / AI Ops Team), nhân viên nghiệp vụ tại doanh nghiệp lớn đang vận hành RAG / Enterprise Search nội bộ.

## User Story #1

**As a Head of AI,**  
I want to biết câu trả lời nào của chatbot có rủi ro cao  
so that tôi có thể kiểm soát trước khi gây ra lỗi nghiệp vụ.

## User Story #2

**As an AI Ops Engineer,**  
I want to biết lỗi nằm ở retrieval, prompt hay dữ liệu nguồn  
so that tôi có thể sửa nhanh và đúng nguyên nhân gốc.

## AI-Specific

### Model Selection

Chọn LLM-as-a-Judge (GPT / Claude) kết hợp rule-based checks vì cần đánh giá groundedness, hallucination và trust score chính xác hơn thay vì chỉ dùng rule cứng.

### Data Source

Dữ liệu lấy từ:

- câu hỏi người dùng
- retrieved chunks từ RAG
- câu trả lời cuối của chatbot
- metadata của source (version, owner, effective date)
- tài liệu nội bộ (knowledge base)

## Fallback UX

Khi AI phát hiện câu trả lời có rủi ro cao hoặc không đủ tự tin:

- hiển thị Warning
- yêu cầu Human Review
- chuyển escalation cho compliance / SME review

---

# 3. Hypothesis & PMF Scorecard

## Riskiest Assumption

- Doanh nghiệp sẽ sẵn sàng trả tiền cho một lớp “Trust Layer” riêng biệt thay vì xem đây chỉ là một tính năng phụ của hệ thống RAG hiện tại.
- Nếu giả định này sai → sản phẩm sẽ khó bán.

## Hypothesis

- TrustLayer AI sẽ giúp doanh nghiệp giảm được những hậu quả nghiệp vụ gây ra bởi câu trả lời sai của RAG / Enterprise Search.

## Aha Moment

- Thời điểm đầu tiên khách hàng nhận ra sản phẩm đã giúp họ tránh được một incident gây hậu quả nghiệp vụ thực tế.
- Khách hàng cảm thấy chi phí mua TrustLayer AI thấp hơn hậu quả nghiệp vụ hoặc số tiền bồi thường phải trả.

## PMF Signal

Với 10 khách hàng doanh nghiệp dùng thử, nếu ít nhất 30% muốn tiếp tục sử dụng vì sản phẩm giúp họ kiểm soát trực quan các sản phẩm AI nội bộ và giảm thiểu rủi ro nghiệp vụ thực tế, đây là tín hiệu tích cực.

### Sean Ellis

**Nếu ngày mai TrustLayer AI không còn tồn tại, bạn sẽ thất vọng ở mức nào?**

Kỳ vọng tín hiệu tốt: khách hàng doanh nghiệp cảm thấy thiếu an tâm vì không còn cách kiểm soát trực quan chất lượng và độ tin cậy của các hệ thống AI nội bộ.

### Retention

**Sau giai đoạn pilot, khách hàng có tiếp tục sử dụng không?**

Kỳ vọng tín hiệu tốt: với 10 khách hàng dùng thử, nếu ít nhất 30% chủ động tiếp tục sử dụng và đưa vào vận hành chính thức.

### Aha Metric

Khách hàng thật sự nhận ra giá trị khi:

- hệ thống phát hiện một câu trả lời sai / hallucination mà team nội bộ chưa nhận ra trước đó
- hoặc trust score giúp xác định chính xác khu vực hệ thống đang fail và xử lý nhanh hơn
