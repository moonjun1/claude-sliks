# claude-skills

Claude Code 에이전트 모음 - 개발 워크플로우 자동화

## 에이전트 목록

| 에이전트 | 역할 | 모델 |
|---------|------|------|
| `visual-inspector` | UI 확인, 스크린샷, 브라우저 버그 검사 | Sonnet |
| `code-reviewer` | 코드 품질/보안/성능 리뷰 | Haiku |
| `pr-writer` | PR 제목/본문 자동 작성 | Haiku |
| `test-writer` | 유닛/통합 테스트 자동 작성 | Sonnet |
| `git-helper` | 커밋 메시지 자동 생성, 브랜치 전략 | Haiku |
| `brand-logo-finder` | 브랜드 로고/에셋 검색 | Haiku |

## 핵심 설계 원칙

**컨텍스트 절약**: MCP 스크린샷/브라우저 작업을 `visual-inspector` 에이전트로 격리해 메인 컨텍스트 소비를 최소화

```
메인 Claude (지휘)
    ├── visual-inspector  → 스크린샷 찍고 요약만 반환
    ├── code-reviewer     → 코드 리뷰 후 문제점만 반환
    ├── pr-writer         → PR 작성 후 텍스트만 반환
    ├── test-writer       → 테스트 작성 후 결과만 반환
    ├── git-helper        → 커밋 메시지 생성 후 반환
    └── brand-logo-finder → 로고 URL 찾아서 반환
```

> 슈퍼클로드와 함께 사용하는 것을 권장합니다.
> 문서 작성, 리서치, 디버깅은 슈퍼클로드의 `technical-writer`, `deep-research-agent`, `root-cause-analyst` 사용.

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
"PR 설명 작성해줘"                  → pr-writer 자동 호출
"이 함수 테스트 짜줘"               → test-writer 자동 호출
"커밋 메시지 써줘"                  → git-helper 자동 호출
"나이키 로고 찾아줘"                → brand-logo-finder 자동 호출
```

직접 지정도 가능합니다:
```
"code-reviewer 써서 auth.ts 리뷰해줘"
"test-writer로 이 컴포넌트 테스트 작성해줘"
```
