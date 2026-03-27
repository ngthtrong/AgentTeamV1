# Hướng Dẫn Sử Dụng Agentic Software Team với GitHub Copilot

> Hướng dẫn đầy đủ để vận hành đội ngũ phát triển phần mềm AI trên GitHub Copilot Chat

---

## 📋 Mục Lục

1. [Giới thiệu](#-giới-thiệu)
2. [Cài đặt và Thiết lập](#-cài-đặt-và-thiết-lập)
3. [Đội ngũ Agent](#-đội-ngũ-agent)
4. [Quy trình Làm việc Chuẩn](#-quy-trình-làm-việc-chuẩn)
5. [Danh sách Slash Commands](#-danh-sách-slash-commands)
6. [Các Điểm Kiểm soát (Human Checkpoints)](#-các-điểm-kiểm-soát-human-checkpoints)
7. [Ví dụ Thực tế](#-ví-dụ-thực-tế)
8. [Troubleshooting](#-troubleshooting)
9. [Best Practices](#-best-practices)

---

## 🎯 Giới thiệu

**Agentic Software Team** là hệ thống multi-agent giúp tự động hóa quy trình phát triển phần mềm với kỷ luật cao:

- **7 Agent** chuyên biệt với vai trò rõ ràng
- **24+ Slash Commands** bao phủ toàn bộ vòng đời phát triển
- **TDD-first** — test trước, code sau
- **Git Discipline** — không ai commit trực tiếp lên `main`
- **Human-in-the-loop** — 7 điểm kiểm soát bắt buộc

### Triết lý

> **AI thực thi kỷ luật, con người giữ quyền phán đoán.**

Hệ thống không thay thế developers, mà giúp họ tuân thủ best practices một cách tự động.

---

## 🔧 Cài đặt và Thiết lập

### Yêu cầu

- VS Code với GitHub Copilot extension
- Git repository đã khởi tạo
- Copilot Chat enabled

### Bước 1: Copy thư mục `.github-copilot`

Nếu bạn đang đọc file này, nghĩa là thư mục đã có sẵn!

### Bước 2: Mở Copilot Chat

Trong VS Code:
- **Windows/Linux**: `Ctrl + Shift + I`
- **macOS**: `Cmd + Shift + I`

### Bước 3: Sử dụng `@workspace`

Để Copilot đọc được các file agent và command, luôn bắt đầu với:

```
@workspace Hãy đọc .github-copilot/copilot-instructions.md và thực hiện ...
```

### Bước 4: Chạy Setup (cho dự án mới)

```
@workspace Hãy thực hiện /setup-project theo hướng dẫn trong .github-copilot/commands/setup-project.md
```

---

## 👥 Đội ngũ Agent

### 1. Agent BA (Business Analyst)

**File:** `agents/ba.md`

**Vai trò:**
- Đọc product brief và phân tích yêu cầu
- Viết spec với acceptance criteria dạng Given/When/Then
- Đánh dấu các điểm mơ hồ cần PO làm rõ

**Kích hoạt:**
```
@workspace Hãy đóng vai Agent BA (đọc agents/ba.md) và thực hiện /analyze-requirements BRIEF-00001
```

---

### 2. Agent PM (Project Manager)

**File:** `agents/pm.md`

**Vai trò:**
- Phân rã spec thành chuỗi task TDD
- Quản lý sprint planning
- Commit planning artifacts vào git

**Kích hoạt:**
```
@workspace Hãy đóng vai Agent PM (đọc agents/pm.md) và thực hiện /plan-sprint
```

---

### 3. Agent Developer

**File:** `agents/developer.md`

**Vai trò:**
- Viết failing unit tests (TDD Red Phase)
- Implement features (TDD Green Phase)
- Chạy coverage gate và performance scan

**Kích hoạt:**
```
@workspace Hãy đóng vai Agent Developer (đọc agents/developer.md) và thực hiện /implement-task TASK-00002
```

---

### 4. Agent Tech Lead

**File:** `agents/techlead.md`

**Vai trò:**
- Review Pull Requests với checklist nghiêm ngặt
- Viết Architecture Decision Records (ADR)
- Đảm bảo code quality và NFR compliance

**Kích hoạt:**
```
@workspace Hãy đóng vai Agent Tech Lead (đọc agents/techlead.md) và thực hiện /techlead-review TASK-00002
```

---

### 5. Agent Tester

**File:** `agents/tester.md`

**Vai trò:**
- Viết và chạy E2E tests
- Thực hiện smoke test trên môi trường live
- Tạo báo cáo cho PO ký duyệt

**Kích hoạt:**
```
@workspace Hãy đóng vai Agent Tester (đọc agents/tester.md) và thực hiện /smoke-test v1.0.0
```

---

### 6. Agent Codebase Analyst

**File:** `agents/analyst.md`

**Vai trò:**
- Khám phá và ghi chép hệ thống hiện có (brownfield discovery)
- Phát hiện fragile zones và tech debt
- Tạo tài liệu phân tích "sống"

**Kích hoạt:**
```
@workspace Hãy đóng vai Agent Analyst (đọc agents/analyst.md) và thực hiện /discover-codebase
```

---

### 7. Agent Doc Sync

**File:** `agents/docsync.md`

**Vai trò:**
- Phát hiện drift giữa tài liệu và code
- Cập nhật tài liệu sau mỗi sprint/PR
- Cảnh báo khi tài liệu bị stale

**Kích hoạt:**
```
@workspace Hãy đóng vai Agent Doc Sync (đọc agents/docsync.md) và thực hiện /doc-sync
```

---

## 🔄 Quy trình Làm việc Chuẩn

### Sprint Flow Hoàn Chỉnh

```
┌─────────────────────────────────────────────────────────────┐
│                    SPRINT WORKFLOW                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. SETUP (một lần)                                         │
│     /setup-project → /detect-stack → /discover-codebase     │
│                                                             │
│  2. REQUIREMENTS                                            │
│     Tạo BRIEF-*.md → /analyze-requirements                  │
│     → PO phê duyệt (đổi tên DRAFT-REQ → REQ)               │
│                                                             │
│  3. PLANNING                                                │
│     /plan-sprint → Tasks được tạo và commit                 │
│                                                             │
│  4. DEVELOPMENT (lặp cho mỗi feature)                       │
│     /write-unit-tests TASK-N                                │
│     → /implement-task TASK-N+1                              │
│     → /run-tests TASK-N+2                                   │
│     → /check-coverage TASK-N+1                              │
│     → /create-pr TASK-N+1                                   │
│     → /techlead-review TASK-N+1                             │
│     → /merge-pr TASK-N+1                                    │
│                                                             │
│  5. RELEASE                                                 │
│     /smoke-test v1.0.0 → PO ký duyệt                       │
│     → /release v1.0.0 → Gõ YES                              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Chi tiết từng Phase

#### Phase 1: Setup (Một lần cho dự án mới)

```bash
# 1. Khởi tạo hệ thống
@workspace Hãy thực hiện /setup-project

# 2. Phát hiện công nghệ
@workspace Hãy thực hiện /detect-stack
# → Review STACK.md và PROJECT-PROFILE.md

# 3. Khám phá codebase (cho dự án có sẵn)
@workspace Hãy thực hiện /discover-codebase

# 4. Cập nhật agent với commands cụ thể
@workspace Hãy thực hiện /update-agents
```

#### Phase 2: Requirements

```bash
# 1. Tạo brief
# → Tạo file .github-copilot/workspace/requirements/input/BRIEF-00001.md

# 2. BA phân tích
@workspace Hãy thực hiện /analyze-requirements BRIEF-00001
# → DRAFT-REQ-00001.md được tạo

# 3. PO review và phê duyệt
# → Đổi tên DRAFT-REQ-00001.md thành REQ-00001.md
```

#### Phase 3: Planning

```bash
@workspace Hãy thực hiện /plan-sprint
# → Tasks được tạo:
#    TASK-00001 (unit-test)
#    TASK-00002 (implementation)
#    TASK-00003 (e2e-test)
```

#### Phase 4: Development

```bash
# TDD Red Phase
@workspace Hãy thực hiện /write-unit-tests TASK-00001
# → Tests fail (expected)

# TDD Green Phase
@workspace Hãy thực hiện /implement-task TASK-00002
# → Tests pass, coverage ≥ 80%

# E2E Testing
@workspace Hãy thực hiện /run-tests TASK-00003

# Create PR
@workspace Hãy thực hiện /create-pr TASK-00002

# Tech Lead Review
@workspace Hãy thực hiện /techlead-review TASK-00002

# Merge (sau khi approved)
@workspace Hãy thực hiện /merge-pr TASK-00002
```

#### Phase 5: Release

```bash
# Smoke Test
@workspace Hãy thực hiện /smoke-test v1.0.0
# → PO ký duyệt trong báo cáo

# Release
@workspace Hãy thực hiện /release v1.0.0
# → Gõ YES để xác nhận
```

---

## 📝 Danh sách Slash Commands

### Setup & Discovery

| Command | Mô tả | Agent |
|---------|-------|-------|
| `/setup-project` | Khởi tạo hệ thống cho dự án mới | All |
| `/detect-stack` | Phát hiện công nghệ dự án | Analyst |
| `/discover-codebase` | Phân tích brownfield 10 giai đoạn | Analyst |
| `/update-agents` | Cập nhật guardrails với project commands | Tech Lead |

### Requirements & Planning

| Command | Mô tả | Agent |
|---------|-------|-------|
| `/analyze-requirements BRIEF-ID` | Phân tích brief, tạo spec | BA |
| `/plan-sprint` | Tạo chuỗi task TDD từ specs | PM |
| `/scan-untracked` | Kiểm tra untracked files trước commit | PM |

### Development

| Command | Mô tả | Agent |
|---------|-------|-------|
| `/write-unit-tests TASK-ID` | Viết failing tests (TDD Red) | Developer |
| `/implement-task TASK-ID` | Implement feature (TDD Green) | Developer |
| `/check-coverage TASK-ID` | Kiểm tra test coverage | Developer |

### Testing

| Command | Mô tả | Agent |
|---------|-------|-------|
| `/run-tests TASK-ID` | Viết và chạy E2E tests | Tester |
| `/smoke-test VERSION` | Chạy smoke test trên live env | Tester |

### Review & Merge

| Command | Mô tả | Agent |
|---------|-------|-------|
| `/create-pr TASK-ID` | Tạo Pull Request | Developer |
| `/techlead-review TASK-ID` | Review PR với checklist | Tech Lead |
| `/merge-pr TASK-ID` | Squash merge vào develop | PM/Tech Lead |

### Release

| Command | Mô tả | Agent |
|---------|-------|-------|
| `/release VERSION` | Merge develop → main, tag | PM |

### Bug Management

| Command | Mô tả | Agent |
|---------|-------|-------|
| `/report-bug "description"` | Tạo bug report | Tester |
| `/triage-bug BUG-ID` | Đánh giá và phân loại bug | PM + Tech Lead |
| `/verify-fix BUG-ID` | Xác nhận bug đã được fix | Tester |
| `/bug-status` | Dashboard trạng thái bugs | PM |

### Branch & Docs

| Command | Mô tả | Agent |
|---------|-------|-------|
| `/branch-status` | Hiển thị trạng thái branches | PM |
| `/cleanup-branches` | Xóa branches đã merge | Developer |
| `/doc-sync` | Đồng bộ tài liệu với code | Doc Sync |
| `/dashboard` | Sprint dashboard tổng quan | PM |

---

## 🚦 Các Điểm Kiểm soát (Human Checkpoints)

### 7 điểm bắt buộc cần con người can thiệp:

| # | Checkpoint | Khi nào | Ai quyết định |
|---|------------|---------|---------------|
| 1 | **Stack Review** | Sau `/detect-stack` | Tech Lead / PO |
| 2 | **Spec Approval** | Sau `/analyze-requirements` | PO |
| 3 | **Untracked Files** | Trước planning commits | PO |
| 4 | **ADR Review** | Khi có quyết định kiến trúc | Tech Lead / Team |
| 5 | **Bug P1 Triage** | Khi phát hiện bug critical | PM + PO |
| 6 | **Smoke Test Sign-off** | Sau `/smoke-test` | PO |
| 7 | **Release Confirmation** | Trước `/release` | PO + gõ YES |

### Cách phê duyệt

**Spec Approval:**
```bash
# Đổi tên file
mv DRAFT-REQ-00001.md REQ-00001.md
git add . && git commit -m "approve: REQ-00001"
```

**Smoke Test Sign-off:**
```markdown
# Trong file smoke-{version}-{date}.md
[x] APPROVED — cho phép chạy /release
Chữ ký: Nguyễn Văn A    Ngày: 2024-03-27
```

**Release Confirmation:**
```
> Gõ 'YES' để xác nhận release:
YES
```

---

## 🎬 Ví dụ Thực tế

### Scenario: Thêm tính năng User Authentication

#### Bước 1: Tạo Brief

Tạo file `.github-copilot/workspace/requirements/input/BRIEF-00001.md`:

```markdown
# BRIEF-00001: User Authentication

## Mục tiêu
Người dùng có thể đăng ký, đăng nhập, và đăng xuất.

## User Stories
- Với tư cách user, tôi muốn đăng ký tài khoản mới
- Với tư cách user, tôi muốn đăng nhập với email/password
- Với tư cách user, tôi muốn đăng xuất

## Ràng buộc
- Password phải >= 8 ký tự
- Email phải unique
- JWT token cho authentication
```

#### Bước 2: BA Phân tích

```
@workspace Hãy đóng vai Agent BA và thực hiện /analyze-requirements BRIEF-00001

Đọc file agents/ba.md để hiểu role.
Đọc file commands/analyze-requirements.md để hiểu workflow.
```

**Output:** `DRAFT-REQ-00001.md` với Given/When/Then

#### Bước 3: PO Phê duyệt

```bash
# Review file và trả lời Open Questions
# Sau đó:
mv DRAFT-REQ-00001.md REQ-00001.md
```

#### Bước 4: PM Plan Sprint

```
@workspace Hãy đóng vai Agent PM và thực hiện /plan-sprint
```

**Output:**
- TASK-00001.yaml (unit-test)
- TASK-00002.yaml (implementation)
- TASK-00003.yaml (e2e-test)
- SPRINT-2024-03-27.md

#### Bước 5: Developer Implement

```
@workspace Hãy thực hiện /write-unit-tests TASK-00001

@workspace Hãy thực hiện /implement-task TASK-00002

@workspace Hãy thực hiện /create-pr TASK-00002
```

#### Bước 6: Tech Lead Review

```
@workspace Hãy thực hiện /techlead-review TASK-00002
```

#### Bước 7: Merge và Release

```
@workspace Hãy thực hiện /merge-pr TASK-00002

@workspace Hãy thực hiện /smoke-test v1.0.0

# PO ký duyệt trong báo cáo

@workspace Hãy thực hiện /release v1.0.0
# → Gõ YES
```

---

## 🔧 Troubleshooting

### Copilot không hiểu context

**Vấn đề:** Copilot không đọc được các file agent/command

**Giải pháp:** Luôn sử dụng `@workspace` và chỉ rõ path:
```
@workspace Hãy đọc file .github-copilot/agents/developer.md và thực hiện theo quy trình trong .github-copilot/commands/implement-task.md cho TASK-00002
```

### Task không tồn tại trong git

**Vấn đề:** `/implement-task` báo lỗi task chưa có trong git

**Giải pháp:**
```bash
# Kiểm tra task đã commit chưa
git log --oneline | grep "TASK-00002"

# Nếu chưa, chạy lại /plan-sprint
```

### Coverage dưới 80%

**Vấn đề:** Coverage gate fail

**Giải pháp:**
1. Thêm tests cho uncovered lines
2. Hoặc ghi Tech Debt note với lý do cụ thể:
```markdown
# TECH-DEBT.md
## TD-001: Coverage thấp cho {module}
- **Lý do**: Legacy code khó test
- **Plan**: Refactor trong sprint sau
```

### Conflict khi merge

**Vấn đề:** PR có conflict với develop

**Giải pháp:**
```bash
git checkout feature/TASK-00002-auth
git fetch origin
git merge origin/develop
# Resolve conflicts
git commit
git push
```

### Smoke test fail

**Vấn đề:** Scenarios fail trên live environment

**Giải pháp:**
1. Đọc error logs trong báo cáo
2. Fix và push lên develop
3. Chạy lại `/smoke-test`

---

## 💡 Best Practices

### 1. Luôn đọc file agent trước khi gọi

```
@workspace Hãy đọc .github-copilot/agents/developer.md trước, sau đó thực hiện ...
```

### 2. Một command một lần

Không gọi nhiều commands cùng lúc. Chờ output trước khi tiếp tục.

### 3. Review OUTPUT trước khi tiếp tục

Sau mỗi command, đọc kỹ output để biết:
- Có lỗi gì không?
- Bước tiếp theo là gì?

### 4. Commit thường xuyên

Mỗi khi có thay đổi quan trọng (spec, task, code), đảm bảo đã commit vào git.

### 5. Đừng skip Human Checkpoints

7 điểm kiểm soát không phải suggestions — đó là **requirements**.

### 6. Dùng Templates

Luôn sử dụng templates trong `.github-copilot/templates/` để đảm bảo consistency.

### 7. Update PROJECT-PROFILE.md

Sau khi setup, đảm bảo PROJECT-PROFILE.md có đầy đủ:
- Commands (test, coverage, start, stop)
- Smoke test scenarios
- Health check URL

---

## 📚 Tài liệu tham khảo

- **copilot-instructions.md** — Entry point và tổng quan
- **PROJECT-PROFILE.md** — Cấu hình project-specific
- **agents/*.md** — Chi tiết từng agent
- **commands/*.md** — Chi tiết từng command
- **templates/*.md** — File mẫu

---

## 🆘 Cần hỗ trợ?

1. Đọc kỹ error message
2. Kiểm tra file agent/command tương ứng
3. Verify git status và branch
4. Chạy `/dashboard` để xem tổng quan

---

*Agentic Software Team v1.0 — Built for GitHub Copilot*
*Hệ thống được xây dựng dựa trên nguyên tắc: AI thực thi kỷ luật, con người giữ quyền phán đoán.*
