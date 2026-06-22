# OpenBuild From Mula (OBFM) — OpenClaw Skill

## Khi nào dùng
- Bắt đầu dự án mới (code, reforge, web, phân tích ngược, content...)
- Cần quản lý scope, plan, risks cho dự án phức tạp
- Muốn quy trình rõ ràng: Spec → Build → Verify → Ship
- Slash commands: /intake, /clarify, /accept, /plan, /state, /build, /review, /redteam, /ship, /retro, /compress, /abort

### Handoff từ SuperBuild
Nếu đã chạy **SuperBuild RPI** trước đó và có `RESEARCH.md` + `PLAN.md`:
- `/intake` dùng RESEARCH.md làm nền cho 02_PRD.md (không hỏi lại từ đầu)
- `/plan` dùng PLAN.md làm nền cho 04_PLAN.md
- Agent design decisions → 07_DECISIONS/ADR-0001.md
- Risk assessment → 06_RISKS.md

```
SuperBuild (nghĩ)  ──RESEARCH.md + PLAN.md──►  OpenBuild (làm)
```

## Trigger keywords
- "tạo dự án", "new project", "openbuild", "khởi tạo", "/intake", "/clarify", "/plan", "/build", "/ship"
- Bất kỳ dự án nào cần structure: reforge app, build web, phân tích ngược, content campaign...

## 12 Luật Lõi (BẤT KHẢ XÂM PHẠM)
1. Không làm khi chưa có Output mô tả 1 câu
2. Mọi claim "fact" phải có nguồn. Không có → gắn [chưa kiểm chứng]
3. Mọi việc đều có Definition of Done (DoD) viết ra
4. Mọi thay đổi lớn phải ghi Decision Log (ADR)
5. Single source of truth: repo, không rải chat
6. Chia nhỏ theo module, mỗi module có test riêng
7. Luôn Verify trước khi ship
8. Dùng STATE snapshot sau mỗi mốc (chống context phình)
9. Ưu tiên quy trình "rẻ" trước khi tốn token AI
10. Retro ngắn sau ship: học 3 điều, sửa template 1 điều
11. **Mỗi step build/forge: 4000-6000 token, đơn lập, tập trung chất lượng. KHÔNG viết dài 1 lần.**
12. **Mỗi step xong → spawn thẩm định độc lập NGAY. PASS mới tiếp. FAIL → sửa + thẩm định lại.**

## Quy trình 6 Pha
| Pha | Tên | Output |
|-----|-----|--------|
| P0 | Intake | 02_PRD.md (DRAFT) |
| P1 | Clarify | 01_SCOPE + 03_ACCEPTANCE |
| P2 | Design | 04_PLAN + 06_RISKS + ADR |
| P3 | Build | 09_OUTPUT/ + 05_STATE |
| P4 | Verify | 10_TESTS/ results |
| P5 | Ship | 11_RETRO + 12_CHANGELOG |

Mỗi pha xong PHẢI cập nhật 05_STATE.md.

## Cấu trúc thư mục
Khi khởi tạo dự án mới, tạo folder `openbuild/` trong workspace dự án:
```
openbuild/
├── 00_README.md          # Đọc 60 giây
├── 01_SCOPE.md           # Trong/ngoài phạm vi
├── 02_PRD.md             # Requirements
├── 03_ACCEPTANCE.md      # DoD + test cases
├── 04_PLAN.md            # Milestones
├── 05_STATE.md           # Snapshot hiện tại
├── 06_RISKS.md           # Risks + owner
├── 07_DECISIONS/         # ADR files
├── 08_ASSETS/            
├── 09_OUTPUT/            
├── 10_TESTS/             
├── 11_RETRO.md           # 3-3-1
└── 12_CHANGELOG.md       
```

## Slash Commands

### /intake — Khởi tạo dự án
1. Hỏi user: "Dự án gì? Output cuối là gì? Deadline?"
2. Tạo folder `openbuild/` trong workspace dự án
3. Viết 00_README.md + 02_PRD.md (DRAFT)
4. Update 05_STATE.md: Now = P0 Intake

### /clarify — Chốt scope
1. Đọc 02_PRD.md
2. Liệt kê 5-12 câu hỏi chốt scope (KHÔNG hỏi lan man)
3. Sau khi user trả lời → viết 01_SCOPE.md + 03_ACCEPTANCE.md
4. Update STATE: Now = P1 Clarify done

### /accept — Viết acceptance
1. Viết 03_ACCEPTANCE.md: DoD + test cases (Given/When/Then) + fail conditions

### /plan — Lên kế hoạch
1. Viết 04_PLAN.md: milestones, timebox, owner, rollback
2. Viết 06_RISKS.md: risk + impact + mitigation + owner
3. Tạo ADR nếu có quyết định lớn
4. Update STATE: Now = P2 Design done

### /state — Cập nhật snapshot
1. Đọc toàn bộ openbuild/
2. Cập nhật 05_STATE.md với Now/Done/Next/Blockers

### /build — Bắt tay làm
1. Kiểm tra: PRD + Acceptance phải tồn tại (H1). Nếu thiếu → BLOCK
2. Đọc 04_PLAN.md → chia thành từng step nhỏ (mỗi step = 1 module/task cụ thể)
3. **Mỗi step thực hiện theo Quy Tắc Đơn Lập:**

```
┌─ STEP LOOP ──────────────────────────────────────┐
│                                                    │
│  ① FORGE/BUILD step (4000-6000 token)              │
│     - Đơn lập: 1 step = 1 việc duy nhất           │
│     - Tập trung: chất lượng > số lượng             │
│     - Giới hạn: KHÔNG viết/build quá 6000 token    │
│     - Output: lưu vào 09_OUTPUT/                   │
│                                                    │
│  ② THẨM ĐỊNH ĐỘC LẬP (ngay sau mỗi step)         │
│     - Spawn sub-agent thẩm định riêng biệt         │
│     - Thẩm định đọc: output step + 03_ACCEPTANCE   │
│     - Chấm: PASS / FAIL + lý do cụ thể            │
│     - PASS → tiếp step tiếp theo                   │
│     - FAIL → sửa step đó + thẩm định lại          │
│     - FAIL 2 lần liên tiếp → BLOCK, hỏi user      │
│                                                    │
│  ③ UPDATE STATE                                    │
│     - Cập nhật 05_STATE.md sau mỗi step PASS       │
│     - Ghi: step nào xong, điểm thẩm định           │
│                                                    │
│  ④ TIẾP step tiếp theo (quay lại ①)               │
│                                                    │
└──────────────────────────────────────────────────┘
```

4. **TUYỆT ĐỐI KHÔNG:**
   - Build cả module lớn 1 lần (chia nhỏ ra)
   - Bỏ qua thẩm định để "chạy cho nhanh"
   - Tự thẩm định chính mình (phải spawn agent riêng)
   - Viết quá 6000 token/step (chia tiếp nếu cần)

5. Khi tất cả steps PASS → chuyển sang /review

### /review — Kiểm tra
1. Đọc 03_ACCEPTANCE.md
2. Soi từng tiêu chí, nêu PASS/FAIL cụ thể
3. Ghi kết quả vào 10_TESTS/

### /redteam — Phá
1. Tìm edge-case, abuse-case
2. Test các fail conditions trong 03_ACCEPTANCE.md
3. Ghi findings vào 10_TESTS/

### /ship — Đóng gói
1. Chạy checklist: PRD ✓, Acceptance ✓, Tests ✓, STATE up-to-date ✓
2. Đóng gói deliverable
3. /retro tự động

### /retro — Học lại
1. Viết 11_RETRO.md: 3 tốt, 3 dở, 1 thay đổi template
2. Update 12_CHANGELOG.md

### /compress — Nén context
1. Nén toàn bộ openbuild/ thành 30 dòng "đọc 60 giây"
2. Dùng khi context phình

### /abort — Dừng sạch
1. Ghi lý do abort vào 05_STATE.md
2. Archive state hiện tại
3. Retro mini (1-1-1)

## Vai trò (đổi mũ khi cần)
| Vai | Trách nhiệm |
|-----|-------------|
| Lead / PM | Scope, trade-off, chốt DoD |
| Builder | Làm deliverable |
| Reviewer | Soi Acceptance, bắt lỗi |
| Researcher | Nguồn, fact-check |
| Red Team | Phá, tìm edge-case |
| Librarian | Gom asset, log quyết định |

## Hooks tự kiểm
- H1: README/PRD/ACCEPTANCE/STATE phải tồn tại trước khi /build → BLOCK nếu thiếu
- H2: Output đúng format → WARN
- H3: Không có claim không nguồn → gắn [chưa kiểm chứng]
- H4: Lỗi cũ không tái xuất → BLOCK
- H5: Mỗi build step > 6000 token → BLOCK, bắt chia nhỏ. Mỗi step chưa thẩm định → BLOCK, không cho tiếp.

## Templates
Templates trống nằm tại: `~/.openclaw/workspace/openbuild-v1.0/openbuild-template/`
Copy vào dự án mới khi /intake.
