# Team

## Thông tin nhóm

- Tên nhóm: Oneman
- Lớp: L3B (theo tên repository; xác nhận quy ước lớp với giảng viên vì đề mẫu ghi L3A).
- Hình thức: một thành viên (làm bài một mình).
- Repo nhóm: `K5-L3B-Day8-oneman-From-Workflows-to-Agents`
- URL repo: https://github.com/HoangQuan23112004/K5-L3B-Day8-oneman-From-Workflows-to-Agents
- Nhánh nộp, commit chốt: chưa chốt; hash sẽ được cập nhật sau khi commit.
- Họ tên: Hoàng Dương Anh Quân
- MSSV: 2A202603735
- Email: 26ai.quanhda@vinuni.edu.vn
- GitHub: HoangQuan23112004

## Thành viên

| STT | Họ tên | MSSV | Email | GitHub | Vai trò |
| --: | ------ | ---- | ----- | ------ | ------- |
|   1 | Hoàng Dương Anh Quân | 2A202603735 | 26ai.quanhda@vinuni.edu.vn | HoangQuan23112004 (chủ repo) | Tích hợp, ba shape, kiểm thử và báo cáo |

Gợi ý chia vai:

- Nhóm 6 người: integration; workflow; single agent; orchestrator; ledger/policy/crash; eval/report.
- Nhóm 4 người: workflow + integration; single; orchestrator + handoff; ledger + evaluation.

## Bảng đóng góp

`python -m lab8.check_submission` đọc bảng này bằng máy: giữ nguyên tiêu đề `## Bảng đóng góp` và các cột. Mỗi người một dòng: commit có nội dung và ít nhất một test đã thêm hoặc sửa.

| Người | Vai | Commit | Test đã thêm hoặc sửa |
| --- | --- | --- | --- |
| Hoàng Dương Anh Quân | Phụ trách toàn bộ dự án: ba shape, kiểm thử, benchmark, ablation và báo cáo | Chưa commit; cập nhật hash sau khi commit | src/tests/test_shape_safety.py: crash/resume, unknown claim, injection, giới hạn ReAct và ablation |

## Cá nhân

Báo cáo cá nhân: [report/team/2A202603735_HoangDuongAnhQuan.md](report/team/2A202603735_HoangDuongAnhQuan.md).

Mỗi thành viên đồng thời tạo một báo cáo riêng trong `report/` theo dạng:

```text
report/team/MSSV_HoTen.md
```

Ví dụ: `report/team/20241234_NguyenVanAn.md`.

## 2A202603735_HoangDuongAnhQuan

### Phần việc phụ trách

- Toàn bộ dự án: workflow, single ReAct, orchestrator/investigator, kiểm thử an toàn, benchmark, ablation và báo cáo.

### Đóng góp cụ thể

- Triển khai `src/lab8/shapes/workflow.py`, `single.py`, `orchestrator.py`; thêm ablation `src/bonus/workflow_draft.py` và script `src/bonus/write_reports.py`.
- Thêm `src/tests/test_shape_safety.py`.

### Bằng chứng

- `src/reports/benchmark.json`, `src/reports/crash.json`, `src/reports/research_ablation.json`.
- [decision_adr.md](report/team/decision_adr.md), [research_card.md](report/team/research_card.md), [group_report.md](report/team/group_report.md).
