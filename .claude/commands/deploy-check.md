---
description: 로컬에서 Jekyll 빌드를 테스트하여 배포 전 문제를 확인합니다
---

# Deploy Check

로컬 Jekyll 빌드를 실행하여 배포 전 문제가 없는지 확인합니다.

## Instructions

1. `bundle exec jekyll build` 실행

2. 빌드 결과 분석:
   - 성공 → "빌드 성공! push하면 자동 배포됩니다." 메시지
   - 실패 → 에러 메시지를 분석하고 해결 방법 제시

3. 일반적인 에러와 해결법:
   - BibTeX 파싱 에러 → papers.bib 문법 확인
   - Liquid 템플릿 에러 → 해당 파일의 문법 확인
   - YAML 파싱 에러 → cv.yml 등 YAML 파일 들여쓰기 확인

4. git status도 함께 확인하여 커밋되지 않은 변경사항을 알려줍니다
