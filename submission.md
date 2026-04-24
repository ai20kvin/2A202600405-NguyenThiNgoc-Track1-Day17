# Day 17 Submission
**Student:** Nguyễn Thị Ngọc - 2A202600405
**Date:** 24 Apr
**Product idea:** TrustLayer AI đánh giá chatbot nội bộ


# 1. MVP Boundary

## In Scope (Chỉ giữ phần cần để test core hypothesis)

Sau khi chatbot RAG trả lời xong, hệ thống tự động đánh giá độ tin cậy của câu trả lời và phân loại:

- Safe  
- Warning  
- High Risk  
- Human Review Required  

Các câu trả lời rủi ro cao sẽ được:

- log vào báo cáo có cấu trúc  
- gắn lý do rủi ro (hallucination / retrieval lỗi / thiếu nguồn)  
- chuyển Human Review cho SME / Compliance  
- sau khi con người sửa → cập nhật vào knowledge base để cải thiện hệ thống

## MVP tập trung vào

### 1. Hallucination Detection

Phát hiện câu trả lời không grounded với tài liệu nguồn.

### 2. Retrieval Quality Check

Kiểm tra retrieved chunks có đủ liên quan để hỗ trợ câu trả lời hay không.

### 3. Trust Score cho từng response

Chấm điểm mức độ đáng tin cậy thay vì chỉ pass/fail.

### 4. Root-Cause Analysis cơ bản

Xác định lỗi chủ yếu đến từ:

- retrieval sai  
- source outdated  
- thiếu source  
- prompt dẫn sai logic

### 5. Dashboard đơn giản

Hiển thị các case High Risk để team ưu tiên xử lý.


## Out-of-Scope

- Evaluate nhiều chatbot/AI products cùng lúc  
- Multi-level approval workflow  
- Audit Trail cấp enterprise  
- Compliance reporting nâng cao  
- Executive dashboard phức tạp  
- Auto self-healing prompt optimization


## Non-Goals

- Không tối ưu prompt để chatbot trả lời hay hơn  
- Không cạnh tranh với LangSmith, RAGAS hay vector DB  
- Không xây chatbot mới  
- Không thay thế hệ thống RAG hiện tại  


# 2. PRD Skeleton (đã refine)

## Problem

Doanh nghiệp không biết chatbot RAG nội bộ đang trả lời đúng hay sai sau khi go-live.

Việc chỉ test trước release không đủ bao phủ toàn bộ câu hỏi thực tế, dẫn đến:

- hallucination  
- lỗi nghiệp vụ  
- compliance risk  
- mất niềm tin vào AI


## Target User

### Primary User

- Head of AI  
- CIO Office  
- AI Ops Team  
- Product Owner của hệ thống RAG nội bộ

### Secondary User

- Compliance Team  
- SME review team  
- Nhân viên nghiệp vụ sử dụng chatbot


## User Story #1

As a Head of AI,  
I want to biết câu trả lời nào của chatbot có rủi ro cao trước khi gây incident,  
so that tôi có thể giảm lỗi nghiệp vụ và kiểm soát compliance risk.

## User Story #2

As an AI Ops Engineer,  
I want to biết lỗi nằm ở retrieval, prompt hay dữ liệu nguồn,  
so that tôi có thể sửa đúng root-cause thay vì debug thủ công nhiều ngày.


## AI-Specific

## Model Selection

Sử dụng:

- LLM-as-a-Judge (GPT / Claude)

kết hợp với:

- Rule-based checks

vì cần đánh giá:

- groundedness  
- hallucination  
- trust score  
- explainability

Rule-based đơn thuần không đủ chính xác.


## Data Source

Dữ liệu đầu vào gồm:

- câu hỏi người dùng  
- retrieved chunks từ RAG  
- final answer của chatbot  
- metadata của source  
  (version, owner, effective date)  
- knowledge base nội bộ  
- feedback từ Human Review


# 3. Fallback UX 

## Trigger

Fallback xảy ra khi:

- trust score dưới ngưỡng an toàn  
- confidence thấp  
- thiếu supporting evidence  
- phát hiện hallucination risk cao


## User sees

Hệ thống hiển thị:

⚠ Warning: This answer may not be reliable

Lý do:

- thiếu nguồn dẫn chứng  
- retrieval chưa đủ liên quan  
- thông tin có thể outdated


## User options

Người dùng có thể:

- yêu cầu Human Review  
- xem source documents  
- escalate cho SME / Compliance  
- đánh dấu feedback “incorrect answer”


## Recovery path

Sau Human Review:

- câu trả lời đúng được cập nhật vào knowledge base  
- issue được log thành failure pattern  
- hệ thống cải thiện trust scoring cho lần sau

→ tránh lặp lại cùng một lỗi


# 4. Hypothesis & PMF Scorecard (đã strengthen)

## Riskiest Assumption

Doanh nghiệp sẵn sàng trả tiền cho một lớp Trust Layer riêng biệt thay vì xem đây chỉ là một tính năng nhỏ của hệ thống RAG hiện tại.

Nếu giả định này sai → sản phẩm rất khó bán.


## Core Hypothesis

Chúng tôi tin rằng:

TrustLayer AI sẽ giúp doanh nghiệp giảm hậu quả nghiệp vụ gây ra bởi các câu trả lời sai từ RAG / Enterprise Search

bằng cách:

- phát hiện hallucination sớm  
- giảm incident thật ngoài production  
- tăng niềm tin để scale AI


## Aha Moment

Khách hàng thật sự nhận ra giá trị khi:

TrustLayer AI phát hiện một câu trả lời sai mà team nội bộ chưa phát hiện trước đó

và giúp họ tránh được một incident gây hậu quả nghiệp vụ thực tế.

Đây là thời điểm khách hàng cảm thấy:

“Chi phí mua sản phẩm nhỏ hơn rất nhiều so với chi phí xử lý hậu quả.”


## PMF Signal

Với 10 khách hàng enterprise pilot:

Nếu ít nhất 30% chủ động tiếp tục sử dụng và đưa vào quy trình vận hành chính thức

→ đây là tín hiệu PMF tích cực.


## Sean Ellis Test

“Nếu ngày mai TrustLayer AI không còn tồn tại, bạn sẽ thất vọng ở mức nào?”

Tín hiệu tốt:

Khách hàng cảm thấy không còn đủ an tâm để vận hành AI nội bộ nếu thiếu TrustLayer AI.


## Retention

Sau pilot:

Khách hàng có tiếp tục dùng không?

Tín hiệu tốt:

≥ 30% khách hàng chủ động renew hoặc mở rộng scope sử dụng.

---

## Aha Metric


## Metric mới (actionable)

### Prevented Incident Rate

Số lượng incident nghiệp vụ được ngăn chặn trước khi xảy ra

Công thức:

Detected High-Risk Answers  
→ Confirmed as True Risk  
→ Fixed before business impact

Đây là metric phản ánh trực tiếp core value của sản phẩm.
