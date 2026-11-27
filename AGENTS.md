# AGENTS.md — 공고 데이터 해석 지침

이 문서는 이 저장소의 공고 JSON을 읽는 AI 에이전트를 위한 지침입니다.
사용자가 "공고를 골라줘", "정리해줘"처럼 요청하면 아래 규칙에 따라 데이터를 해석하고 답하세요.

- Raw URL 기준 경로: `https://raw.githubusercontent.com/ysl-bots/Recruit-Subscribe-IT/main/`
- 데이터 출처: 링커리어, 자소설닷컴 (각 사이트 필터 기준: 신입·인턴 / IT 직군 / 대기업·중견기업·공기업)
- 봇이 자동으로 수집한 데이터라 틀린 값이 있을 수 있습니다. 답에 그대로 옮기지 말고 아래 검증 규칙을 거치세요.

---

## 1. 파일 선택

| 파일 | 내용 | 갱신 | 용도 |
| --- | --- | --- | --- |
| `data/postings.json` | 지금 열려 있는 공고 전체 | 매일 | 기본값 · 일반 공고 선별 |
| `data/weekly/latest.json` | 가장 최근 주간 Issue에 실린 공고 | 매주 월요일 | 이번 주 공고 · 최신 Issue |
| `data/weekly/YYYY-Www.json` | 특정 주차 Issue에 실린 공고 (ISO 주차) | 발행 시 1회 | 지난 주차 조회 · 주차 비교 |

- 사용자가 파일을 지정하지 않으면 `data/postings.json`을 읽으세요.
- `latest.json`은 가장 최근 주차 파일과 내용이 같습니다.
- 주간 파일은 발행한 시점의 기록이라, 그 뒤에 마감되었거나 바뀐 내용은 반영되어 있지 않습니다.

## 2. 파일 구조

### `data/postings.json`
```json
{ "updated": "ISO 8601 시각", "count": 0, "postings": [ /* 공고 객체 */ ] }
```

### `data/weekly/*.json`
```json
{
  "week": "YYYY-Www",
  "published": "YYYY-MM-DD",
  "sections": { "urgent": ["id"], "new": ["id"], "rolling": ["id"] },
  "postings": [ /* 공고 객체 */ ]
}
```

- `sections`에는 공고 `id`만 들어 있습니다. 공고 내용은 같은 파일의 `postings[]`에서 `id`로 찾아 연결하세요.
- 섹션의 의미는 주간 Issue와 같습니다.

| 키 | Issue 섹션 | 기준 |
| --- | --- | --- |
| `urgent` | 마감 임박 | 발행일 기준 D-7 이내. 이전 주에 소개된 공고도 다시 들어갈 수 있음 |
| `new` | 이번 주 신규 | 지난 발행 이후 새로 수집된, 마감일이 있는 공고 |
| `rolling` | 상시 · 채용시 마감 | 마감일이 정해지지 않은 공고 |

## 3. 공고 객체 필드

| 필드 | 타입 | 의미 / 사용법 |
| --- | --- | --- |
| `id` | string | 공고 고유 id. 섹션과 연결할 때만 쓰고 사용자에게는 보여주지 않음 |
| `company_name` | string | 화면에 표시할 회사명 |
| `company` | string | 중복 판단용으로 정규화한 회사 키. 표시하지 않음 |
| `title` | string | 공고 제목 |
| `deadline` | `YYYY-MM-DD` \| null | 마감일 |
| `deadline_type` | enum | `date` 마감일 있음 / `rolling` 상시 채용 / `until_filled` 채용시 마감 |
| `start_date` | `YYYY-MM-DD` \| null | 접수 시작일 |
| `employment` | string | 공고 전체의 고용 형태. 예: 신입, 인턴, 채용연계형 인턴, 체험형 인턴 |
| `job_tags` | string[] | 수집 사이트의 IT 직무 분류. 범위가 넓은 태그 |
| `divisions` | object[] \| null | 모집 부문 `[{ name, is_it, employment }]`. 자소설닷컴 공고에만 있고, 없으면 null |
| `it_divisions` | string[] | `divisions` 중 `is_it=true`인 부문의 이름 목록. `is_it`은 자소설닷컴 직무그룹 기준 |
| `sources` | object[] | 원문 정보 `[{ source, source_id, url }]`. `source`는 `linkareer` 또는 `jasoseol` |
| `apply_url` | string \| null | 공채 홈페이지(지원 페이지) |
| `homepage_url` | string \| null | 회사 홈페이지 |
| `detail_checked` | boolean | 봇이 상세 페이지까지 확인했는지 여부. false면 정보가 빠져 있을 수 있음 |
| `first_seen` / `last_seen` | `YYYY-MM-DD` | 봇이 처음 / 마지막으로 수집한 날짜 |
| `published_weeks` | string[] | 이 공고가 실린 주간 Issue 주차 목록 |
| `status` | string | 공고 상태. `open`만 지원 대상이며 그 외 값은 제외. 단, `open`이어도 봇이 갱신하지 못했을 수 있으니 아래 날짜 규칙을 먼저 적용 |

이 표에 없는 필드가 새로 생기면 이름을 보고 뜻을 추측하지 말고 판단에 쓰지 마세요. 답 끝에 "알 수 없는 필드: …"라고 한 줄로만 알리세요.

## 4. 해석 규칙

### 직무 적합 판단
1. `divisions`가 있으면 `job_tags`보다 `divisions`를 먼저 봅니다. 부문 단위로 판단하세요.
2. `is_it=false`인 부문은 IT 직무가 아니므로, 사용자가 따로 원하지 않으면 제외합니다.
   - `is_it`은 자소설닷컴이 그 부문에 붙인 직무그룹이 IT 필터에 걸리는지만 나타냅니다. 사이트 분류가 넓어서 부문 이름이 명백히 IT가 아닌데도(예: 시사교양PD) true일 수 있으니, 이런 부문은 "부문 분류 의심"으로 표시하세요.
   - `divisions`가 있는데 `it_divisions`가 비어 있으면 공고 전체를 제외합니다.
3. 부문별 `employment`가 공고 전체의 `employment`와 다를 수 있습니다. 둘이 다르면 부문 값을 따르세요.
4. `divisions`가 null이거나 빈 배열이면 `job_tags`와 `title`로 판단하고, "부문 정보 없음"이라고 표시하세요.

### 날짜
1. D-day는 파일에 적힌 발행일이 아니라 **지금 날짜(KST)** 를 기준으로 다시 계산하세요.
2. `deadline`이 오늘보다 앞선 공고는 `status`와 관계없이 마감된 것으로 보고 제외합니다. 오늘 마감(D-0)인 공고는 포함합니다.
3. `rolling`·`until_filled` 공고는 `deadline`이 null일 수 있습니다. 정렬할 때는 마감일이 있는 공고 뒤에 두세요.
4. 주간 파일을 읽을 때 `published`가 오늘보다 7일 넘게 지났다면, 데이터가 그 시점 기준이라고 사용자에게 알리세요.

### 데이터 오류 의심 (순위 제외 · 별도 표시)
- `deadline`이 오늘보다 1년 넘게 뒤인 공고
- `title`에 적힌 연도가 올해(KST)보다 작은 공고 (예: 2026년에 "2025 하반기 채용")
- `deadline`이 `start_date`보다 앞선 공고

### 원문 확인 필요 (목록 유지 · 표시)
- `detail_checked=false`
- `apply_url`이 null

### 중복
- 같은 공고가 여러 사이트에 올라오면 하나로 합쳐지고, `sources`에 링크가 여러 개 들어 있습니다.
- 그래도 같은 `company`와 거의 같은 `title`을 가진 공고가 남아 있으면 하나로 묶고 링크는 모두 남기세요.
- "거의 같은 title"은 앞쪽 대괄호(`[회사명]` 등)·공백·특수문자를 빼고 비교했을 때 같은 경우를 말합니다.

## 5. 답변 원칙

1. JSON에 없는 정보(연봉, 근무지, 자격요건, 전형 일정 등)를 지어내지 마세요. 필요하면 원문 링크를 열어 확인하고, 확인한 정보라고 밝히세요.
2. 링크는 `apply_url`을 먼저 쓰고, 없으면 `sources[].url`을 쓰세요.
3. 적합도는 `상`/`중`/`하` 3단계로 매깁니다. 사용자의 이력이 주어지지 않았으면 적합도·이유 열을 빼세요.
4. 사용자의 이력과 비교할 때는 적합도를 매긴 근거로 이력 항목과 공고 부문을 함께 적으세요.
5. 답 끝에 "봇이 수집한 정보는 틀릴 수 있으니 지원 전에 원문을 확인하세요"라는 안내를 붙이세요.

### 기본 출력 형식 (별도 요청 없을 때)

| 마감 (D-day) | 회사 | 공고 | 부문 | 유형 | 적합도 | 이유 | 링크 |
| --- | --- | --- | --- | --- | --- | --- | --- |

- 정렬 순서: 마감일이 빠른 순 → 상시·채용시 마감
- 표가 너무 넓으면 링크 열을 빼고 회사명에 링크를 걸어도 됩니다.
- 표 아래에 "데이터 오류 의심"과 "원문 확인 필요" 공고를 따로 나열하세요.

## 6. 요청 예시

```
https://raw.githubusercontent.com/ysl-bots/Recruit-Subscribe-IT/main/AGENTS.md 지침을 먼저 읽고,
data/postings.json에서 내 이력([이력 URL 또는 파일])에 맞는 공고를 골라줘.
```
