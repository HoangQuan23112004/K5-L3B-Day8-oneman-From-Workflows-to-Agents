# Báo cáo nhóm — Lab 8 LogiX-VN

## 1. Thông tin nhóm

- Nhóm: Oneman, một thành viên.
- Thành viên: Hoàng Dương Anh Quân, MSSV 2A202603735, email 26ai.quanhda@vinuni.edu.vn, GitHub HoangQuan23112004.
- Báo cáo cá nhân: [2A202603735_HoangDuongAnhQuan.md](2A202603735_HoangDuongAnhQuan.md).
- Commit triển khai: 098fa7f3673112465f32262f1bfe12764600d24e (nhánh main).

## 2. Mục tiêu và cách tiếp cận

Xử lý claim đúng chính sách và bảo toàn ledger khi retry/crash. Hoàn thiện workflow
trước, sau đó single ReAct và orchestrator, đối chiếu swarm tham chiếu. Giữ nguyên
host, thước đo, config, fixture và swarm. Không dùng API key hoặc model thật.

## 3. Giải pháp

Workflow đọc claim qua get_claim_view, kiểm order trong scope của principal, quote
từ order/policy hiện hành bằng compute_compensation, rồi gọi issue_refund. Không
trích order_id, số ngày trễ, điều khoản hay số tiền từ ticket.

Single dùng run_react(max_steps=8, worker=True). Adapter decide gọi MockLLM và
chọn tuần tự tool đọc theo observation: order, policy, lịch sử refund, quote. Vì
MockLLM không sinh function call, đây là mô phỏng ReAct bằng adapter trạng thái.
Sau vòng lặp, code lấy quote từ tool; model không được gọi tool ghi hoặc chọn tiền.

Orchestrator dựng make_handoff với facts rỗng và tool đọc, chạy investigator qua
run_readonly_investigators, nhận order view/quote rồi tự gọi host. Chỉ handoff đã
sanitize được ghi trace; parent context và raw ticket không được chuyển qua handoff.

Cả ba xử lý riêng claim không tồn tại, thiếu order, đơn ngoài scope, quote bằng 0,
host từ chối và amount conflict. Quote đầy đủ được đưa cho host để tránh trừ phần
đã hoàn hai lần. finish đọc ledger và audit thật thay vì điền số liệu giả. crash_at
được chuyển nguyên vẹn, InjectedCrash không bị nuốt. finally đóng connection và
ghi trace cả khi crash. Khóa refund:claim_id và transaction của host bảo vệ retry.

## 4. Kết quả

Nguồn: src/reports/benchmark.json. Bốn shape × 14 claim × 5 seed = 280 trial, cùng
MockLLM, seed 11,22,33,44,55, bảng giá mô phỏng, policy as_of 2026-11-15 và DB mới
cho từng trial.

| Shape | Pass | Pass^5 | Seed min–max | Variance | Cost/success (VND) | Steps mean / p95 |
| --- | ---: | ---: | --- | ---: | ---: | --- |
| workflow | 1.000000 | 1.000000 | 1.000000–1.000000 | 0.000000000 | 27.8571 | 4.4286 / 5 |
| single | 1.000000 | 1.000000 | 1.000000–1.000000 | 0.000000000 | 47.9057 | 9.6429 / 11 |
| orchestrator | 1.000000 | 1.000000 | 1.000000–1.000000 | 0.000000000 | 57.5271 | 5.3571 / 6 |
| swarm | 0.985714 | 0.928571 | 0.928571–1.000000 | 0.000816327 | 49.5670 | 2.8000 / 3 |

Swarm sai C-002 tại seed 55: tin số ngày trễ trong ticket thay cho order. Đây là
điểm yếu cố ý của bản tham chiếu; không sửa để giữ đối chứng. Ba shape mới đều
đạt 70/70. Workflow được chọn vì đạt cùng độ đúng với mức tự chủ và chi phí thấp.

Crash suite: 20/20 từ src/reports/crash.json. Suite này chủ yếu kiểm
host và swarm; test bổ sung kiểm cả 3 cửa sổ crash/resume cho từng shape mới.
Public evaluator cùng các test approval/injection không ghi nhận lỗi an toàn.
Chưa nhận student_drop hoặc hidden labels, nên chưa có kết quả hidden.

Ablation draft: pass giữ 1.0, cost/success tăng từ 27.857143
lên 28.871429 VND. Xem decision_adr.md và research_card.md.

## 5. Khó khăn và cách xử lý

Tên trace dựa trên shape/claim/seed nên cần chạy các lệnh kiểm chứng tuần tự để không ghi đè artifact của nhau. Steps đếm event, không đại diện latency hay số tool call. Chi phí chỉ là chi phí mô phỏng theo công thức của host hiện tại.

## 6. Tổng hợp đóng góp

Nhóm có một thành viên, Hoàng Dương Anh Quân, thực hiện toàn bộ: thiết kế, triển khai
ba shape, kiểm thử, benchmark, ablation và báo cáo. Bảng đóng góp nằm ở TEAM.md,
báo cáo cá nhân ở 2A202603735_HoangDuongAnhQuan.md.

## 7. Hạn chế và hướng cải thiện

MockLLM không phải model sản xuất; latency unavailable. Public chỉ có 14 claim.
Benchmark fresh DB không thay thế test retry/crash. Các shape có số lần đọc tool
và độ chi tiết trace khác nhau nên so sánh tổng thể không cô lập topology.
Ablation draft mới cô lập một thay đổi hành vi; evaluator không chấm câu trả lời.
Đánh giá lại quyết định khi có tập mù mới, model thật hoặc nhánh điều tra độc lập.

## 8. Tài liệu tham khảo

- docs/business_rules.md: nguồn dữ kiện và luật hoàn tiền.
- docs/contracts.md: RunResult, trace, nhánh status và ranh giới dữ liệu.
- docs/architecture.md, docs/metrics.md: kiến trúc và công thức đánh giá.
- docs/bonus_topics.md, B4: hướng dẫn baseline và ablation của lab (nguồn thứ cấp).
- src/lab8/shapes/swarm_reference.py: đối chứng do đề bài cung cấp.

Số liệu trong báo cáo được tạo lại bằng python -m bonus.write_reports sau khi
chạy các lệnh benchmark/crash trong README; không sửa tay JSON bằng chứng.
