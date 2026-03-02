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
