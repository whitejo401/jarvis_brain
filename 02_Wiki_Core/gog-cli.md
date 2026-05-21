---
title: gog CLI
category: google-workspace
status: active
version: v0.16.0
installed: true
auth: authenticated
related:
  - Gmail
  - Calendar
  - Drive
  - Sheets
  - Docs
  - Contacts
  - Tasks
  - Chat
  - YouTube
  - Analytics
tags: [google, workspace, gmail, calendar, drive, cli, automation]
created: 2026-05-21
source: raw/gog-cli-reference.md
---

# gog CLI

Google Workspace 완전 제어 CLI. Gmail, Calendar, Drive, Contacts, Sheets, Docs 등 20개 이상 Google 서비스 통합 제어.

## 📦 기본 정보

| 항목 | 값 |
|---|---|
| **버전** | v0.16.0 |
| **바이너리 경로** | `/home/sence401/.local/bin/gog` |
| **설정 파일** | `~/.config/gogcli/config.json` |
| **자격증명** | `~/.config/gogcli/credentials.json` |
| **Client Secret** | `~/.openclaw/google_client_secret.json` |
| **인증 계정** | `whitejo401@gmail.com` |
| **키링 백엔드** | file (source: config) |

## 🔐 인증 (Authentication)

### 초기 설정
```bash
# Client secret 등록
gog auth credentials /home/sence401/.openclaw/google_client_secret.json

# 계정 추가 (서비스 지정)
gog auth add whitejo401@gmail.com \
  --services gmail,calendar,drive,contacts,docs,sheets

# 또는 전체 서비스 자동 감지
gog auth add whitejo401@gmail.com

# 인증 상태 확인
gog auth status
# 또는
gog status
```

### 별칭 (Alias)
| 명령어 | 별칭 |
|---|---|
| `gog login` | `gog auth add` |
| `gog logout` | `gog auth remove` |
| `gog status` / `gog st` | `gog auth status` |

### 계정 설정
```bash
# 환경변수로 기본 계정 설정 (반복 생략)
export GOG_ACCOUNT=whitejo401@gmail.com

# 명령행에서 계정 지정
gog --account=whitejo401@gmail.com gmail search ...
```

## 📧 Gmail

### 메시지 검색
```bash
# 전체 스레드 검색 (1행 = 1 스레드)
gog gmail search 'newer_than:7d' --max 10
gog gmail search 'from:boss@example.com' --max 5
gog gmail search 'label:unread' --max 20

# 개별 메일 검색 (스레드 분리)
gog gmail messages search "in:inbox from:ryanair.com" --max 20

# JSON 출력 (스크립팅용)
gog gmail search 'newer_than:1d' --max 5 --json

# 평문 출력 (TSV, 파싱 안정)
gog gmail search --max 10 --plain
```

### 메일 발송
```bash
# 일반 발송
gog gmail send --to someone@example.com \
  --subject "Meeting Follow-up" \
  --body "Hello, see attached."

# 다중 줄 (heredoc)
gog gmail send --to someone@example.com \
  --subject "Report" \
  --body-file - <<'EOF'
Hi Team,

Here is the weekly report.

Best,
Sir
EOF

# 파일 기반 (여러 줄)
gog gmail send --to someone@example.com \
  --subject "Report" \
  --body-file ./message.txt

# HTML 발송
gog gmail send --to someone@example.com \
  --subject "Report" \
  --body-html "<p>Hello</p><ul><li>Item 1</li></ul>"

# 초안 생성
gog gmail drafts create --to someone@example.com \
  --subject "Draft" --body-file ./draft.txt

# 초안 발송
gog gmail drafts send <draftId>

# 특정 메일에 답장
gog gmail send --to someone@example.com \
  --subject "Re: Original" --body "Reply" \
  --reply-to-message-id <msgId>
```

### 발송 차단 (에이전트 안전)
```bash
# 에이전트 발송 차단 (openclaw.json에서 설정)
gog --gmail-no-send gmail send ...
```

## 📅 Calendar

### 일정 조회
```bash
# 일정 목록
gog calendar events <calendarId> \
  --from 2026-05-21T00:00:00+09:00 \
  --to 2026-05-21T23:59:59+09:00

# 기본 캘린더 (primary)
gog calendar events primary --from 2026-05-01 --to 2026-05-31
```

### 일정 생성
```bash
# 기본 생성
gog calendar create primary \
  --summary "Team Meeting" \
  --from 2026-05-22T14:00:00+09:00 \
  --to 2026-05-22T15:00:00+09:00

# 색상 지정 (ID 1-11)
gog calendar create primary \
  --summary "Important" \
  --from 2026-05-22T10:00:00+09:00 \
  --to 2026-05-22T11:00:00+09:00 \
  --event-color 4

# 색상 수정
gog calendar update primary <eventId> \
  --summary "Updated Title" \
  --event-color 7
```

### 캘린더 색상
| ID | 색상 | ID | 색상 |
|---|---|---|---|
| 1 | #a4bdfc (파랑) | 7 | #46d6db (터쿼이즈) |
| 2 | #7ae7bf (초록) | 8 | #e1e1e1 (회색) |
| 3 | #dbadff (보라) | 9 | #5484ed (진파랑) |
| 4 | #ff887c (빨강) | 10 | #51b749 (녹색) |
| 5 | #fbd75b (노랑) | 11 | #dc2127 (진빨강) |
| 6 | #ffb878 (주황) | | |

```bash
gog calendar colors
```

## 📁 Drive

### 파일 검색 및 관리
```bash
# 파일 검색
gog drive search "query" --max 10
gog drive search "report" --max 5

# 별칭: find
gog drive find "report" --max 5

# 파일 목록
gog drive ls
gog ls  # 별칭

# 파일 다운로드
gog drive download <fileId>
gog download <fileId>  # 별칭

# 파일 업로드
gog drive upload /path/to/file
gog upload /path/to/file  # 별칭

# URL 열기 (브라우저)
gog open <fileId>
gog open "https://drive.google.com/..."
```

## 📄 Docs

```bash
# 텍스트로 내보내기
gog docs export <docId> --format txt --out /tmp/doc.txt

# 텍스트 출력
gog docs cat <docId>

# 별칭
gog doc <command>  # docs → doc
```

> ⚠️ **In-place 편집 불가**: Docs API 클라이언트 필요 (gog 미지원)

## 📊 Sheets

```bash
# 시트 메타데이터
gog sheets metadata <sheetId> --json

# 데이터 조회
gog sheets get <sheetId> "Tab!A1:D10" --json

# 데이터 업데이트
gog sheets update <sheetId> "Tab!A1:B2" \
  --values-json '[["A","B"],["1","2"]]' \
  --input USER_ENTERED

# 데이터 추가 (행 삽입)
gog sheets append <sheetId> "Tab!A:C" \
  --values-json '[["x","y","z"]]' \
  --insert INSERT_ROWS

# 데이터 지우기
gog sheets clear <sheetId> "Tab!A2:Z"

# 별칭: sheet
gog sheet get <sheetId> "Tab!A1:D10" --json
```

## 👥 Contacts / People

```bash
# 연락처 목록
gog contacts list --max 20

# 내 프로필
gog me
gog people me
gog whoami
gog who-am-i

# 별칭: contact, person
```

## ✅ Tasks

```bash
# 할일 관리
gog tasks <command>
gog task <command>  # 별칭
```

## 💬 Chat

```bash
# Google Chat
gog chat <command>
```

## 🎤 YouTube

```bash
# 활동, 비디오, 플레이리스트, 댓글, 채널
gog youtube <command>
gog yt <command>  # 별칭
```

## 🏢 Google Workspace

### Groups
```bash
gog groups <command>
gog group <command>  # 별칭
```

### Admin
```bash
# 도메인 위임 위임 필요
gog admin <command>
```

## 🔧 공통 옵션

| 옵션 | 설명 |
|---|---|
| `-a, --account=EMAIL` | 계정 지정 (API 명령어) |
| `-j, --json` | JSON 출력 (스크립팅용) |
| `-p, --plain` | TSV 평문 출력 (안정 파싱) |
| `-n, --dry-run` | 변경 없이 실행 계획만 출력 |
| `-y, --force` | 파괴적 명령 확인 스킵 |
| `--no-input` | 프롬프트 없이 실패 (CI용) |
| `--results-only` | JSON 모드에서 결과만 출력 |
| `--select=FIELDS` | JSON 필드 선택 (콤마 구분, dot path) |
| `--verbose` / `-v` | 상세 로깅 |
| `--color` | 출력 색상: auto\|always\|never |
| `--client=NAME` | OAuth 클라이언트 선택 |
| `--access-token=TOKEN` | 액세스 토큰 직접 사용 (1시간 유효) |
| `--enable-commands=...` | 명령어 활성화 제한 |
| `--disable-commands=...` | 명령어 비활성화 |

## 🛡️ 에이전트 안전 기능

### 발송 차단
```bash
gog --gmail-no-send gmail send ...
```
openclaw.json에서 활성화 시 에이전트 Gmail 발송 방지.

### 실행 계획 (Dry Run)
```bash
gog --dry-run gmail send --to test@test.com --subject "Test" --body "Hello"
```
실제 발송 없이 실행 계획만 확인.

### 강제 확인 스킵
```bash
gog -y calendar create primary --summary "Test" ...
```
경고 확인 없이 진행.

## 🔄 별칭 정리

| 명령어 | 별칭 |
|---|---|
| `gmail` | `mail`, `email` |
| `drive` | `drv` |
| `docs` | `doc` |
| `slides` | `slide` |
| `calendar` | `cal` |
| `contacts` | `contact` |
| `tasks` | `task` |
| `people` | `person` |
| `sheets` | `sheet` |
| `forms` | `form` |
| `meet` | `meeting` |
| `youtube` | `yt` |
| `classroom` | `class` |
| `analytics` | `ga` |
| `searchconsole` | `gsc`, `search-console`, `webmasters` |
| `appscript` | `script`, `apps-script` |
| `send` | `gmail send` |
| `ls` | `drive ls` |
| `search` | `drive search` / `drive find` |
| `open` | `drive browse` |
| `download` | `drive download` / `dl` |
| `upload` | `drive upload` / `up`, `put` |
| `login` | `auth add` |
| `logout` | `auth remove` |
| `status` | `auth status` / `st` |

## 🎯 OpenClaw 연동 예시

### Heartbeat 시 unread 이메일 확인
```bash
UNREAD=$(gog gmail search 'label:unread' --max 5 --json 2>/dev/null)
# JSON 파싱 후Sir에게 알림
```

### 정기 캘린더 브리핑
```bash
gog calendar events primary \
  --from $(date -d "today 00:00" +%Y-%m-%dT00:00:00+09:00) \
  --to $(date -d "today 23:59" +%Y-%m-%dT23:59:00+09:00)
```

### Drive 파일 백업
```bash
gog drive search "backup" --max 10
```

## 🔗 관련
- [[Brain_Wiki]] 수석 지식 아키텍트 역할 규정
- [[OpenClaw]] 세션 관리
- [[Memory]] 지속적 메모 시스템
