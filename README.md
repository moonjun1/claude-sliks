# claude-skills

SuperClaude와 함께 사용하는 Claude Code 커스텀 에이전트 모음

---

## 전체 설정 구조

```
~/.claude/
├── CLAUDE.md                 # SuperClaude 진입점 (자동 로드)
├── settings.json             # 전역 설정
├── agents/                   # 에이전트 (23개)
│   ├── [SuperClaude 기본 17개]
│   └── [커스텀 6개] ← 이 레포
└── commands/sc/              # SuperClaude 슬래시 커맨드 (25개)
```

---

## 커스텀 에이전트 6개

> SuperClaude 기본 에이전트에 없는 기능만 추가

| 에이전트 | 언제 쓰나 | 모델 |
|---------|---------|------|
| `visual-inspector` | UI 확인, 스크린샷, 브라우저 버그 | Sonnet |
| `code-reviewer` | 코드 품질/보안/성능 리뷰 | Haiku |
| `pr-writer` | PR 제목/본문 자동 작성 | Haiku |
| `test-writer` | 테스트 코드 자동 작성 | Sonnet |
| `git-helper` | 커밋 메시지 생성, 브랜치 전략 | Haiku |
| `brand-logo-finder` | 브랜드 로고/에셋 검색 | Haiku |

---

## SuperClaude 기본 에이전트 17개

> 이미 설치되어 있음 — 별도 설치 불필요

| 에이전트 | 언제 쓰나 |
|---------|---------|
| `frontend-architect` | React, Next.js, UI 설계 |
| `backend-architect` | API, 서버, DB 설계 |
| `system-architect` | 전체 시스템 아키텍처 |
| `security-engineer` | 보안 취약점 분석 |
| `performance-engineer` | 성능 최적화 |
| `quality-engineer` | 테스트 전략, 품질 관리 |
| `refactoring-expert` | 코드 리팩토링 |
| `technical-writer` | 문서 작성 |
| `deep-research-agent` | 심층 웹 리서치 |
| `root-cause-analyst` | 버그 근본 원인 분석 |
| `devops-architect` | CI/CD, 인프라 |
| `python-expert` | Python 전문 코드 |
| `backend-architect` | 백엔드 설계 |
| `pm-agent` | 프로젝트 관리, 문서화 |
| `requirements-analyst` | 요구사항 분석 |
| `learning-guide` | 개념 학습, 설명 |
| `business-panel-experts` | 비즈니스 전략 분석 |
| `socratic-mentor` | 소크라테스식 학습 |

---

## SuperClaude 슬래시 커맨드 25개

```
/sc:help          모든 커맨드 목록 보기
/sc:implement     기능 구현
/sc:analyze       코드 분석
/sc:improve       코드 개선
/sc:test          테스트 실행
/sc:build         빌드
/sc:research      웹 리서치
/sc:design        아키텍처 설계
/sc:document      문서 작성
/sc:git           git 작업
/sc:troubleshoot  문제 해결
/sc:refactor      리팩토링 (cleanup)
/sc:explain       개념 설명
/sc:brainstorm    요구사항 탐색
/sc:workflow      워크플로우 생성
/sc:task          복잡한 작업 관리
/sc:spawn         멀티 에이전트 오케스트레이션
/sc:pm            프로젝트 매니저
/sc:load          세션 로드
/sc:save          세션 저장
/sc:reflect       작업 검토
/sc:estimate      개발 공수 산정
/sc:business-panel 비즈니스 전략 패널
/sc:spec-panel    스펙 리뷰
/sc:select-tool   MCP 툴 선택
```

---

## 실제 사용 예시

### 개발할 때
```
"로그인 페이지 UI 확인해줘"
→ visual-inspector가 스크린샷 찍고 요약만 반환 (컨텍스트 절약)

"이 코드 리뷰해줘"
→ code-reviewer가 변경사항 분석 후 우선순위별 피드백

"PR 설명 써줘"
→ pr-writer가 git diff 분석 후 PR 본문 자동 생성

"이 함수 테스트 짜줘"
→ test-writer가 Jest/Vitest/pytest 맞춤 테스트 작성

"커밋 메시지 써줘"
→ git-helper가 변경사항 분석 후 Conventional Commits 형식으로 생성
```

### 설계/분석할 때
```
/sc:design "인증 시스템 설계해줘"
/sc:analyze "이 코드 분석해줘"
/sc:brainstorm "어떤 기능 추가할까"
/sc:research "React Server Components 리서치해줘"
```

### 에이전트 직접 지정
```
"visual-inspector로 체크아웃 페이지 확인해줘"
"test-writer로 auth.ts 테스트 작성해줘"
"frontend-architect한테 이 컴포넌트 구조 리뷰 맡겨줘"
```

---

## 컨텍스트 절약 원리

MCP(Playwright/Chrome)로 스크린샷을 직접 찍으면 이미지+HTML이 메인 컨텍스트에 쌓여서 빨리 고갈됩니다.

```
❌ 기존 방식
메인 Claude → 스크린샷 → 이미지/HTML 전체가 메인 컨텍스트에 쌓임

✅ visual-inspector 사용
메인 Claude → visual-inspector (별도 컨텍스트에서 스크린샷 처리)
                              → "버튼 클릭 안됨" 3줄 요약만 반환
```

---

## 설치

```bash
# 이 레포 클론
git clone https://github.com/moonjun1/claude-sliks.git

# 모든 프로젝트에서 사용 (권장)
cp claude-sliks/.claude/agents/*.md ~/.claude/agents/
```

---

## 계정 전환 (gh CLI)

```bash
gh auth switch -u moonjun1    # moonjun1 계정으로
gh auth switch -u xl8jayden   # xl8jayden 계정으로
gh auth status                # 현재 계정 확인
```
