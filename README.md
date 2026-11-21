# Recruit-Subscribe-IT

IT 직무 **신입·인턴** 채용 공고를 매주 모아 이 저장소에 **Issue**로 올립니다. 저장소를 Watch하면 GitHub 알림 메일로 받아볼 수 있습니다.

- 소스: [링커리어](https://linkareer.com), [자소설닷컴](https://jasoseol.com). 여러 곳에 올라온 같은 공고는 하나로 합치고 링크를 모두 겁니다.
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

열린 공고 전체가 JSON으로 매일 갱신됩니다. Codex·Claude Code 같은 에이전트에 이렇게 요청해 보세요.

```
https://raw.githubusercontent.com/ysl-bots/Recruit-Subscribe-IT/main/data/postings.json 을 읽고,
내 이력([이력 페이지 URL 또는 파일])과 비교해 지원할 만한 공고를 마감일 순으로 정리해줘.
divisions 필드가 있으면 내 직무와 맞는 부문만 골라줘.
```

| 파일 | 내용 |
| --- | --- |
| [`data/postings.json`](https://raw.githubusercontent.com/ysl-bots/Recruit-Subscribe-IT/main/data/postings.json) | 지금 열려 있는 공고 전체 (매일 갱신) |
| [`data/weekly/`](data/weekly) | 주차별(YYYY-Www.json) Issue에 실린 공고 |

주요 필드: `company_name`, `title`, `deadline`, `deadline_type`(date / rolling / until_filled), `employment`, `job_tags`(사이트 IT 직무 분류), `divisions`(모집 부문, 자소설닷컴만), `it_divisions`, `sources`(원문 링크), `apply_url`(공채 홈페이지, 없으면 null), `homepage_url`(회사 홈페이지).

> 봇이 수집한 정보는 틀릴 수 있습니다. 지원 전에 반드시 원문을 확인하세요. 공고 본문은 저장·재게시하지 않고 원문 링크만 제공합니다.

봇 코드: [ysl-bots/Recruit](https://github.com/ysl-bots/Recruit)
