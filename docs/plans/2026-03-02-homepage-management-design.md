# Homepage Management with Claude Code — Design Document

**Date:** 2026-03-02
**Author:** Minsung Kim
**Status:** Approved

## Overview

SNU ECE PhD student 학술 홈페이지(al-folio 기반)를 Claude Code 워크플로우로 효율적으로 관리하기 위한 설계.

## Goals

1. **콘텐츠 업데이트 간소화** — 논문, 뉴스, CV를 자연어 명령으로 업데이트
2. **Custom Skills** — 반복 작업을 슬래시 커맨드로 자동화
3. **점진적 디자인 개선** — 대화형으로 하나씩 적용

## Part 1: CLAUDE.md 프로젝트 문서화

프로젝트 루트에 `CLAUDE.md` 작성. 포함 내용:

- **프로젝트 개요**: al-folio Jekyll 테마, GitHub Pages 배포
- **파일 구조 맵**:
  - `_bibliography/papers.bib` → 논문 목록 (BibTeX 포맷)
  - `_data/cv.yml` → CV 정보 (YAML)
  - `_news/announcement_N.md` → 뉴스 항목
  - `_pages/about.md` → 메인 페이지
  - `_pages/publications.md`, `cv.md`, `news.md` → 각 페이지
  - `_config.yml` → 사이트 설정
  - `assets/img/` → 이미지
- **콘텐츠 업데이트 규칙**:
  - papers.bib: BibTeX 엔트리 포맷, selected={true} 플래그 규칙
  - 뉴스: 파일명 규칙 (announcement_N.md), frontmatter 구조
  - CV: cv.yml 섹션 구조 (Education, Experience 등)
- **디자인 규칙**: 색상 스킴 (#2698ba sky blue), 폰트, 레이아웃 규칙
- **배포**: master push → GitHub Actions 자동 배포
- **주의사항**: _includes/, _layouts/ 수정 시 주의, bib 포맷 검증

## Part 2: Custom Skills (슬래시 커맨드)

`.claude/commands/` 디렉토리에 저장.

### /add-paper
- **입력**: arXiv ID 또는 논문 정보
- **동작**: arXiv에서 메타데이터 fetch → BibTeX 생성 → papers.bib에 추가
- **확인**: selected 여부, 저널/학회 정보

### /add-news
- **입력**: 뉴스 내용
- **동작**: 다음 번호의 announcement 파일 생성, 날짜 자동 설정

### /update-cv
- **입력**: 없음 (대화형)
- **동작**: 현재 CV 보여주고 어떤 섹션을 수정할지 대화로 안내

### /deploy-check
- **입력**: 없음
- **동작**: `bundle exec jekyll build` 로컬 빌드 테스트, 에러 리포트

### /design-improve
- **입력**: 페이지 이름 (about, publications, cv 등)
- **동작**: 해당 페이지 분석 후 구체적 개선안 제시, 승인 시 적용

## Part 3: 디자인 개선 로드맵

### Phase 1 (우선순위 높음)
1. About 페이지 소개문 보강 — 연구 관심사, 키워드, 최근 성과 추가
2. 프로필 이미지 — 원형 여부 선택

### Phase 2 (우선순위 중간)
3. About 페이지 구조화 — Research Interests 섹션 분리
4. CV Hobby 섹션 정리 또는 제거
5. News 섹션 타임라인 개선

### Phase 3 (우선순위 낮음)
6. Projects 페이지 추가
7. 다크모드 토글 활성화
8. SEO (Open Graph 메타 태그) 활성화

## Technical Notes

- **스택**: Jekyll + al-folio theme + GitHub Pages
- **배포**: GitHub Actions (`.github/workflows/deploy.yml`)
- **언어/템플릿**: Liquid, SCSS, YAML, BibTeX
- **Ruby 버전**: 3.2.2
