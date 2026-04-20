# Popup PDF
## 1. 참고
- iText는 웹 브라우저 렌더링 엔진과 달리 **CSS 지원 범위가 제한적**
- 반응형 스타일(`%`, `rem`, `flex` 등) 미지원 → PDF는 PX만 지원함
- 웹 CSS 그대로 사용 시 오차, 깨짐, 비노출 문제 발생
---

## 2. 단위 변경
- `rem` 단위 → 소수점 오차로 레이아웃 깨짐 → 사용하지 않는다
- `px` 단위만 사용
- 
## 3. 스타일 속성 확인
- 사용 가능/불가능 속성 구분
- `flex`, `%`, `calc()` 등 미지원 → `inline-block`, `float`, 고정 `px`로 대체
- 반응형 관련 속성(`rem`, `%`, `flex`, `calc`, `@media`)은 **모두 제외**

## 4. 사용 가능/불가능 속성 리스트

| 구분 | 속성/패턴 | 지원 여부 | 비고 / 주의사항 |
|------|-----------|-----------|----------------|
| 레이아웃 | `flex`, `display:flex`, `flex-*` | ❌ 미지원 | 적용 시 빈 화면 출력 |
| 레이아웃 | `grid` | ⚠️ 부분 지원 | `gap` 속성은 미지원 |
| 레이아웃 | `table` | ⚠️ 부분 지원 | 기본 테이블 구조 가능, 복잡한 병합/auto 레이아웃 제약 있음 |
| 레이아웃 | `table-cell`, `vertical-align` | ❌ 미지원 | 셀 정렬 불가능 |
| 크기 단위 | `height: 100%`, `width: 100%` | ❌ 미지원 | 상위 요소 크기 인식 못함 |
| 크기 단위 | `%` 단위 (width, height 등) | ❌ 미지원 | 고정 수치 `px` 권장 |
| 크기 단위 | `rem` 단위 | ❌ 미지원 | 소수점 오차로 레이아웃 깨짐 |
| 크기 단위 | `px` 단위 | ✅ 지원 | 안정적, 권장 |
| 함수 | `calc()` | ❌ 미지원 | 단순 값만 사용 가능 |
| 선택자 | `nth-child(n)` | ⚠️ 부분 지원 | 구버전 미지원, iText9부터 지원 |
| 변환 | `transform: translate(x, y)` | ⚠️ 부분 지원 | iText9 지원, 일부 값 불안정 |
| 정렬 | `top`, `left`, `right`, `bottom` | ✅ 지원 | `translate` 대신 권장 |
| 태그/스타일 | inline 태그 + `display:block` (예: `strong {display:block}`) | ❌ 미지원 | CSS로 블록 전환 불가 → `p`, `div` 등 블록 요소로 태그 변경 필요 |
| 태그 | `input`, `script` | ❌ 미지원 | PDF 변환 시 `span`으로 자동 치환 |
| 태그 | `a` | ✅ 지원 | 링크/스타일 정상 적용 가능 |
| 기본 스타일 | `margin`, `padding` | ✅ 지원 | `px` 단위 권장 |
| 기본 스타일 | `color`, `background-color` | ✅ 지원 | 정상 적용 |
| 기본 스타일 | `font-size`, `font-weight` | ✅ 지원 | 정상 적용 |
| 레이아웃 대체 | `inline-block`, `float` | ✅ 지원 | `flex` 대체 수단 |
 
---

## 6. 추가 주의사항

- `::after`, `::before` 등 **가상 요소**에는 `left`, `right` 적용됨
- 가상 요소 외 `a`, `div` 등에는 `right` 값 적용 안됨 → `right` 대신 `left` 값으로 위치 조절

 

