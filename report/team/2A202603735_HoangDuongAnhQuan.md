# Báo cáo cá nhân — 2A202603735_HoangDuongAnhQuan

## 1. Thông tin thành viên

- Họ tên: Hoàng Dương Anh Quân
- MSSV: 2A202603735
- Email: 26ai.quanhda@vinuni.edu.vn
- GitHub: HoangQuan23112004
- Hình thức: làm bài một mình (nhóm Oneman, một thành viên, do xin nghỉ về khám NVQS).

## 2. Phần việc phụ trách

Em phụ trách toàn bộ dự án: workflow, single ReAct, orchestrator/investigator,
kiểm thử an toàn, benchmark, ablation và toàn bộ báo cáo.

## 3. Đóng góp cụ thể

Em triển khai ba shape dùng cùng bộ tool đọc, compute_compensation và host issue_refund.
Workflow không gọi model. Single dùng run_react tối đa 8 bước, chỉ có tool đọc.
Orchestrator chuyển handoff allowlist cho investigator. Cả ba truyền quote đầy đủ và
crash_at đến host, đóng connection trong finally và ghi trace ở cả nhánh trả sớm.

## 4. Bằng chứng

- `src/tests/test_shape_safety.py`: kiểm cả ba cửa sổ crash cho từng shape, trace
  cho claim không tồn tại, ticket injection, giới hạn ReAct và tính bất biến của ablation.
- `src/reports/benchmark.json`: 4 shape × 14 claim × 5 seed.
- `src/reports/crash.json`: suite 20 kịch bản do công cụ lab sinh.
- `src/reports/research_ablation.json`: workflow so với workflow thêm một draft call.
- `report/team/decision_adr.md`: quyết định kiến trúc và giới hạn kết luận.

## 5. Công cụ AI đã dùng

Em dùng Codex làm trợ lý để giải thích yêu cầu, gợi ý code, gợi ý test và hỗ trợ
trình bày báo cáo, trong phạm vi [RULES.md](../../RULES.md) cho phép. Em tự kiểm tra
kết quả bằng cách chạy các lệnh trong README (smoke, pytest, benchmark, crash test,
ablation), đọc trace và đối chiếu số liệu trong báo cáo với các file JSON do công cụ
của lab sinh ra. Số liệu benchmark, run id và phần tự đánh giá không do AI tạo ra.

## 6. Tự đánh giá

Em thực hiện bài một mình. Về kết quả sản phẩm, bài đã có đủ workflow, single ReAct
và orchestrator, kèm benchmark, kiểm thử crash/retry và ablation để đối chiếu các
cách tiếp cận. Em đánh giá điểm mạnh của bài là có bằng chứng kiểm thử và nêu rõ
giới hạn của kết quả, đặc biệt là việc sử dụng MockLLM và tập claim công khai còn nhỏ.

Hạn chế của em là vẫn dùng trợ lý AI khi viết code, nên em cần tiếp tục củng cố
để tự trình bày được toàn bộ chương trình. Các phần em tập trung nắm vững là lý do
chọn workflow, ranh giới giữa model và code, lý do truyền quote đầy đủ vào host,
các nhánh xử lý lỗi và những dữ liệu cố ý không ghi vào trace.
