# Team

## Thông tin nhóm

- Tên nhóm:
- Lớp: L3A
- Repo nhóm: `K5-L3A-DAY08-TenNhom-FromWorkflowsToAgents` (xem [SUBMISSION.md](SUBMISSION.md))
- URL repo, nhánh nộp, commit chốt:

## Thành viên

| STT | Họ tên | MSSV | Email | GitHub | Vai trò |
| --: | ------ | ---- | ----- | ------ | ------- |
|   1 | Lê Xuân Hạ | 2A202603571 |       |        | Ledger + evaluation |
|   2 |        |      |       |        |         |

Gợi ý chia vai:

- Nhóm 6 người: integration; workflow; single agent; orchestrator; ledger/policy/crash; eval/report.
- Nhóm 4 người: workflow + integration; single; orchestrator + handoff; ledger + evaluation.

## Bảng đóng góp

`python -m lab8.check_submission` đọc bảng này bằng máy: giữ nguyên tiêu đề `## Bảng đóng góp` và các cột. Mỗi người một dòng: commit có nội dung và ít nhất một test đã thêm hoặc sửa.

| Người | Vai | Commit | Test đã thêm hoặc sửa |
| --- | --- | --- | --- |
| Chu Thân Nhất (2A202603649) | Shape single | `bb013891c8919658136be99e2a8b2dc8850d641d` | `tests/test_single_shape.py`: `test_single_uses_order_row_for_quote`, `test_single_early_returns_have_trace`, `test_single_propagates_crash_and_writes_trace` |
| Lê Xuân Hạ (2A202603571) | Ledger + evaluation | *(điền hash sau khi commit)* | `src/tests/test_ledger_crash_and_gate.py` (16 test mới: crash point, cổng approval, trần, scope) |

## Cá nhân

Mỗi thành viên đồng thời tạo một báo cáo riêng trong `report/` theo dạng:

```text
report/team/MSSV_HoTen.md
```

Ví dụ: `report/team/20241234_NguyenVanAn.md`.

Sao chép mục dưới đây cho từng thành viên.

## 2A202603571_LeXuanHa

### Phần việc phụ trách

- Ledger + evaluation: kiểm chứng `durability.issue_refund`, crash suite (CP4), benchmark và phân tích metric (CP5), rà `TODO CP3` ở ba shape.

### Đóng góp cụ thể

- Thêm `src/tests/test_ledger_crash_and_gate.py` (16 test, chạy trong cổng host).
- Chạy crash suite `20/20`, benchmark mốc swarm, phân tích lỗi swarm T02 seed 55.
- Điền mục 4.1–4.2 `group_report.md`, dòng `Crash safety` trong `decision_adr.md`.

### Bằng chứng

- `src/reports/crash.json`, `src/reports/benchmark_swarm.json`, run id `R-swarm-C-002-s55`.
- Báo cáo chi tiết: `report/team/2A202603571_LeXuanHa.md`.
- Commit: *(điền hash sau khi commit)*

## MSSV_HoTen

### Phần việc phụ trách

-

### Đóng góp cụ thể

-

### Bằng chứng

-
