---
created: 2026-05-20
source: raw/2026-05-20-context-preservation-principle.md
tags: [openclaw, context-preservation, principle, compaction]
---

# 컨텍스트 보존 원칙

## 🧠 핵심 원칙

어떤 상황에서도 이전 대화 컨텍스트 손실 최소화.
**컨텍스트 손실 = 성장의 중단**

## 🔍 문제 배경

- 컴팩션 중 SIGTERM → 세션 파일 깨짐
- `transcript tail is not resumable` → 세션 복구 불가
- 이전 대화 컨텍스트 전체 손실

## ✅ 적용된 조치

| 조치 | 파일 |
|---|---|
| 컨텍스트 보존 원칙 | `AGENTS.md` 6 번 항목 |
| SessionTakeoverError 프로토콜 | `AGENTS.md` 7 번 항목 |
| 세션 백업 규칙 | `memory/YYYY-MM-DD.md` |
| 장기 기억 정제 | `MEMORY.md` |
| Git 기록 | `git commit -a` |

## 📋 재시작 전 체크리스트

```
[ ] memory/YYYY-MM-DD.md — 현재 세션 상태 기록
[ ] 진행 중 작업 요약 — 목적, 상태, 다음 단계
[ ] MEMORY.md — 장기 기록으로 마이그레이션
[ ] commit — git 에 변경사항 저장
```

## 🎯 원칙

저장하려면 이 질문:
> "3 일 후, 이 정보가 Sir 에게 유용한가?"
- Yes → 기록
- No → 버림

지나친 저장은 노이즈를 만들고 역효과를 낸다.

---

## 🔗 관련
- [[OpenClaw]] 컴팩션 세션 파일 잠금 에러
- [[Brain_Wiki]] 수석 지식 아키텍트 역할 규정
- [[Memory]] 장기 기억 관리 프로토콜
