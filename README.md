# Super Search

체계적인 딥 리서치 스킬 for Claude Code

## 사용법

```
"research [주제]"
"[주제] 조사해줘"
"[A] vs [B] 비교"
```

## 핵심 기능

| 기능 | 설명 |
|------|------|
| **6개 병렬 검색** | 직접/동의어/비판/전문사이트/최신/비교 동시 검색 |
| **5점 검증** | 최신성, 관련성, 권위, 교차확인, 접근성 체크 |
| **노이즈 필터** | SEO 콘텐츠 팜, 페이월, 바이어스 소스 자동 제외 |
| **신뢰도 표시** | High / Medium / Low / Unverified |
| **3+ 대안 제시** | 단일 답변 대신 맥락별 옵션 제공 |

## 출력 형식

```
Executive Summary → Findings (신뢰도 포함) → Sources → Gaps → Follow-up
```

## 설치

**Option 1: 직접 복사**
```
.claude/skills/super-search/ 폴더를 프로젝트에 복사
```

**Option 2: .skill 파일 사용**
```bash
# super-search.skill = 배포용 zip 패키지
unzip super-search.skill -d .claude/skills/
```

## 파일 구조

```
.claude/skills/super-search/
├── SKILL.md              # 워크플로우
└── references/EXECUTE.md # 상세 가이드
```
