---
author: JARVIS
created: 2026-05-20
tags: [raw, status/ingested]
ingest_step: 7/7
---

# 2026-05-20 OpenClaw 컴팩션 에러 해결 세션

## 📅 세션 일시
2026-05-20 22:57 KST ~ 2026-05-21 00:27 KST

## 🔴 증상
- `EmbeddedAttemptSessionTakeoverError: session file changed while embedded prompt lock was released`
- Telegram/webchat 응답 지연 → 무한 대기
- Gateway 재시작 시 세션 복구 실패

## 🔍 근본 원인
compaction 프로세스가 세션 `.jsonl` 파일을 교체하는 도중 SIGTERM 신호를 받아 중간에 중단됨.

## ✅ 해결 과정
1. Config 변경 (`compaction.notifyUser: true`)
2. AGENTS.md 규칙 주입 (컨텍스트 보존, SessionTakeoverError)
3. Gateway 재시작
4. Git 기록

## 📝 논의 내용
1. 컴팩션 메커니즘 분석 및 원인 규명
2. 재발 방지 프로토콜 수립
3. 컨텍스트 보존 원칙 정의
4. Brain_Wiki (Obsidian 지식 베이스) 구축 기획
5. Git 자동 commit & push 절차 도입

## 📋 결정 사항
- `compaction.notifyUser: true` 적용
- AGENTS.md 에 컨텍스트 보존 원칙 + SessionTakeoverError 규칙 주입
- Brain_Wiki 3 계층 구조 (Raw → Core → Meta) 로 지식 베이스 구축
- Git 자동 commit & push 절차 도입

## 🔗 관련
- [[OpenClaw]] 컴팩션 세션 파일 잠금 에러
- [[OpenClaw]] 컨텍스트 보존 원칙
- [[Brain_Wiki]] 수석 지식 아키텍트 역할 규정
- [[Git]] 자동 commit & push 절차
