# 2. Document Selection — Nhóm (10 điểm)

### Domain & Lý Do Chọn

**Domain:** 
> Customer Support FAQ (Trung tâm trợ giúp Shopee)

**Tại sao nhóm chọn domain này?**
> Đây là domain có cấu trúc dữ liệu phân cấp rõ ràng (cặp Câu hỏi - Câu trả lời), rất lý tưởng để kiểm chứng hiệu quả của các phương pháp chunking trong việc bảo toàn ngữ cảnh. Nội dung tài liệu mang tính thực tế cao, giúp nhóm dễ dàng đánh giá độ chính xác của hệ thống RAG thông qua các tình huống hỗ trợ khách hàng phổ biến.

### Data Inventory

| # | Tên tài liệu | Nguồn | Số ký tự | Metadata đã gán |
|---|--------------|-------|----------|-----------------|
| 1 |[Cảnh báo lừa đảo] Mua sắm an toàn cùng Shopee|https://help.shopee.vn/portal/4 |~2,100|topic:security, type:warning|
| 2 |[Dịch vụ] Cách liên hệ Chăm sóc khách hàng Shopee |https://help.shopee.vn/portal/4 |~1,200|topic: support, type: contact|
| 3 |Quy trình Trả hàng và Hoàn tiền Shopee |https://help.shopee.vn/portal/4 |~3,800|topic: returns, type: process|
| 4 |Các phương thức Thanh toán khả dụng |https://help.shopee.vn/portal/4 |~2,500|topic: payment, type: info|
| 5 |Thời gian Đơn hàng & Vận chuyển dự kiến |https://help.shopee.vn/portal/4 |~1,800|topic: shipping, type: policy|

### Metadata Schema

| Trường metadata | Kiểu | Ví dụ giá trị | Tại sao hữu ích cho retrieval? |
|----------------|------|---------------|-------------------------------|
|topic |String |security, returns |Giúp hệ thống lọc đúng nhóm kiến thức, tránh nhầm lẫn giữa quy trình trả hàng và quy trình thanh toán.|
|type |String |warning, process |Hỗ trợ mô hình xác định phong thái trả lời (ví dụ: warning thì cần trả lời nghiêm túc, cảnh báo cao) |
|source_url |String |help.shopee.vn/s/article/... |Cung cấp trích dẫn (citation) để người dùng có thể kiểm chứng thông tin gốc sau khi nhận câu trả lời từ AI.|
|last_updated |Date |2026-01-20 |Giúp hệ thống ưu tiên lấy dữ liệu mới nhất nếu có sự thay đổi về chính sách vận chuyển hoặc thanh toán. |
---

## 3. Chunking Strategy — Cá nhân chọn, nhóm so sánh (15 điểm)

### Baseline Analysis

Chạy `ChunkingStrategyComparator().compare()` trên 2-3 tài liệu:

| Tài liệu | Strategy | Chunk Count | Avg Length | Preserves Context? |
|-----------|----------|-------------|------------|-------------------|
| | FixedSizeChunker (`fixed_size`) | | | |
| | SentenceChunker (`by_sentences`) | | | |
| | RecursiveChunker (`recursive`) | | | |

### Strategy Của Tôi

**Loại:** [FixedSizeChunker / SentenceChunker / RecursiveChunker / custom strategy]

**Mô tả cách hoạt động:**
> *Viết 3-4 câu: strategy chunk thế nào? Dựa trên dấu hiệu gì?*

**Tại sao tôi chọn strategy này cho domain nhóm?**
> *Viết 2-3 câu: domain có pattern gì mà strategy khai thác?*

**Code snippet (nếu custom):**
```python
# Paste implementation here
```

### So Sánh: Strategy của tôi vs Baseline

| Tài liệu | Strategy | Chunk Count | Avg Length | Retrieval Quality? |
|-----------|----------|-------------|------------|--------------------|
| | best baseline | | | |
| | **của tôi** | | | |

### So Sánh Với Thành Viên Khác

| Thành viên | Strategy | Retrieval Score (/10) | Điểm mạnh | Điểm yếu |
|-----------|----------|----------------------|-----------|----------|
| Tôi | | | | |
| [Tên] | | | | |
| [Tên] | | | | |

**Strategy nào tốt nhất cho domain này? Tại sao?**
> *Viết 2-3 câu:*

---

## 4. My Approach — Cá nhân (10 điểm)

Giải thích cách tiếp cận của bạn khi implement các phần chính trong package `src`.

### Chunking Functions

**`SentenceChunker.chunk`** — approach:
> *Viết 2-3 câu: dùng regex gì để detect sentence? Xử lý edge case nào?*

**`RecursiveChunker.chunk` / `_split`** — approach:
> *Viết 2-3 câu: algorithm hoạt động thế nào? Base case là gì?*

### EmbeddingStore

**`add_documents` + `search`** — approach:
> *Viết 2-3 câu: lưu trữ thế nào? Tính similarity ra sao?*

**`search_with_filter` + `delete_document`** — approach:
> *Viết 2-3 câu: filter trước hay sau? Delete bằng cách nào?*

### KnowledgeBaseAgent

**`answer`** — approach:
> *Viết 2-3 câu: prompt structure? Cách inject context? viết chi tiết answer*

### Test Results

```
# Paste output of: pytest tests/ -v
```

**Số tests pass:** __ / __

---

## 5. Similarity Predictions — Cá nhân (5 điểm)

| Pair | Sentence A | Sentence B | Dự đoán | Actual Score | Đúng? |
|------|-----------|-----------|---------|--------------|-------|
| 1 | How do I return a product ? | What is the shop's return policy? | high | 0.5501 | Có |
| 2 | Python is used for machine learning | Python helps build AI systems. | high | 0.6855 | Có |
| 3 | Reset your password from the settings page | Neural networks have many layers | low | 0.0526 | Có |
| 4 | Chunking affects retrieval quality | Small chunks can lose context in retrieval | high | 0.7702 | Có |
| 5 | Billing issues should be escalated when documentation is missing | Brown bears live in northern forests | low | -0.0018 | Có |

**Kết quả nào bất ngờ nhất? Điều này nói gì về cách embeddings biểu diễn nghĩa?**
> Kết quả mình thấy bất ngờ nhất là cặp 4 có score cao hơn cả cặp 1, dù hai câu ở cặp 1 nhìn rất giống nhau với người đọc. Điều này cho thấy embedding không chỉ dựa vào từ khóa bề mặt mà còn phản ánh mức độ gần nhau về ngữ nghĩa và ngữ cảnh sử dụng. Đồng thời, score không cần phải gần 1.0 mới được xem là tương đồng mạnh, vì embedding biểu diễn nghĩa theo không gian liên tục chứ không theo luật khớp từ tuyệt đối.

---

## 6. Results — Cá nhân (10 điểm)

Chạy 5 benchmark queries của nhóm trên implementation cá nhân của bạn trong package `src`. **5 queries phải trùng với các thành viên cùng nhóm.**

### Benchmark Queries & Gold Answers (nhóm thống nhất)

| # | Query | Gold Answer |
|---|-------|-------------|
| 1 | What is a vector store used for ? | A vector store stores embeddings and retrieves the most similar items for semantic search and RAG.|
| 2 | Why does chunking matter for retrieval quality ? | Chunking matters because chunks that are too small lose context and chunks that are too large dilute relevance.|
| 3 | How should a RAG system respond when retrieval is insufficient ? | It should say the context is insufficient or escalate instead of inventing an answer.|
| 4 | Why is metadata filtering useful in support systems ?| Filtering reduces noise and prevents surfacing the wrong internal content to the wrong audience. |
| 5 | How is Python used in AI and RAG applications ?| Python is used to clean data, train models, and connect embedding models, vector stores, and application logic.|

### Kết Quả Của Tôi

| # | Query | Top-1 Retrieved Chunk (tóm tắt) | Score | Relevant? | Agent Answer (tóm tắt) |
|---|-------|--------------------------------|-------|-----------|------------------------|
| 1 | What is a vector store used for ? |`vector_store_notes.md` giải thích vector store là nơi lưu embeddings và truy xuất các mục gần nhất để phục vụ semantic search, recommendation và RAG. | 0.6625 | Có | Vector store được dùng để lưu vector embeddings và tìm các nội dung tương tự nhất phục vụ semantic retrieval và RAG.|
| 2 | Why does chunking matter for retrieval quality ? | `chunking_experiment_report.md` nêu rằng chunk quá nhỏ sẽ mất ngữ cảnh, còn chunk quá lớn sẽ chứa quá nhiều ý và làm giảm độ liên quan. | 0.6442 | Có | Chunking ảnh hưởng trực tiếp đến retrieval vì nó quyết định mức độ đầy đủ ngữ cảnh và độ chính xác của kết quả truy xuất.|
| 3 | How should a RAG system respond when retrieval is insufficient ? | `rag_system_design.md` nói rằng nếu kết quả retrieval yếu hoặc mâu thuẫn thì hệ thống nên nói rõ điều đó thay vì giả vờ câu trả lời là đầy đủ. | 0.3519 | Có | Hệ thống nên thừa nhận thiếu ngữ cảnh hoặc đề xuất escalation, không nên tự bịa ra câu trả lời.|
| 4 | Why is metadata filtering useful in support systems ? | `vector_store_notes.md` mô tả metadata filtering giúp giảm noise và tránh trả về tài liệu sai nhóm người dùng; nội dung này cũng phù hợp với `customer_support_playbook.txt` | 0.4745 | Có | Metadata filtering giúp giới hạn phạm vi tìm kiếm đúng nguồn tài liệu, tránh lộ quy trình nội bộ hoặc trả lời sai đối tượng.|
| 5 | How is Python used in AI and RAG applications ? | `python_intro.txt` mô tả Python được dùng để xử lý dữ liệu, huấn luyện mô hình, chạy evaluation scripts, và kết nối embedding models với vector stores trong hệ thống RAG. | 0.6674 | Có | Python là ngôn ngữ phổ biến để xây dựng pipeline AI và tích hợp các thành phần như embeddings, vector store và application logic.|

**Bao nhiêu queries trả về chunk relevant trong top-3?** 5 / 5

---

## 7. What I Learned (5 điểm — Demo)

**Điều hay nhất tôi học được từ thành viên khác trong nhóm:**
> *Viết 2-3 câu:*

**Điều hay nhất tôi học được từ nhóm khác (qua demo):**
> *Viết 2-3 câu:*

**Nếu làm lại, tôi sẽ thay đổi gì trong data strategy?**
> *Viết 2-3 câu:*

---

## Tự Đánh Giá

| Tiêu chí | Loại | Điểm tự đánh giá |
|----------|------|-------------------|
| Warm-up | Cá nhân | / 5 |
| Document selection | Nhóm | / 10 |
| Chunking strategy | Nhóm | / 15 |
| My approach | Cá nhân | / 10 |
| Similarity predictions | Cá nhân | / 5 |
| Results | Cá nhân | / 10 |
| Core implementation (tests) | Cá nhân | / 30 |
| Demo | Nhóm | / 5 |
| **Tổng** | | **/ 100** |
