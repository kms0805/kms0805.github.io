# Minsung Kim's Academic Homepage

## Overview
al-folio Jekyll 테마 기반 학술 홈페이지. GitHub Pages로 자동 배포.
- URL: https://kms0805.github.io
- 소유자: Minsung Kim (SNU ECE M.S/Ph.D.)

## File Structure

### Content Files (자주 수정)
- `_bibliography/papers.bib` — 논문 목록 (BibTeX). `selected={true}`로 메인 페이지 노출 제어
- `_data/cv.yml` — CV 정보 (Education, Experience 섹션)
- `_news/announcement_N.md` — 뉴스/공지. N은 순번 (현재 1~7)
- `_pages/about.md` — 메인 페이지 (프로필, 소개문)

### Page Files
- `_pages/publications.md` — 논문 목록 페이지
- `_pages/cv.md` — CV 페이지
- `_pages/news.md` — 뉴스 아카이브 페이지

### Config & Assets
- `_config.yml` — 사이트 전역 설정 (이름, 소셜 링크 등)
- `assets/img/` — 이미지 파일 (프로필: winter_pro.png)
- `_sass/` — SCSS 스타일시트
- `_includes/` — Liquid 파셜 템플릿 (수정 시 주의)
- `_layouts/` — 페이지 레이아웃 (수정 시 주의)

## Content Update Rules

### Adding a Paper (papers.bib)
- BibTeX 포맷 사용
- 필수 필드: title, author, year, month
- `selected={true}` → 메인 페이지 "Selected Publications"에 노출
- `arxiv={ID}` → arXiv 링크 자동 생성
- `pdf={URL}` → PDF 링크
- `journal` 또는 `booktitle` → 학회/저널명 (예: "ICLR 2026", "EMNLP 2025")
- author 포맷: "Last, First" 또는 "First Last" (기존 엔트리 스타일 따를 것)
- 엔트리 키 포맷: `lastname+year+keyword` (예: kim2025trainingdynamics)

### Adding News (_news/)
- 파일명: `announcement_N.md` (N = 다음 순번)
- Frontmatter:
  ```yaml
  ---
  layout: post
  date: YYYY-MM-DD
  inline: true
  related_posts: false
  ---
  ```
- 본문: 마크다운. 논문 링크는 `[Title](URL)` 형태, 학회는 `**bold**`
- 예시: `Our [Paper Title](arxiv-url) paper is accepted to **VENUE YEAR**!`

### Updating CV (_data/cv.yml)
- 섹션 타입: `map` (key-value), `time_table` (시간순 항목), `nested_list`
- Education/Experience는 `time_table` 타입
- 각 항목에 `logo` 필드로 기관 로고 지정 가능 (assets/img/에 저장)

## Design Rules
- Primary color: #3d8eb0 (Vivid Dolphin ocean blue, `$cyan-color` in `_sass/_variables.scss`)
- Palette: Vivid Dolphin — surface #52a8cc, deep navy #1e5a78, spray aqua #6dc0d8
- 프로필 이미지: coin flip (앞면 프로필 사진, 뒷면 MIL 로고, `_pages/about.md`에서 `image_circular: true`)
- Font: Source Sans 3 (body + headings)

## Deployment
- master branch push → GitHub Actions 자동 배포
- Workflow: `.github/workflows/deploy.yml`
- 로컬 빌드 테스트: `bundle exec jekyll build`
- 로컬 서버: `bundle exec jekyll serve`

## Important Notes
- `_includes/`, `_layouts/` 파일은 al-folio 테마 핵심 파일. 수정 시 영향 범위 확인 필요
- papers.bib 상단의 `---\n---`는 Jekyll frontmatter. 삭제하지 말 것
- 커밋 후 배포까지 약 2~3분 소요

## Custom Skills
이 프로젝트에는 `.claude/commands/`에 다음 스킬이 있습니다:
- `/add-paper` — 논문 추가 (arXiv ID 또는 논문 정보)
- `/add-news` — 뉴스/공지 추가
- `/update-cv` — CV 업데이트 (대화형)
- `/deploy-check` — 로컬 빌드 테스트
- `/design-improve` — 페이지 디자인 개선안 제시
