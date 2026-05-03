# Day 17 Submission
**Student:** Nguyễn Thị Ngọc - 2A202600405
**Date:** 24 Apr
**Product idea:** TrustLayer AI - Lớp đánh giá độ tin cậy cho chatbot nội bộ doanh nghiệp

---

## 1. MVP Boundary Sheet

**Riskiest Assumption:**
> Doanh nghiệp sẽ trả tiền cho một lớp "Trust Layer" riêng biệt thay vì xem đây là tính năng phụ của hệ thống RAG hiện tại.

**In-Scope** (tối đa 3):
- [ ] **Automated Trust Score Calculation** — test giả định: AI có thể đánh giá chính xác độ tin cậy của response RAG
- [ ] **Risk Classification Dashboard** — test giả định: Manager cần xem trực quan các response rủi ro cao để hành động
- [ ] **Human Review Workflow** — test giả định: Doanh nghiệp sẵn sàng quy trình con người xác nhận các response AI đánh giá rủi ro

**Out-of-Scope:**
- Multi-chatbot evaluation — lý do bỏ: Cần validation cho single chatbot trước
- Executive dashboard — lý do bỏ: Manager dashboard là đủ cho MVP
- Compliance reporting — lý do bỏ: Cần product-market fit trước
- Multi-level approval — lý do bỏ: Single-level review đủ cho MVP

**Non-Goals:**
- Không tối ưu prompt để chatbot trả lời hay hơn
- Không thay thế hệ thống RAG hiện tại
- Không cạnh tranh với LangSmith, RAGAS

---

## 2. PRD Skeleton

### Problem Statement
> Head of AI tại doanh nghiệp lớn đang mất 15-20% niềm tin vào chatbot RAG nội bộ vì không biết response nào có hallucination — mỗi lần sai gây ra lỗi nghiệp vụ và tốn chi phí sửa chữa.

### Target User
> Head of AI / CIO Office tại các doanh nghiệp lớn (>500 nhân viên) đang vận hành hệ thống RAG/Enterprise Search nội bộ.

### User Stories
**Story 1:**
> As a Head of AI, I want to see all high-risk responses in real-time dashboard, so that I can prevent business operational errors before they impact customers.

**Story 2:**
> As an AI Ops Engineer, I want to know exactly whether errors come from retrieval, prompt, or data source, so that I can fix root cause within 30 minutes instead of spending hours debugging.

### AI-Specific

**Model Selection:**
- Model: GPT-4o mini
- Lý do chọn: Cân bằng giữa accuracy (đánh giá groundedness) và cost cho enterprise scale, latency <2s phù hợp real-time evaluation
- Trade-offs chấp nhận: Accuracy 90-95% thay vì 98% của GPT-4o full
- Trade-offs không chấp nhận: Latency >5s, cost >$0.01 per evaluation

**Data Requirements:**
- Nguồn: RAG system logs, user queries, retrieved chunks, chatbot responses, knowledge base metadata
- Owner: AI Ops Team
- Update frequency: Real-time for evaluation, daily for knowledge base updates
- Quality control: Automated validation + weekly human audit

**Fallback UX:**
- Chiến lược: Human-in-the-loop
- Trigger: Confidence score <80% hoặc khi response chứa conflicting information
- Hành động: Hiển thị "AI Uncertain - Requires Review" banner, disable auto-approval, route to human reviewer queue
- User options: Review & Edit, Escalate to SME, or Accept with Risk Acknowledgment

### Success Metrics
- Primary metric: % high-risk responses caught before user impact
- Ngưỡng thành công: >85% high-risk responses flagged within 5 seconds
- Timeframe đo lường: 30 ngày sau pilot

### Dependencies & Constraints
- API integration: OpenAI API, RAG system webhook
- Timeline constraint: MVP trong 8 tuần
- Budget constraint: < $5000/month for LLM costs
- Legal constraint: Must comply with enterprise data privacy policies

---

## 3. Hypothesis Table

### Hypothesis 1 (cho Automated Trust Score)
> "Chúng tôi tin rằng automated trust score sẽ giúp Head of AI đạt được việc ngăn chặn 85% operational errors. Chúng tôi sẽ biết mình đúng khi thấy % high-risk responses caught before user impact đạt 85% trong 30 ngày."

Riskiest assumption: LLM-as-judge có thể đánh giá chính xác groundedness của domain-specific content
Cách test cheapest: Wizard of Oz MVP với 5 người đánh giá thủ công 100 responses

### Hypothesis 2 (cho Risk Classification Dashboard)
> "Chúng tôi tin rằng real-time risk dashboard sẽ giúp Manager đạt được việc giảm 50% time-to-detect issues. Chúng tôi sẽ biết mình đúng khi thấy average time from high-risk response to human review giảm từ 4 giờ xuống 2 giờ trong 2 tuần."

Riskiest assumption: Manager thực sự monitor dashboard real-time
Cách test cheapest: Landing page MVP với mock dashboard và email alerts

---

## 4. PMF Scorecard

**Aha Moment:**
> Head of AI lần đầu thấy dashboard flag một response high-risk, click vào xem root cause là retrieval error, fix data source, và prevent 10 users khỏi getting wrong answer.

**Actionable Metric:**
> % of enterprises that reduce operational incidents by >20% within 60 days of implementation

**PMF Method:**
> Sean Ellis Test — ngưỡng: >40% "Very disappointed"
> Retention Curve — ngưỡng: D30 retention > 30% for enterprise customers
> Aha Moment tracking — ngưỡng: 60% customers achieve first prevented error within 14 days

**Vanity Metrics tôi sẽ không dùng:**
- Total dashboard views
- Number of evaluations performed
- Sign-up count
- API call volume

---

## 5. AI Critique Log

**Điểm AI chỉ ra:**
1. **Scope creep** — In-Scope có 6 tính năng, cần cắt xuống 3 — Action: Accept — Lý do: Handbook yêu cầu tối đa 3 tính năng để focus
2. **Vague fallback UX** — Chỉ ghi "hiển thị Warning" — Action: Accept — Lý do: Cần trigger và action cụ thể theo template
3. **Missing actionable metrics** — Dùng vanity metrics — Action: Accept — Lý do: Cần metrics gắn với business outcomes
4. **Weak hypothesis format** — Chưa theo công thức chuẩn — Action: Accept — Lý do: Handbook yêu cầu công thức cụ thể

**Thay đổi lớn nhất giữa Version A và Version B:**
> Cắt MVP từ 6 tính năng xuống 3 tính năng focus, thêm 3 thành phần AI-specific vào PRD, viết lại hypothesis theo công thức chuẩn, và định nghĩa actionable metrics thay vì vanity metrics.

---

## 6. Self-assessment

Mắt xích nào trong [MVP Boundary → PRD → Hypothesis → PMF] bạn đang yếu nhất?
> PMF Scorecard — vẫn còn khó định nghĩa actionable metrics thực sự cho enterprise product

Open questions bạn muốn giải đáp tiếp:
1. Làm sao để đo "prevented operational errors" khi customer không report errors được prevented?
2. Threshold cho retention curve có thực sự 30% cho enterprise products hay cần khác?
3. Wizard of Oz MVP có thực sự feasible cho LLM-as-judge evaluation?
