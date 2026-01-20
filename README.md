# Super Search Skill

일반 리서치 작업을 위한 체계적인 딥 리서치 스킬.

[super_websearch](https://github.com/user/super_websearch) (여행 특화 스킬)의 핵심 장점을 가져와 범용 리서치에 적용할 수 있도록 재설계했습니다.

## 설치 위치

```
.claude/skills/super-search/
├── SKILL.md                    # 핵심 워크플로우 및 원칙
└── references/
    └── EXECUTE.md              # 상세 실행 가이드
```

## 트리거

다음과 같은 요청 시 자동 활성화:
- "research X", "investigate Y", "deep dive on Z"
- "compare A vs B", "find information about"
- 시장 분석, 기술 평가, 경쟁사 조사, 학술 조사 등

## 핵심 기능

### 1. Parallel Multi-Query Strategy
단일 검색에 의존하지 않고 6개 병렬 검색 실행:
- Primary query (직접 검색)
- Alternative terminology (동의어, 관련 개념)
- Negative angle (문제점, 한계, 비판)
- Expert sources (site: 검색)
- Recent developments (시간 필터)
- Comparative context (대안 비교)

### 2. Source Credibility Hierarchy
소스 우선순위:
1. Primary sources (공식 문서, 연구 논문)
2. Verified platforms (전문 출판물, 피어리뷰)
3. Expert analysis (검증된 전문가)
4. Secondary aggregations (뉴스, 블로그)
5. Community sources (포럼, SNS - 감정만 참고)

### 3. 5-Point Validation
모든 핵심 정보 검증:

| Check | Question |
|-------|----------|
| Currency | 최신 정보인가? |
| Relevance | 질문에 직접 답하는가? |
| Authority | 신뢰할 수 있는 출처인가? |
| Corroboration | 복수 소스에서 확인되는가? |
| Accessibility | 사용자가 검증 가능한가? |

### 4. Noise Filtering
자동 제외 대상:
- SEO 콘텐츠 팜
- 페이월 콘텐츠 (대안 없는 경우)
- 비공개 바이어스 소스
- 검증 불가 주장

### 5. Confidence Indicators
결과에 신뢰도 표시:
- **High**: 복수 권위 소스 일치
- **Medium**: 단일 권위 또는 복수 2차 소스
- **Low**: 제한된 소스
- **Unverified**: 단일 소스, 교차검증 필요

## 원본 스킬(super_websearch)에서 가져온 패턴

| 원본 (여행 특화) | super-search (일반 리서치) |
|-----------------|---------------------------|
| 5-Point Place Validation | 5-Point Information Validation |
| Parallel Time-Slot Search | Parallel Multi-Query Strategy |
| Hotplace Filtering | Noise Filtering (SEO/Bias) |
| Source Hierarchy (Diningcode > 블로그) | Source Credibility Hierarchy |
| 5+ Alternatives per slot | 3+ Alternatives for decisions |
| 추천 이유 필수 | Confidence Indicators |
| MD Output with links | Structured Output with sources |

## 출력 형식

```markdown
# [Topic] - Deep Research Report

## Executive Summary
- Key finding 1
- Key finding 2
- ...

## Methodology
- Search queries executed: N
- Sources evaluated: N
- Sources included: N

## Detailed Findings
### Theme 1
[Findings with confidence indicators]

## Sources
- [Source](URL) - description

## Limitations & Gaps
- What couldn't be determined

## Recommended Follow-up
- Next steps
```

## 참고

- 상세 실행 가이드: [references/EXECUTE.md](.claude/skills/super-search/references/EXECUTE.md)
- 원본 여행 스킬: `/Users/jwcorp/super_websearch`
