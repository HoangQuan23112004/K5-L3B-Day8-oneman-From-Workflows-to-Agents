# Decision record

- Shape chọn: workflow
- Commit hash: 098fa7f3673112465f32262f1bfe12764600d24e (nhánh main).
- Run id dẫn chứng: R-workflow-C-001-s11 (src/reports/benchmark.json).
- Pass rate / pass^5 / seed min-max / variance: 1.000000 / 1.000000 / 1.000000–1.000000 / 0.000000000.
- Cost per success (mô phỏng, VND): 27.857143.
- Crash safety: 20/20 (src/reports/crash.json); test_shape_safety.py kiểm thêm cả 3 cửa sổ crash trên từng shape mới.
- Secret leak / approval bypass: không ghi nhận lỗi trên public evaluator và test an toàn; chưa có kết quả hidden, không khẳng định cho dữ liệu chưa chạy.

## Vì sao shape này

So sánh cùng 14 claim, seed 11,22,33,44,55, MockLLM, bảng giá và host. Workflow,
single, orchestrator đều qua 70/70 trial, còn swarm qua 69/70. Workflow không gọi
model và có chi phí thấp nhất. Với nhiệm vụ chủ yếu đọc order → quote → host ghi
refund theo luật cố định, thêm tự chủ chưa đem lại lợi ích đo được trên public.

| Shape | Pass | Pass^5 | Seed min–max | Variance | Cost/success (VND) | Steps mean / p95 |
| --- | ---: | ---: | --- | ---: | ---: | --- |
| workflow | 1.000000 | 1.000000 | 1.000000–1.000000 | 0.000000000 | 27.8571 | 4.4286 / 5 |
| single | 1.000000 | 1.000000 | 1.000000–1.000000 | 0.000000000 | 47.9057 | 9.6429 / 11 |
| orchestrator | 1.000000 | 1.000000 | 1.000000–1.000000 | 0.000000000 | 57.5271 | 5.3571 / 6 |
| swarm | 0.985714 | 0.928571 | 0.928571–1.000000 | 0.000816327 | 49.5670 | 2.8000 / 3 |

## Có đáng đổi lấy multi-agent không

Giữ workflow trong phạm vi lab. Các shape khác dùng số tool/model call và cách
ghi event khác nhau, nên bảng trên là so sánh cả triển khai; không thể quy mọi
chênh lệch chỉ cho topology. Ablation trong research_card chỉ thay một biến:
thêm draft call, pass không đổi và cost tăng 1.014286 VND/success.
Xem xét lại nếu task mới cần suy luận không thể biểu diễn bằng luật, có các nhánh
điều tra độc lập, hoặc khi đánh giá mù trên dữ liệu mới cho thấy cải thiện đủ bù chi phí.

## Trace và bằng chứng

Workflow ghi found, order view qua allowlist, quote, kết quả host và decision.
Không ghi ticket, principal_id, parent_secret, raw headers, SĐT, địa chỉ, tên hàng
hay internal note. Single chỉ ghi tên action và kết quả các tool đọc đã giới hạn
field. Orchestrator ghi handoff từ make_handoff với facts rỗng, tool read-only,
order view và số lượng policy. Không tóm tắt parent context bằng LLM để giả làm redaction.

Mỗi nhánh dùng finish để lấy ledger/audit thật và ghi trace. finally ghi trace và
đóng DB kể cả khi InjectedCrash lan ra ngoài. Quote đầy đủ được chuyển cho host;
host sở hữu approval, cap theo đơn và idempotency refund:claim_id. Các artifact
runtime nằm trong traces/ và bị gitignore; không tạo debug dump ở nơi khác.

## Giới hạn

MockLLM không phải model sản xuất; decide của single là adapter theo trạng thái
vì MockLLM chỉ trả text, không sinh tool call có cấu trúc. Latency là unavailable.
Steps là số event trace và không phải số vòng ReAct hay thời gian thực thi;
khác độ chi tiết logging làm so sánh steps giữa shape có giới hạn.
Benchmark public chạy DB mới cho mỗi trial; retry/crash là các test riêng.
Chưa nhận student_drop nên chưa thể kết luận đạt ngưỡng hidden.
Các số là dữ liệu sinh từ công cụ, không chỉnh JSON. Commit triển khai: 098fa7f.
