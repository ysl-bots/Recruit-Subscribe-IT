# Recruit-Subscribe-IT

IT 직무 **신입·인턴** 채용 공고를 매주 모아 이 저장소에 **Issue**로 올립니다. 저장소를 Watch하면 GitHub 알림 메일로 받아볼 수 있습니다.

- 소스: [링커리어](https://linkareer.com), [자소설닷컴](https://jasoseol.com)
- 발송: 매주 월요일 오전 8시경 (KST)
- 범위: 각 사이트 필터 기준 **신입·인턴 / IT 직군 / 대기업·중견기업·공기업** 공고
- "분야" 칸에는 사이트의 IT 직무 분류(자소설닷컴은 IT 모집 부문명)를 표시합니다.

## Issue 구성

| 섹션 | 내용 |
| --- | --- |
| 마감 임박 (D-7 이내) | 지난주에 소개한 공고도 마감이 가까우면 다시 나옵니다 |
| 이번 주 신규 | 지난 발행 이후 새로 올라온 공고 (마감일 순) |
| 상시 · 채용시 마감 | 마감일이 정해지지 않은 공고 |

## 구독하기

1. 이 저장소 우측 상단 **Watch → Custom** 선택
2. **Issues** 체크 → Apply
3. [알림 설정](https://github.com/settings/notifications)에서 Watching 항목의 **Email**이 켜져 있는지 확인

## 구독 취소

**Watch → Participating and @mentions**(또는 Ignore)로 바꾸면 됩니다.

## 에이전트로 나에게 맞는 공고 고르기

열린 공고 전체가 JSON으로 매일 갱신됩니다. 데이터 해석 규칙(마감 공고 제외, 부문 단위 직무 판단, 오류 의심 공고 분리, 출력 형식 등)은 [`AGENTS.md`](AGENTS.md)에 정리해 두었습니다. 에이전트에 지침을 먼저 읽도록 요청해 보세요.

```
https://raw.githubusercontent.com/ysl-bots/Recruit-Subscribe-IT/main/AGENTS.md 지침을 먼저 읽고,
data/postings.json에서 내 이력([이력 URL 또는 파일])에 맞는 공고를 골라줘.
```

| 파일 | 내용 |
| --- | --- |
| [`AGENTS.md`](https://raw.githubusercontent.com/ysl-bots/Recruit-Subscribe-IT/main/AGENTS.md) | 에이전트용 데이터 해석 지침 (파일 구조, 필드 의미, 검증 규칙, 출력 형식) |
| [`data/postings.json`](https://raw.githubusercontent.com/ysl-bots/Recruit-Subscribe-IT/main/data/postings.json) | 지금 열려 있는 공고 전체 (매일 갱신) |
| [`data/weekly/latest.json`](https://raw.githubusercontent.com/ysl-bots/Recruit-Subscribe-IT/main/data/weekly/latest.json) | 가장 최근 주간 Issue에 실린 공고 (매주 월요일 갱신) |
| [`data/weekly/`](data/weekly) | 주차별(YYYY-Www.json) Issue에 실린 공고. 발행 시점 기록이라 이후 변경은 반영되지 않습니다 |

필드 목록과 의미는 [`AGENTS.md`의 공고 객체 필드](AGENTS.md#3-공고-객체-필드)를 참고하세요.

> 봇이 수집한 정보는 틀릴 수 있습니다. 지원 전에 반드시 원문을 확인하세요. 공고 본문은 저장·재게시하지 않고 원문 링크만 제공합니다.

봇 코드: [ysl-bots/Recruit](https://github.com/ysl-bots/Recruit)
