# claude-skills

Claude Code 에이전트 모음 - 개발 워크플로우 자동화

## 에이전트 목록

| 에이전트 | 역할 | 모델 |
|---------|------|------|
| `visual-inspector` | UI 확인, 스크린샷, 브라우저 버그 검사 | Sonnet |
| `code-reviewer` | 코드 품질/보안/성능 리뷰 | Haiku |
| `debugger` | 버그 근본 원인 분석 및 수정 | Sonnet |
| `pr-writer` | PR 제목/본문 자동 작성 | Haiku |
| `doc-writer` | README, API 문서, 주석 작성 | Haiku |
| `research-agent` | 웹 심층 리서치 및 리포트 작성 | Sonnet |
| `brand-logo-finder` | 브랜드 로고/에셋 검색 | Haiku |

## 핵심 설계 원칙

**컨텍스트 절약**: MCP 스크린샷/브라우저 작업을 `visual-inspector` 에이전트로 격리해 메인 컨텍스트 소비를 최소화

```
메인 Claude (지휘)
    ├── visual-inspector  → 스크린샷 찍고 요약만 반환
    ├── code-reviewer     → 코드 리뷰 후 문제점만 반환
    ├── debugger          → 버그 분석/수정 후 결과만 반환
    └── pr-writer         → PR 작성 후 텍스트만 반환
```

## 설치

### 모든 프로젝트에서 사용 (권장)
```bash
cp .claude/agents/*.md ~/.claude/agents/
```

### 특정 프로젝트에서만 사용
```bash
cp -r .claude/agents/ your-project/.claude/agents/
```

## 사용 방법

Claude가 자동으로 상황에 맞는 에이전트를 선택합니다.

```
"로그인 페이지 UI 확인해줘"         → visual-inspector 자동 호출
"이 PR 올리기 전에 리뷰해줘"        → code-reviewer 자동 호출
"에러 났어: [에러 메시지]"           → debugger 자동 호출
"PR 설명 작성해줘"                  → pr-writer 자동 호출
"이 함수 문서화해줘"                → doc-writer 자동 호출
"React Server Components 리서치해줘" → research-agent 자동 호출
```

직접 지정도 가능합니다:
```
"code-reviewer 써서 auth.ts 리뷰해줘"
"debugger한테 이 에러 맡겨줘"
```

## 참고

- `visual-inspector`, `debugger`: memory 기능으로 패턴 누적 학습
- `code-reviewer`: 개발자별 반복 실수 패턴 기억
- `debugger`: 프로젝트별 버그 패턴 기억
