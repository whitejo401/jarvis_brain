---
created: 2026-05-20
source: raw/2026-05-20-git-automation.md
tags: [git, automation, commit, push, security-check]
---

# Git 자동 commit & push 절차

## 🎯 목적

작업 완료 시 변경사항을 자동으로 Git 에 기록하여 데이터 유실 방지 및 추적 가능성 확보.

## 🔄 자동 Commit & Push 프로토콜

```
작업 성공 시:
  1. 변경 파일 확인 (git status)
  2. .commit-check.sh — 보안 점검 자동 실행
  3. Sir 에게 보고: "변경사항 commit & push 진행하겠습니다"
  4. git add + git commit -m "한글 커밋 메시지"
  5. git push
  6. 완료 보고
```

## 📦 자동 commit 대상

- ✅ `memory/YYYY-MM-DD.md` 업데이트
- ✅ AGENTS.md / MEMORY.md 규칙 변경
- ✅ Brain_Wiki 변경사항
- ✅ 작업 결과물 파일

## ❌ Commit 하지 않을 것

- ❌ 시크릿/자격증명
- ❌ 임시/디버깅 파일
- ❌ node_modules/ 등 노이즈
- ❌ Sir 가 "commit 말고 그냥 메모만"이라고 한 경우

## 📝 Commit 메시지 형식

```
feat: <한글 요약>  (새 기능/규칙)
fix: <한글 요약>   (에러 해결/교정)
wiki: <한글 요약>  (Brain_Wiki 변경)
memory: <한글 요약> (메모리/일지 업데이트)
```

## 🛡️ 보안 점검 (.commit-check.sh)

```bash
#!/bin/bash
# commit-security-check.sh
# 커밋 전 보안 점검 스크립트 (Git 추적 파일만 검사)

PATTERNS="sk-[a-zA-Z0-9]{20,}|ghp_[a-zA-Z0-9]{36}|xox[baprs]-[a-zA-Z0-9-]+|github_pat_[a-zA-Z0-9_]{22,}|AKIA[0-9A-Z]{16}"
RESULT=$(grep -rPniE "$PATTERNS" --include="*.md" --include="*.py" --include="*.sh" --include="*.json" -l 2>/dev/null | grep -v ".git/" | grep -v ".commit-check.sh" || true)
```

**검사 항목:**
1. 자격 증명 패턴 검사 (API 키, 토큰, 시크릿)
2. `.env` 파일 커밋 체크
3. 민감 정보 텍스트 체크 (문맥 기반)

## 🔄 Push 실패 시 대응

- 로그 기록 → heartbeat 시 재시도
- 최대 3 회 재시도 후 → Sir 에게 보고

## 📊 적용 현황

| Repo | commit | 내용 |
|---|---|---|
| `jarvis` | `5d7a289` | 작업 완료 시 Git 자동 commit & push 절차 주입 |
| `jarvis` | `a8998ec` | 수석 지식 아키텍트 역할 규정 주입 |
| `jarvis_brain` | `885d270` | Brain_Wiki 초기화 (3 계층 구조) |

---

## 🔗 관련
- [[OpenClaw]] 컴팩션 세션 파일 잠금 에러
- [[Brain_Wiki]] 수석 지식 아키텍트 역할 규정
- [[Security]] 자격 증명 보호 프로토콜
