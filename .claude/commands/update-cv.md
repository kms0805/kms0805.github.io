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
