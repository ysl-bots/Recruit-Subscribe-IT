# Recruit-Subscribe-IT

IT 직무 **신입·인턴** 채용 공고를 매주 모아 이 저장소에 **Issue**로 올립니다. 저장소를 Watch하면 GitHub 알림 메일로 받아볼 수 있습니다.

- 소스: [링커리어](https://linkareer.com), [자소설닷컴](https://jasoseol.com), [캐치](https://www.catch.co.kr). 여러 곳에 올라온 같은 공고는 하나로 합치고 링크를 모두 겁니다.
- 발송: 매주 월요일 오전 8시경 (KST)
- 범위: 신입, 채용연계형·체험형 인턴 (경력 공고 제외)
- 대기업 공채처럼 여러 부문을 한 번에 뽑는 공고는 IT 부문이 있을 때만 싣고, "분야" 칸에 IT 부문명을 표시합니다.

## Issue 구성

| 섹션 | 내용 |
| --- | --- |
| 마감 임박 (D-7 이내) | 지난주에 소개한 공고도 마감이 가까우면 다시 나옵니다 |
| 이번 주 신규 | 지난 발행 이후 새로 올라온 공고 (마감일 순) |
| 상시 · 채용시 마감 | 마감일이 정해지지 않은 공고 |
| IT 관련 가능성 | 봇이 IT 직무인지 확신하지 못한 공고 (사유 표시) |

## 구독하기

1. 이 저장소 우측 상단 **Watch → Custom** 선택
2. **Issues** 체크 → Apply
3. [알림 설정](https://github.com/settings/notifications)에서 Watching 항목의 **Email**이 켜져 있는지 확인

## 구독 취소

**Watch → Participating and @mentions**(또는 Ignore)로 바꾸면 됩니다.

## 에이전트로 나에게 맞는 공고 고르기

열린 공고 전체가 JSON으로 매일 갱신됩니다. Codex·Claude Code 같은 에이전트에 이렇게 요청해 보세요.

```
https://raw.githubusercontent.com/ysl-bots/Recruit/main/data/postings.json 을 읽고,
내 이력([이력 페이지 URL 또는 파일])과 비교해 지원할 만한 공고를 마감일 순으로 정리해줘.
divisions 필드가 있으면 내 직무와 맞는 부문만 골라줘.
```

| 파일 | 내용 |
| --- | --- |
| [`data/postings.json`](https://raw.githubusercontent.com/ysl-bots/Recruit/main/data/postings.json) | 지금 열려 있는 IT·가능성 공고 전체 (매일 갱신) |
| `data/weekly/YYYY-Www.json` | 해당 주 Issue에 실린 공고 |

주요 필드: `company_name`, `title`, `deadline`, `deadline_type`(date / rolling / until_filled), `employment`, `divisions`(모집 부문과 `is_it`), `it_divisions`, `it_tags`, `it_status`(it / maybe), `sources`(원문 링크).

> 봇이 수집한 정보는 틀릴 수 있습니다. 지원 전에 반드시 원문을 확인하세요. 공고 본문은 저장·재게시하지 않고 원문 링크만 제공합니다.

봇 코드: [ysl-bots/Recruit](https://github.com/ysl-bots/Recruit)
