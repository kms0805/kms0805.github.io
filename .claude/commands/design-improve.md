---
description: 페이지 디자인을 분석하고 개선안을 제시합니다. 페이지 이름을 입력하세요 (about, publications, cv)
---

# Design Improve

지정한 페이지의 디자인을 분석하고 구체적인 개선안을 제시합니다.

## Instructions

1. 대상 페이지 파악:

   - about → `_pages/about.md`, `_includes/my_education.liquid`, `_includes/my_experience.liquid`
   - publications → `_pages/publications.md`, `_includes/selected_papers.liquid`
   - cv → `_pages/cv.md`, `_data/cv.yml`
   - 미지정 → 사용자에게 어떤 페이지를 개선할지 물어봅니다

2. 해당 페이지와 관련 파일들을 읽어 현재 상태를 파악합니다

3. 디자인 개선안을 제시합니다. 고려 사항:

   - Primary color: #2698ba (sky blue)
   - al-folio 테마의 기존 CSS 클래스 활용 우선
   - 모바일 반응형 유지
   - 학술 홈페이지에 적합한 깔끔한 스타일

4. 각 개선안에 대해:

   - 무엇을 변경하는지 설명
   - 수정할 파일과 코드를 구체적으로 보여줌
   - 사용자 승인 후에만 적용

5. 디자인 개선 로드맵 (docs/plans/2026-03-02-homepage-management-design.md 참고):
   - Phase 1: About 소개문 보강, 프로필 이미지
   - Phase 2: About 구조화, CV Hobby 정리, News 개선
   - Phase 3: Projects 페이지, 다크모드, SEO

## Input

$ARGUMENTS
