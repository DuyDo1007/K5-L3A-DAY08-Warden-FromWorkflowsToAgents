# Báo cáo nhóm — Lab 8 LogiX-VN

Tối đa 5 trang ngoài phụ lục. Số liệu lấy từ file trong `src/reports/`, không chép tay từ terminal.

## 1. Thông tin nhóm

- Tên nhóm:
- Thành viên: xem `TEAM.md`
- Commit chốt (trùng `report/team/decision_adr.md`):

## 2. Mục tiêu và cách tiếp cận

Nhóm hiểu bài toán thế nào, chia việc ra sao, thứ tự làm bốn shape.

...

## 3. Giải pháp

Mô tả ba shape nhóm viết (workflow, single, orchestrator): mỗi shape đọc gì, quyết định gì, gọi host ra sao. Chỉ rõ ranh giới code / model (tham khảo [docs/architecture.md](../../docs/architecture.md)), cách handoff được sanitize và trace ghi những event nào.

...

## 4. Kết quả

Bảng so sánh 4 shape trên bộ public (từ `src/reports/benchmark.json`): pass rate, pass^5, seed min–max, variance, cost/success, mean/p95 steps. Kèm crash safety (`src/reports/crash.json`) và, nếu có, kết quả tự kiểm trên `student_drop/`.

### 4.1 Crash safety (`src/reports/crash.json`)

`python -m lab8.crash_test --count 20 --out reports/crash.json` → `crash_safety 20/20`. Gồm: 17 điểm crash của `issue_refund` (5 × `before_effect`, 7 × `after_effect_before_checkpoint`, 5 × `after_checkpoint`), retry khác amount (conflict, giữ số đã commit), retry khi thiếu approval (không ghi), và swarm crash sau effect rồi resume (một refund).

Test bổ sung của nhóm: `src/tests/test_ledger_crash_and_gate.py` (16 test, chạy trong cổng host): crash lặp lại 3 lần tại cùng một điểm, approval hết hạn / revoked / pending / 0 VND, trần theo approval và theo số tiền đã trả, claim lạ, đơn của principal khác, claim không có order.

### 4.2 Mốc đối chứng: swarm (`src/reports/benchmark_swarm.json`)

`python -m lab8.benchmark --shapes swarm --out reports/benchmark_swarm.json` (14 claim public × seed 11,22,33,44,55):

| Shape | Pass rate | pass^5 | Seed min–max | Variance | Cost/success (VND mô phỏng) | Mean / p95 steps |
| --- | --: | --: | --: | --: | --: | --: |
| swarm | 0.986 (69/70) | 0.929 | 0.929–1.000 | 0.000816 | 49.57 | 2.8 / 3.0 |

Trial fail duy nhất: `R-swarm-C-002-s55` (T02), `status resolved != denied`. Ticket ghi "giao trễ 5 ngày", order O-202 trong DB có `delay_days = 2`. Ở seed chia hết cho 5, swarm tin số trong ticket (event `model_override` trong trace), tính lại quote thành 20000 và host **ghi refund thật**, vì approval A-002 hợp lệ. Bài học: cổng host chỉ chặn approval, scope, trần và idempotency; nó không kiểm dữ kiện dùng để tính quote. Vì vậy shape phải lấy `delay_days` từ `lookup_order`, không lấy từ chữ trong ticket.

### 4.3 Bốn shape (`src/reports/benchmark.json`)

`python -m lab8.benchmark --shapes workflow,single,orchestrator,swarm --cases data/public_claims.jsonl --seeds 11,22,33,44,55 --out reports/benchmark.json` (14 claim public × 5 seed = 70 trial mỗi shape, cùng bảng giá mô phỏng):

| Shape | Pass rate | pass^5 | Seed min–max | Variance | Cost/success (VND mô phỏng) | Mean / p95 steps | Model calls TB | Tool calls TB |
| --- | --: | --: | --: | --: | --: | --: | --: | --: |
| workflow | 1.000 (70/70) | 1.000 | 1.000–1.000 | 0 | 27.86 | 4.43 / 5 | 0 | 2.79 |
| single | 1.000 (70/70) | 1.000 | 1.000–1.000 | 0 | 38.50 | 8.14 / 9 | 3.57 | 3.64 |
| orchestrator | 1.000 (70/70) | 1.000 | 1.000–1.000 | 0 | 55.71 | 3.43 / 4 | 0 | 5.57 |
| swarm | 0.986 (69/70) | 0.929 | 0.929–1.000 | 0.000816 | 49.57 | 2.8 / 3 | 3.0 | 4.66 |

`steps` là số event trong trace (`lab8/trace.py`), không phải số vòng ReAct; single vẫn chạy `run_react` với `max_steps=8`.

Nhận xét: ba shape nhóm viết đều pass 70/70 và không dao động giữa các seed. Trên bộ public, chênh lệch duy nhất giữa chúng là chi phí: workflow rẻ nhất (27.86), single đắt hơn 38%, orchestrator đắt gấp đôi (55.71) vì investigator gọi thêm tool. Swarm là shape duy nhất có variance, do điểm yếu ở mục 4.2.

Crash suite chạy lại sau khi gộp ba shape: vẫn `20/20`. Cổng host `python -m pytest -q`: 75 passed. Ma trận student `python -m pytest -q -m student -o addopts=`: 267 passed, 0 failed.

Quyết định kiến trúc và lý do: xem `report/team/decision_adr.md`. Phiếu nghiên cứu và ablation: xem `report/team/research_card.md`.

## 5. Khó khăn và cách xử lý

Lỗi gặp phải (kèm lệnh, output hoặc ảnh chụp) và cách nhóm xử lý.

...

## 6. Tổng hợp đóng góp

Chi tiết đóng góp của từng thành viên nằm trong `TEAM.md` (bảng đóng góp) và báo cáo cá nhân `report/team/MSSV_HoTen.md`.

## 7. Hạn chế và hướng cải thiện

MockLLM không phải model sản xuất; latency là `unavailable`. Nêu thêm giới hạn của nhóm và điều kiện làm kết luận thay đổi.

...

## 8. Tài liệu tham khảo

...
