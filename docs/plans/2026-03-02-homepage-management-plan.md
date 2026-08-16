# Homepage Management with Claude Code — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Claude Code를 활용하여 al-folio 학술 홈페이지를 자연어 명령으로 관리할 수 있는 워크플로우 구축

**Architecture:** 프로젝트 루트에 CLAUDE.md로 프로젝트 컨텍스트를 문서화하고, `.claude/commands/`에 반복 작업용 슬래시 커맨드(skills)를 생성. 기존 al-folio 테마 구조를 그대로 유지하며 관리 편의성만 추가.

**Tech Stack:** Jekyll, al-folio theme, Liquid, YAML, BibTeX, GitHub Pages, Claude Code Skills

---

### Task 1: CLAUDE.md 작성

**Files:**
- Create: `CLAUDE.md`

**Step 1: CLAUDE.md 파일 생성**

프로젝트 루트에 아래 내용으로 `CLAUDE.md`를 작성한다:

```markdown
# Minsung Kim's Academic Homepage

## Overview
al-folio Jekyll 테마 기반 학술 홈페이지. GitHub Pages로 자동 배포.
- URL: https://kms0805.github.io
- 소유자: Minsung Kim (SNU ECE M.S/Ph.D.)

## File Structure

### Content Files (자주 수정)
- `_bibliography/papers.bib` — 논문 목록 (BibTeX). `selected={true}`로 메인 페이지 노출 제어
- `_data/cv.yml` — CV 정보 (Education, Experience, Hobby 섹션)
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
- Primary color: #2698ba (sky blue)
- 프로필 이미지: 현재 사각형 (image_circular: false)
- Font: Arial, sans-serif (소개문 제목)

## Deployment
- master branch push → GitHub Actions 자동 배포
- Workflow: `.github/workflows/deploy.yml`
- 로컬 빌드 테스트: `bundle exec jekyll build`
- 로컬 서버: `bundle exec jekyll serve`

## Important Notes
- `_includes/`, `_layouts/` 파일은 al-folio 테마 핵심 파일. 수정 시 영향 범위 확인 필요
- papers.bib 상단의 `---\n---`는 Jekyll frontmatter. 삭제하지 말 것
- 커밋 후 배포까지 약 2~3분 소요
```

**Step 2: Commit**

```bash
git add CLAUDE.md
git commit -m "docs: add CLAUDE.md for Claude Code homepage management"
```

---

### Task 2: /add-paper 스킬 생성

**Files:**
- Create: `.claude/commands/add-paper.md`

**Step 1: commands 디렉토리 생성 및 스킬 작성**

```markdown
---
description: 논문을 papers.bib에 추가합니다. arXiv ID, 논문 제목, 또는 BibTeX를 입력하세요.
---

# Add Paper

사용자가 제공한 정보를 바탕으로 `_bibliography/papers.bib`에 논문을 추가합니다.

## Instructions

1. 사용자 입력 분석:
   - arXiv ID가 주어지면 (예: 2510.02370) → `https://arxiv.org/abs/{ID}` 에서 WebFetch로 제목/저자/연도 정보를 가져옵니다
   - 논문 제목이 주어지면 → 사용자에게 저자, 연도, 학회, arXiv ID를 확인합니다
   - BibTeX가 직접 주어지면 → 포맷 검증 후 추가

2. BibTeX 엔트리 생성:
   - 기존 `_bibliography/papers.bib`를 읽어 포맷 스타일을 확인합니다
   - 엔트리 키: `lastname+year+keyword` (예: kim2025trainingdynamics)
   - 필수 필드: title, author, year, month
   - 선택 필드: journal/booktitle, arxiv, pdf, code, selected

3. 사용자에게 확인:
   - 생성된 BibTeX 엔트리를 보여주고 확인을 받습니다
   - `selected={true}`로 설정할지 물어봅니다

4. papers.bib 파일 끝 (마지막 `}` 뒤, 빈 줄 전)에 새 엔트리를 추가합니다

5. 뉴스도 함께 추가할지 물어봅니다 → "예"이면 /add-news 워크플로우를 안내합니다

## Input
$ARGUMENTS
```

**Step 2: Commit**

```bash
git add .claude/commands/add-paper.md
git commit -m "feat: add /add-paper skill for managing publications"
```

---

### Task 3: /add-news 스킬 생성

**Files:**
- Create: `.claude/commands/add-news.md`

**Step 1: 스킬 파일 작성**

```markdown
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
```

**Step 2: Commit**

```bash
git add .claude/commands/add-news.md
git commit -m "feat: add /add-news skill for news announcements"
```

---

### Task 4: /update-cv 스킬 생성

**Files:**
- Create: `.claude/commands/update-cv.md`

**Step 1: 스킬 파일 작성**

```markdown
---
description: CV를 업데이트합니다. 수정할 내용을 설명하세요 (예: "인턴 경력 추가")
---

# Update CV

`_data/cv.yml` 파일을 수정하여 CV를 업데이트합니다.

## Instructions

1. 먼저 `_data/cv.yml`의 현재 내용을 읽어 사용자에게 보여줍니다

2. 사용자 입력에 따라 수정할 섹션을 판단합니다:
   - Education → `time_table` 형식으로 항목 추가/수정
   - Experience → `time_table` 형식으로 항목 추가/수정
   - 새 섹션 → 기존 섹션 구조를 참고하여 추가

3. `time_table` 항목 구조:
   ```yaml
   - title: [직함/학위]
     institution: [기관명]
     year: [기간]
     logo: [로고파일명.png]  # assets/img/ 에 저장
     department: [부서/학과]  # 선택
     maindescription: [상세 기간]  # 선택
   ```

4. 로고 이미지가 필요하면 사용자에게 파일을 요청하거나 assets/img/에 이미 있는지 확인합니다

5. 수정 내용을 보여주고 확인을 받은 후 적용합니다

## Input
$ARGUMENTS
```

**Step 2: Commit**

```bash
git add .claude/commands/update-cv.md
git commit -m "feat: add /update-cv skill for CV management"
```

---

### Task 5: /deploy-check 스킬 생성

**Files:**
- Create: `.claude/commands/deploy-check.md`

**Step 1: 스킬 파일 작성**

```markdown
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
```

**Step 2: Commit**

```bash
git add .claude/commands/deploy-check.md
git commit -m "feat: add /deploy-check skill for build testing"
```

---

### Task 6: /design-improve 스킬 생성

**Files:**
- Create: `.claude/commands/design-improve.md`

**Step 1: 스킬 파일 작성**

```markdown
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
```

**Step 2: Commit**

```bash
git add .claude/commands/design-improve.md
git commit -m "feat: add /design-improve skill for design improvements"
```

---

### Task 7: 전체 통합 테스트 및 최종 커밋

**Step 1: 모든 파일이 올바르게 생성되었는지 확인**

```bash
cat CLAUDE.md
ls .claude/commands/
cat .claude/commands/add-paper.md
cat .claude/commands/add-news.md
cat .claude/commands/update-cv.md
cat .claude/commands/deploy-check.md
cat .claude/commands/design-improve.md
```

**Step 2: git status로 모든 변경사항 확인**

```bash
git status
git log --oneline -7
```

모든 파일이 커밋되었는지 확인. 빠진 파일이 있으면 추가 커밋.
