# Notion API는 database 블록을 이동/삭제할 수 없다

Notion 워크스페이스를 API로 대정리하면서 알게 된 제약.

## 배경
- 2025-09-03 버전부터 Notion은 database(컨테이너)와 data source를 분리함.
- `Notion-Version` 헤더가 구버전(2022-06-28)이면 `/v1/data_sources` 엔드포인트가 전부 400 에러 → **무조건 최신 버전 쓸 것**.

## 안 되는 것 (API 자체 제약 — 어떤 MCP로도 불가)
| 작업 | 대안 |
|---|---|
| DB 블록 이동 (`move-page`는 page만 허용) | 안의 행(page)들을 하나씩 이동 |
| DB 블록 삭제 (엔드포인트 없음) | data source를 `in_trash: true` 처리 |
| 새 DB 생성 (create-a-data-source는 기존 DB용) | Create Database API 또는 앱에서 직접 |

## 함정
- **collection id ≠ database id ≠ data_source_id.** 셀 다 다른 값. relation 객체나 `retrieve-a-database`의 `data_sources[]`로 매핑 확인.
- linked database(링크된 뷰)는 `retrieve-a-database`가 거부함. 같은 collection을 보는 여러 뷰는 중복 DB가 아니라 하나의 DB를 필터링한 창문.
- page를 다른 DB로 옮기면 title은 자동 매핑되지만, 대상 DB에 없는 속성값은 사라짐 (본문은 보존).
