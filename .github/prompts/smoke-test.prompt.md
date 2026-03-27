---
name: Smoke Test
description: "Chay smoke-test tren live environment va tao report cho PO sign-off."
argument-hint: "v1.0.0"
agent: agent-tester
tools: [read, search, edit, execute]
---
Thuc hien `/smoke-test` cho version duoc truyen vao.

Yeu cau:
1. Doc PROJECT-PROFILE truoc khi chay.
2. Record commit hash va cross-check release readiness.
3. Tao report voi bang ket qua PASS/FAIL.
4. Nhac PO sign-off truoc release.
