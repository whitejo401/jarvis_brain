---
created: 2026-05-20
source: raw/2026-05-20-openclaw-compaction-error.md
tags: [openclaw, compaction, session, error, troubleshooting]
---

# OpenClaw 컴팩션 세션 파일 잠금 에러 해결

## 🔴 증상

- `EmbeddedAttemptSessionTakeoverError: session file changed while embedded prompt lock was released`
- Telegram/webchat 응답 지연 → 무한 대기
- Gateway 재시작 시 세션 복구 실패 (`transcript tail is not resumable`)

## 🔍 근본 원인

compaction 프로세스가 세션 `.jsonl` 파일을 교체하는 도중 SIGTERM 신호를 받아 중간에 중단됨.
교체된 파일이 깨져서 `writeLock` 재요청 시 `SessionTakeoverError` 발생.
이후 컴팩션 재시도 시 `transcript tail is not resumable` → 세션 복구 불가.

**타임라인:**
```
22:57:08  config change 감지 (compaction.notifyUser)
22:57:15  SIGTERM → gateway restart
22:57:53  stale lock 제거 완료
22:57:58  ❌ 세션 복구 실패
23:02:45  ❌ Context overflow 감지
23:03:04  ✅ auto-compaction 성공
```

## ✅ 해결 과정

### 1. Config 변경
- `~/.openclaw/openclaw.json` → `agents.defaults.compaction.notifyUser: true`
- 컴팩션 시작/완료 시 Sir에게 알림 발송

### 2. AGENTS.md 규칙 주입
- 6. 컨텍스트 보존 원칙 (Context Preservation)
- 7. 세션 파일 잠금 에러 대응 (SessionTakeoverError)

### 3. Gateway 재시작
- `openclaw gateway restart` 실행
- 새 세션 생성되어 정상 가동

### 4. Git 기록
- commit: `c1ffa21` feat: context preservation principle + SessionTakeoverError protocol

## 🛡️ 재발 방지 조치

| 조치 | 상태 |
|---|---|
| `compaction.notifyUser: true` | ✅ 적용 |
| SessionTakeoverError 프로토콜 | ✅ 주입 |
| 컨텍스트 보존 원칙 | ✅ AGENTS.md 주입 |
| `memory/` 백업 프로토콜 | ✅ 규칙화 |
| `.commit-check.sh` 보안 점검 | ✅ 자동화 |

## 🔧 재시도 체크리스트
- [ ] 컴팩션 발생 시 `notifyUser` 알림 확인
- [ ] `SessionTakeoverError` 감지 시 AGENTS.md 규칙에 따라 보고
- [ ] 재시작 전 `memory/YYYY-MM-DD.md`에 컨텍스트 백업
- [ ] `bash .commit-check.sh` 후 commit & push

## 🎯 자동 감지 규칙
- `embedded prompt lock released` → compaction race condition 의심
- `stale session lock` 감지 → 자동 정리 제안
- 컴팩션 중 SIGTERM → 컨텍스트 손실 가능성 확인

---

## 🔗 관련
- [[OpenClaw]] 컨텍스트 보존 원칙
- [[Brain_Wiki]] 수석 지식 아키텍트 역할 규정
- [[Git]] 자동 commit & push 절차
