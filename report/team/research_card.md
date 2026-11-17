# Research card

## Phiếu của nhóm

| Field | Hướng dẫn | Nhóm điền |
| --- | --- | --- |
| CÂU | R1 / R2 / R3 / R4 | R4 — evaluation confounders; ablation bước draft. |
| SOURCE | Nguồn, năm, URL, loại bằng chứng | [docs/bonus_topics.md, B4](../../docs/bonus_topics.md), tài liệu lab 2026, nguồn thứ cấp nội bộ; [docs/metrics.md](../../docs/metrics.md). Không xác minh hoặc khẳng định số liệu của paper gốc được nhắc trong đề. |
| CLAIM | Kết luận trong phạm vi nguồn | Theo hướng dẫn B4, so sánh hiệu quả cần baseline đơn giản và ablation với model, tool, seed, corpus, giá, evaluator giữ cố định. |
| BOUNDARY | Giới hạn kết luận | Thử nghiệm này chỉ có LogiX-VN public 14 claim, MockLLM, 5 seed và giá mô phỏng; không suy rộng sang model thật. |
| HYPOTHESIS | Dự đoán đo được | Thêm đúng một draft call vào workflow giữ pass/pass^5, tool calls và refund không đổi, tăng cost/success trên 0 VND; không dự đoán cải thiện chất lượng ngôn ngữ. |
| METHOD | Biến cố định / biến thay đổi | Giữ chung _run_claim và toàn bộ đường xử lý; biến draft=False→True là thay đổi hành vi duy nhất. Tên shape/run_id riêng để cô lập artifact. Cùng MockLLM, tools, seeds 11,22,33,44,55, public corpus, pricing và evaluator; DB mới mỗi trial. |
| FAILURE / METRIC | Lỗi dự kiến và phép đo | Dự kiến không đổi business outcome; đo pass rate, pass^5, variance, cost/success, model/tool calls. test_draft_ablation_preserves_business_result đối chiếu effect, proposal và status. Không dùng pass để đo chất lượng câu trả lời. |

## Kết quả ablation

Lệnh đã chạy từ src/:

```powershell
.\.venv\Scripts\python.exe -m lab8.benchmark --shapes workflow,workflow_draft --extra-shape workflow_draft=bonus.workflow_draft:run_claim --cases data/public_claims.jsonl --seeds 11,22,33,44,55 --out reports/research_ablation.json
```

- File report: src/reports/research_ablation.json.
- Run id: R-workflow-C-001-s11 và R-workflow_draft-C-001-s11.
- Workflow: pass=1.000000, pass^5=1.000000, variance=0.000000000, cost/success=27.857143 VND.
- Workflow + draft: pass=1.000000, pass^5=1.000000, variance=0.000000000, cost/success=28.871429 VND.
- Chênh lệch cost/success: +1.014286 VND; model calls trung bình 0→1; tool calls trung bình giữ 2.785714.
- Kết quả ủng hộ hypothesis trong phạm vi public: draft tăng chi phí và không thay đổi tính đúng nghiệp vụ do evaluator không chấm câu chữ.
- Giới hạn: đây không phải ablation topology, không chứng minh draft vô ích cho trải nghiệm khách hàng. MockLLM không phải model sản xuất, latency unavailable. Chưa chạy hidden.
