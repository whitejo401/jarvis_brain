---
author: JARVIS
created: 2026-05-20
tags: [raw, status/ingested]
ingest_step: 7/7
---

# 2026-05-20 Brain_Wiki 구축 기획 세션

## 📅 세션 일시
2026-05-20 23:58 KST ~ 2026-05-21 00:27 KST

## 🎯 논의 목적
karpathy 의 LLM-Wiki 패턴을 참고하여 Sir 의 Obsidian 지식 베이스 구축 기획

## 📐 최종 아키텍처

```
/mnt/d/Brain_Wiki/
├── 01_Raw_Sources/     # Human(AI) 공동 인큐베이팅 폴더
├── 02_Wiki_Core/       # 구조화·정제된 최종 지식
├── 03_Meta_Index/      # index.md + log.md
└── .obsidian/          # (불변)
```

## 🔄 주요 규칙
- Two-Track Routing (Track A: 즉시 승격 / Track B: 보류)
- Strict Attribution (YAML frontmatter)
- Write-Safe 프로토콜 (원자적 쓰기)
- Ingest Checkpoint (7 단계 중 어디든 에러 → 중단점 보존)
- Lint 무결성 검사 (매주 1 회 cron job)

## 📋 결정 사항
1. 3 계층 구조 적용 (Raw → Core → Meta)
2. Two-Track Routing 적용
3. Strict Attribution (author: Sir/JARVIS)
4. Write-Safe 프로토콜 (원자적 쓰기)
5. Ingest Checkpoint (7 단계 중 어디든 에러 → 중단점 보존)
6. Lint 무결성 검사 (매주 1 회 cron job)
7. Git 연동 (`jarvis_brain` 리포지토리)

## 📝 논의 내용
1. karpathy LLM-Wiki 패턴 정독 및 분석
2. Sir 의 요구사항 수집 (에러 대응방침, Intent 분류 등)
3. 최종 기획안 수립 및 합의
4. `agent.md` 에 역할 규정 주입
5. Brain_Wiki Git 리포지토리 초기화
6. Lint cron job 설정 (Sunday 02:00 KST isolated)

## 🔗 관련
- [[Brain_Wiki]] 수석 지식 아키텍트 역할 규정
- [[karpathy]] LLM-Wiki 패턴
