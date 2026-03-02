---
description: 뉴스/공지를 추가합니다. 내용을 입력하세요 (예: "ICLR 2026 accepted!")
---

# Add News

`_news/` 디렉토리에 새 뉴스 항목을 추가합니다.

## Instructions

1. `_news/` 디렉토리의 기존 파일을 확인하여 다음 순번(N)을 결정합니다
   - 파일명 패턴: `announcement_N.md`
   - 현재 최대 번호 + 1 사용

2. 사용자 입력에서 뉴스 내용을 파악합니다:
   - 논문 acceptance → `Our [Paper](url) paper is accepted to **Venue Year**!` 형태
   - 수상 → `Received **Award Name** at Event!` 형태
   - 기타 → 사용자 입력 그대로

3. 날짜 확인:
   - 기본값: 오늘 날짜
   - 사용자가 다른 날짜를 지정하면 해당 날짜 사용

4. 새 파일 `_news/announcement_N.md` 생성:
   ```
   ---
   layout: post
   date: YYYY-MM-DD
   inline: true
   related_posts: false
   ---

   [뉴스 내용]
   ```

## Input
$ARGUMENTS
