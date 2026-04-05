# save-reference

> 디자인 레퍼런스 이미지를 분석하고 구조화하여 저장하는 스킬

## Trigger

다음 키워드가 포함된 사용자 요청 시 이 스킬을 활성화합니다:

- "이거 저장"
- "레퍼런스 추가"
- "이 디자인 참고"
- "save reference"

---

## 프로세스

### Step 1: 이미지 수집

이미지를 다음 방법 중 하나로 받습니다:

- 스크린샷 (클립보드에서 붙여넣기)
- 다운로드한 파일 (경로 지정)
- 채팅으로 어떤 사이트의 어떤 부분이 좋은지 설명

### Step 2: 이미지 분석

제공된 이미지를 다음 항목으로 분석합니다:

#### 2-1. 주요 색상 팔레트

```
Primary:   #XXXXXX (용도 설명)
Secondary: #XXXXXX (용도 설명)
Gray:      #XXXXXX ~ #XXXXXX (배경/텍스트 범위)
Accent:    #XXXXXX (강조 포인트)
```

- hex 코드로 추출
- 각 색상의 용도를 함께 기록

#### 2-2. 폰트 & 사이즈 추정

```
Heading:  [추정 서체], [추정 사이즈], [weight]
Body:     [추정 서체], [추정 사이즈], [weight]
Caption:  [추정 서체], [추정 사이즈], [weight]
```

- 시각적으로 가장 유사한 Google Fonts / 시스템 폰트 추정
- 상대적 사이즈 비율도 함께 기록

#### 2-3. 간격/패딩 패턴

```
Section 간격:     ~XX px (약 space-y-XX)
Card 내부 패딩:   ~XX px (약 p-XX)
요소 간 간격:     ~XX px (약 gap-XX)
```

- Tailwind 유틸리티 클래스로 매핑

#### 2-4. 주목할 컴포넌트 패턴

각 컴포넌트의 특징을 기록합니다:

- **카드**: 모서리 둥글기, 그림자, 보더 유무
- **버튼**: 스타일(filled/outlined/ghost), 크기, 아이콘 위치
- **네비게이션**: 구조(탭/사이드바/탑바), 활성 상태 표시
- **폼 요소**: 입력 필드 스타일, 라벨 위치
- **기타**: 눈에 띄는 독특한 UI 패턴

#### 2-5. 전체 느낌 요약

한 줄로 전체 디자인의 분위기를 요약합니다.

```
예시:
- "미니멀하고 여백이 넉넉한 SaaS 대시보드, 파란색 계열 모노톤"
- "다크 모드 기반 트레이딩 앱, 네온 그린 액센트로 데이터 강조"
- "따뜻한 베이지 톤의 이커머스, 둥근 카드와 부드러운 그림자"
```

### Step 3: 스크린샷 저장

`docs/references/screenshots/` 디렉토리에 서술적 파일명으로 저장합니다.

```
docs/references/screenshots/{source}-{category}-{description}.png

예시:
docs/references/screenshots/dribbble-dashboard-analytics-dark.png
docs/references/screenshots/linear-sidebar-navigation.png
docs/references/screenshots/stripe-pricing-cards.png
```

### Step 4: 분석 파일 생성

`docs/references/{source}-analysis.md` 파일을 생성합니다.

```markdown
# {Source} 디자인 분석

## 출처
- URL: (있을 경우)
- 캡처일: YYYY-MM-DD
- 카테고리: [대시보드 / 랜딩 / 모바일 / 이커머스 / ...]

## 한 줄 요약
> [전체 느낌 한 줄 요약]

## 색상 팔레트
| 용도 | Hex | 미리보기 |
|------|-----|----------|
| Primary | #XXXXXX | |
| Secondary | #XXXXXX | |
| Gray | #XXXXXX | |
| Accent | #XXXXXX | |

## 타이포그래피
- Heading: ...
- Body: ...
- Caption: ...

## 간격 패턴
- Section gap: ...
- Card padding: ...
- Element gap: ...

## 주목할 컴포넌트
- ...

## 우리 프로젝트에 적용할 포인트
- [ ] ...
```

### Step 5: design-references.md 업데이트

`docs/design-references.md`에 새 항목을 추가합니다.

```markdown
### [Source 이름] - [간단 설명]
- 스크린샷: `docs/references/screenshots/{filename}.png`
- 분석: `docs/references/{source}-analysis.md`
- 한 줄 요약: [전체 느낌 요약]
- 추가일: YYYY-MM-DD
- 태그: #dashboard #dark-mode #minimal ...
```

---

## Chrome 숏컷 프롬프트 템플릿

### "디자인 레퍼런스 수집" 숏컷

Claude in Chrome에서 사용하는 퀵 캡처 숏컷입니다.

```
[숏컷 설정]
이름: 디자인 레퍼런스 수집
단축키: (사용자 지정)

[프롬프트]
지금 보고 있는 화면을 디자인 레퍼런스로 저장해줘.
다음을 분석해서 정리해줘:
1. 주요 색상 팔레트 (hex)
2. 타이포그래피 추정
3. 간격/패딩 패턴
4. 주목할 컴포넌트 패턴
5. 전체 느낌 한 줄 요약

저장 경로: docs/references/screenshots/ 아래에 적절한 이름으로 저장
분석 파일: docs/references/ 아래에 {source}-analysis.md로 생성
```

### "이 UI로 만들어줘" 숏컷

캡처한 레퍼런스를 바탕으로 바로 UI 구현을 시작하는 숏컷입니다.

```
[숏컷 설정]
이름: 이 UI로 만들어줘
단축키: (사용자 지정)

[프롬프트]
지금 보고 있는 화면을 참고해서 우리 프로젝트의 디자인 시스템에 맞게 구현해줘.

순서:
1. 화면을 JSONC 디자인 브리프로 변환
2. docs/design-system.md의 토큰과 매핑
3. 차이가 있는 부분은 우리 디자인 시스템 기준으로 조정
4. React/HTML 코드 생성
5. 4가지 상태(로딩/에러/빈 상태/성공) 포함

출력: 단일 컴포넌트 파일 + 프리뷰 가능한 형태
```

---

## 주의 사항

- 이 스킬은 사용자가 제공한 이미지(붙여넣기, 파일 경로, 채팅 설명)만으로 동작합니다
- 저작권이 있는 이미지의 경우, 레퍼런스 용도로만 사용하고 분석 파일에 출처를 명시합니다
- 스크린샷 파일명은 영문 소문자, 하이픈 구분으로 통일합니다
- 분석 결과는 `design-preview` 스킬에서 JSONC 브리프 생성 시 참조할 수 있도록 구조화합니다
