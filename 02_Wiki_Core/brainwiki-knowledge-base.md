---
created: 2026-05-20
source: raw/2026-05-20-brainwiki-planning.md
tags: [brainwiki, obsidian, knowledge-base, architecture, planning]
---

# Brain_Wiki 지식 베이스 구축 기획

## 🎯 목적

Sir 와 JARVIS 가 공동으로 Obsidian 기반 개인 지식 베이스를 구축하고 관리.
karpathy 의 LLM-Wiki 패턴을 참고하여 Sir 의 상황에 맞게 재해석.

## 📐 3 계층 아키텍처

```
/mnt/d/Brain_Wiki/
├── 01_Raw_Sources/     # Human(AI) 공동 인큐베이팅 폴더
├── 02_Wiki_Core/       # 구조화·정제된 최종 지식
├── 03_Meta_Index/      # index.md + log.md
└── .obsidian/          # (불변)
```

**파일 명명 규칙:**
| 레이어 | 형식 | 예시 |
|---|---|---|
| `01_Raw_Sources/` | `YYYY-MM-DD-<topic>.md` | `2026-05-20-openclaw-compaction.md` |
| `02_Wiki_Core/` | 주제 기반 | `openclaw-compaction-error.md` |
| `03_Meta_Index/` | 고정 | `index.md`, `log.md` |

## 🔄 Two-Track Routing

### Track A: Raw → Ingest (즉시 승격)
- **대상:** 시스템 오류 해결, 인프라 구축, 기능 추가, 코드 픽스 (확정된 기술적 사실)
- **동작:**
  1. `01_Raw_Sources/` 에 Raw 문서 작성 (author: JARVIS)
  2. 즉시 Ingest → `02_Wiki_Core/` 로 정제 문서 생성
  3. `index.md` + `log.md` 업데이트
  4. Raw 문서에 `tags: [raw, status/ingested]` 부여

### Track B: Raw Incubation (보류 및 공동 인큐베이팅)
- **대상:** 기획 아이디어, 카테고리화 애매한 정보, 긴 호흡 리서치
- **동작:** `01_Raw_Sources/` 에 임시 문서 누적 (Append)
- **Ingest 판단 기준 (3 개 중 1 개 충족 시):**
  - 3 개 이상의 참조 문서가 생성되었을 때
  - 실제 적용/검증 사례가 있을 때
  - Sir 가 "Ingest 해줘"라고 지시할 때
- **Ingest 실행:** Sir 에게 승인 요청 또는 자율 처리

## 📝 Strict Attribution

**Raw 문서 YAML frontmatter:**
```yaml
---
author: Sir       # 또는 JARVIS
created: YYYY-MM-DD
tags: [raw, incubation]
---
```

**Ingest 완료 후 Raw 문서:**
- 삭제 ❌ (원본 손실)
- 태그 변경 ✅ → `tags: [raw, status/ingested]`
- 문서 내용 유지 (참조용)

## 🛡️ Write-Safe 프로토콜

```
❌ write("file.md", content)  ← 에러 발생 시 손실

✅ 원자적 쓰기:
  1. write("file.tmp", content)  # 임시 파일
  2. exec("mv file.tmp file.md")  # 원자적 교체
  3. rename 실패 → .tmp 파일 보존 (재시도 가능)
```

## 🔄 Ingest Checkpoint (작업 중단점 기록)

Raw 문서 frontmatter 에 단계 기록:
```yaml
---
author: JARVIS
created: 2026-05-20
tags: [raw, status/ingesting]
ingest_step: 4/7
---
```

**7 단계 중 어디라도 에러 발생 → checkpoint 보존 → `status/ingesting` 태그 유지 → Sir 지시 시 재개**

## 🎯 Intent 분류 (Sir 의 지시 → 실행)

| Sir 의 지시 | Intent | 동작 |
|---|---|---|
| "이어서 해줘" | Resume | `ingesting` 문서 → 멈춘 단계부터 재개 |
| "ingest 마무리 해줘" | Batch Ingest | 모든 `ingesting` 문서 → 일괄 완료 |
| "Wiki 에 정리해줘" | New Ingest | 새 문서 생성 → Raw → Core Ingest |
| "Raw 에 넣어줘" | Raw Only | `01_Raw_Sources/`에만 작성 (Ingest 안 함) |
| "Raw 에 잠시만" | Incubation | Track B 보류 (Ingest 안 함) |
| "상태 보여줘" | Status | `ingesting`/`ingested` 목록 보고 |
| "Lint 해줘" | Lint | 무결성 검사 즉시 실행 |
| "취소해줘" | Rollback | `ingesting` → `incubation`으로 롤백 |

## 🧹 Compound & Lint

### Compound (지식 복리)
- 대화 중 인사이트/분석표 → 채팅에 휘발 ❌ → wiki 업데이트 ✅
- 관련 개념에 `[[양방향 링크]]` 연결

### Lint (무결성 검사)
- **실행 방식:** cron job (isolated session) — 메인 세션과 분리
- **실행 주기:** 매주 1 회 (Sunday 02:00 KST)
- **검사 항목:**
  - 끊어진 링크 (broken links)
  - 고아 문서 (inbound link 없는 페이지)
  - 모순된 주장
  - 중복 개념
- **제약:** 한 번에 최대 5 개 파일 수정 → 컴팩션 충돌 방지

## 📊 Meta Index 구조

**`03_Meta_Index/index.md`**
```markdown
# 📚 Brain_Wiki Index

## Wiki Core
| 페이지 | 요약 | 생성일 |
|---|---|---|
| [[openclaw-compaction-error]] | 컴팩션 세션 파일 잠금 에러 해결 | 2026-05-20 |

## Raw Sources
| 문서 | 작성자 | 상태 |
|---|---|---|
| [[2026-05-20-idea-x]] | Sir | incubation |

---
_Last updated: 2026-05-20_
```

**`03_Meta_Index/log.md`**
```markdown
## 2026-05-20

- [00:30] init | Brain_Wiki 구조 초기화 (3 계층)
- [00:30] ingest | OpenClaw 컴팩션 에러 해결
- [00:30] ingest | 컨텍스트 보존 원칙
- [00:30] raw | Brain_Wiki 작성 규칙 초안
```

## 🔗 Git 연동

- **리포지토리:** `https://github.com/whitejo401/jarvis_brain.git`
- **commit 전:** `.commit-check.sh` 보안 점검
- **commit 메시지:** `wiki: <한글 요약>`

---

## 🔗 관련
- [[karpathy]] LLM-Wiki 패턴
- [[OpenClaw]] 컴팩션 세션 파일 잠금 에러
- [[OpenClaw]] 컨텍스트 보존 원칙
- [[Git]] 자동 commit & push 절차
