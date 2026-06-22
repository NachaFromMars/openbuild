# OPENBUILD FROM MULA (OBFM) v1.0
# Project Operating System Blueprint

> Spec rõ → Làm nhanh → Kiểm chuẩn → Ship sạch → Học lại
> Copy là chạy. Không phụ thuộc Claude / GPT / Local.

**Version 1.0 | February 2026 | Reviewed by Quantum Momentum Council**

---

## 0. Luật Lõi — 10 Điều Không Thương Lượng

> NGUYÊN TẮC NỀN: Mọi quy tắc dưới đây là bất khả xâm phạm. Vi phạm bất kỳ điều nào = dừng lại, sửa trước khi tiếp tục.

1. Không làm khi chưa có Output (đầu ra) mô tả 1 câu.
2. Mọi claim "fact" phải có nguồn. Không có thì gắn nhãn [chưa kiểm chứng].
3. Mọi việc đều có Definition of Done (DoD) viết ra.
4. Mọi thay đổi lớn phải ghi Decision Log (ADR).
5. Một nơi là "Sự Thật" (single source of truth): repo/notion, không rải chat.
6. Chia nhỏ theo module, mỗi module có test/tiêu chí riêng.
7. Luôn có vòng Verify trước khi ship (tự kiểm + reviewer).
8. Không để context phình: dùng snapshot STATE sau mỗi mốc.
9. Ưu tiên quy trình "rẻ" (hooks/validators) trước khi tốn token AI.
10. Retro ngắn sau ship: học 3 điều, sửa template 1 điều.
11. **Mỗi step build/forge: 4000-6000 token, đơn lập, tập trung chất lượng. KHÔNG viết/build dài 1 lần.**
12. **Mỗi step xong → spawn thẩm định độc lập NGAY. PASS mới tiếp. FAIL → sửa + thẩm định lại. FAIL 2 lần → BLOCK.**

---

## 1. Cấu Trúc Dự Án Chuẩn (Bộ Xương)

```
openbuild/
├── 00_README.md          # Đọc 60 giây: dự án là gì, output, deadline
├── 01_SCOPE.md           # Trong/ngoài phạm vi, giả định, phụ thuộc
├── 02_PRD.md             # Yêu cầu: Problem, Users, Must/Should/Could
├── 03_ACCEPTANCE.md      # DoD + test cases (Given/When/Then)
├── 04_PLAN.md            # Milestones, timebox, rollback plan
├── 05_STATE.md           # Snapshot: Now/Done/Next/Blockers
├── 06_RISKS.md           # Risk list + mitigation + trigger
├── 07_DECISIONS/         # ADR-0001.md, ADR-0002.md, ...
├── 08_ASSETS/            # Tài nguyên dự án
├── 09_OUTPUT/            # Deliverables cuối
├── 10_TESTS/             # Test cases (dù không phải code)
├── 11_RETRO.md           # 3-3-1: tốt, dở, thay đổi
└── 12_CHANGELOG.md       # Track thay đổi template qua dự án
```

---

## 2. Template Nội Dung Cho Từng File

### 00_README.md (đọc 60 giây)
- Dự án là gì (1 câu)
- Output cuối (format: PDF / landing page / API / 10 post / album art)
- Deadline / ràng buộc
- Cách chạy: Plan → Build → Verify → Ship
- Link tới PRD + Acceptance + State

### 01_SCOPE.md
- Trong phạm vi (bullet)
- Ngoài phạm vi (bullet)
- Giả định (assumptions)
- Dependencies (phụ thuộc)

### 02_PRD.md (yêu cầu)
- Problem / Goal
- Users / Use cases
- Requirements (Must / Should / Could)
- Constraints (tech, tone, policy, budget)
- Non-goals
- Open questions

### 03_ACCEPTANCE.md (DoD + test case)
- Definition of Done (cứng)
- Checklist chất lượng (mềm)
- Test cases (Given / When / Then)
- Fail conditions (những lỗi là loại ngay)

### 04_PLAN.md
- Milestones (M1 / M2 / M3)
- Deliverables mỗi mốc
- Owner / role (người chịu trách nhiệm)
- Timebox mỗi mốc
- Rollback plan (nếu fail)

### 05_STATE.md (snapshot chống mục)
| Trường | Mô tả |
|--------|-------|
| Now | Đang ở đâu |
| Done | Đã xong gì |
| Next | Bước tiếp theo |
| Blockers | Kẹt gì |
| Decisions pending | Cần chốt gì |
| Quality status | Pass / Fail theo acceptance |

### 06_RISKS.md
| Trường | Mô tả |
|--------|-------|
| Risk list | Impact + Likelihood |
| Mitigation | Biện pháp giảm thiểu |
| Trigger | Dấu hiệu rủi ro sắp nổ |
| Owner | Ai chịu trách nhiệm mitigate |

### 07_DECISIONS/ADR-xxxx.md
- Context: Tại sao cần quyết định
- Decision: Chọn gì
- Alternatives considered: Các phương án khác
- Consequences: Hệ quả
- Date + Owner

### 11_RETRO.md (3-3-1)
- 3 điều làm tốt
- 3 điều làm dở
- 1 thay đổi áp dụng cho template (bắt buộc → ghi vào 12_CHANGELOG.md)

---

## 3. Quy Trình 6 Pha

Chuẩn hóa để không lạc. Mỗi pha xong phải cập nhật 05_STATE.md.

| Pha | Tên | Mô tả | Output |
|-----|-----|-------|--------|
| P0 | Intake | Gom input, tạo PRD nháp | 02_PRD.md (DRAFT) |
| P1 | Clarify | Chốt câu hỏi, đóng Scope + Acceptance | 01_SCOPE + 03_ACCEPTANCE |
| P2 | Design | Plan + rủi ro + quyết định nền | 04_PLAN + 06_RISKS + ADR |
| P3 | Build | Forge/build đơn lập 4-6K token/step, thẩm định sau mỗi step | 09_OUTPUT/ + 05_STATE |
| P4 | Verify | Reviewer + test cases + red-team phá | 10_TESTS/ results |
| P5 | Ship | Đóng gói + retro + update template | 11_RETRO + 12_CHANGELOG |

> QUY TẮC: Mỗi pha xong PHẢI cập nhật 05_STATE.md. Vi phạm = vi phạm Luật Lõi #8.

---

## 4. Bộ Vai Chuẩn

Dùng cho 1 người cũng được — "đổi mũ" theo từng vai.

| Vai | Trách nhiệm |
|-----|-------------|
| Lead / PM | Scope, trade-off, chốt DoD |
| Builder | Làm deliverable |
| Reviewer | Soi Acceptance, bắt lỗi |
| Researcher | Nguồn, fact-check |
| Red Team | Phá, tìm edge-case |
| Librarian | Gom asset, log quyết định, giữ repo sạch |

---

## 5. Hooks / Validators (Đồ Cơ Khí)

> Triết lý: Rẻ mà cứu mạng. Chạy trước mọi lần commit / ship.

| Hook | Kiểm tra | Fail Action |
|------|----------|-------------|
| H1: File check | README / PRD / ACCEPTANCE / STATE tồn tại | BLOCK: không cho build |
| H2: Format check | Output đúng độ dài, cấu trúc, naming | WARN + log |
| H3: Banned words | Không có từ cấm / claim cấm | BLOCK |
| H4: Source required | Số liệu / so sánh / thống kê phải có nguồn | WARN + gắn [chưa kiểm chứng] |
| H5: Regression | Lỗi từng gặp không tái xuất | BLOCK |
| H6: Step budget | Build step > 6000 token | BLOCK: bắt chia nhỏ |
| H7: Gate check | Step chưa qua thẩm định độc lập | BLOCK: không cho tiếp step sau |

> IMPLEMENTATION: Có thể implement bằng: shell script (pre-commit hook), CI check (GitHub Actions), hoặc manual checklist.

---

## 6. Bộ Lệnh Slash Chuẩn

| Lệnh | Chức năng |
|------|-----------|
| /intake | Tạo PRD nháp từ mô tả thô |
| /clarify | Liệt kê 5–12 câu hỏi chốt scope |
| /accept | Viết 03_ACCEPTANCE.md + test cases |
| /plan | Viết 04_PLAN.md timebox theo mốc |
| /state | Cập nhật snapshot hiện tại |
| /build | Tạo output theo module |
| /review | Soi theo acceptance + nêu fail cụ thể |
| /redteam | Phá bằng edge-case + abuse-case |
| /ship | Đóng gói deliverable + checklist cuối |
| /retro | Sinh retro 3-3-1 và update template |
| /compress | Nén toàn bộ dự án thành 30 dòng "đọc 60 giây" |
| /abort | Dừng dự án sạch: ghi lý do, archive state, retro mini |

---

## 7. System Prompt Chuẩn

```
# OPENBUILD FROM MULA — System Instructions

## Core Behavior
- Luôn hỏi/đòi 02_PRD.md và 03_ACCEPTANCE.md TRƯỚC khi build.
- Nếu thiếu, tạo bản nháp và gắn nhãn "DRAFT".
- Khi đưa ra thông tin thực tế mà không có nguồn: gắn [chưa kiểm chứng].
- Luôn kết thúc bằng: (1) bước tiếp theo, (2) những gì đang block.

## Slash Commands Available
/intake /clarify /accept /plan /state /build
/review /redteam /ship /retro /compress /abort

## Quality Gates
- Không build khi chưa có PRD + Acceptance.
- Mọi số liệu phải có nguồn hoặc gắn [chưa kiểm chứng].
- Mỗi pha xong = update STATE.
- Ship = phải qua Verify + Retro.
```

---

## 8. Nâng Cấp "Tàn Bạo"

| Nâng cấp | Vấn đề giải quyết | Cơ chế |
|----------|-------------------|--------|
| DoD + Test Case bắt buộc | "Làm tới khi thấy ổn" = không tiêu chuẩn | 03_ACCEPTANCE.md |
| ADR (Decision Log) | Loạn quyết định, quên lý do | 07_DECISIONS/ |
| STATE Snapshot | Context phình, mất phương hướng | 05_STATE.md mỗi pha |
| Red Team | Ảo tưởng thành phẩm | /redteam command |
| Retro + Template Update | Lặp lại lỗi cũ | 11_RETRO + 12_CHANGELOG |
| /abort | Zombie project không chết được | Clean shutdown protocol |
| Risk Owner | Risk list không ai nhận | Owner field in 06_RISKS |

---

## 9. Phán Quyết Hội Đồng

> ĐÁNH GIÁ TỔNG THỂ: OBFM là một hệ điều hành dự án nghiêm túc, thiết kế đúng cho người làm thật. Framework này không làm đẹp cho có — mỗi lớp đều giải quyết một điểm đau cụ thể. Với các bổ sung từ Hội Đồng (Risk Owner, /abort, CHANGELOG), OBFM đạt mức production-ready.

**Trạng thái: ✅ APPROVED** với 3 bổ sung đã tích hợp.

**Phiên bản:** 1.0
**Ngày phán quyết:** 27 tháng 2, 2026

---

*OpenBuild-FromMula — Copy là chạy.*
