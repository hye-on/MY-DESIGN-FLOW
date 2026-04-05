# design-feedback

> 사용자 피드백을 디자인 토큰 레벨로 번역하여 반영하고 DESIGN.md를 업데이트하는 스킬

## Trigger

다음 키워드가 포함된 사용자 요청 시 이 스킬을 활성화합니다:

- "피드백"
- "수정해줘"
- "이 부분 바꿔줘"
- "design feedback"

---

## 프로세스

### Step 1: 사용자 피드백 파싱

사용자의 자연어 피드백을 수집하고 분석합니다.

- 피드백 대상 화면/컴포넌트 식별
- 변경 의도 파악 (색상, 크기, 간격, 레이아웃, 분위기 등)
- 모호한 표현은 구체적 질문으로 명확화

### Step 2: 디자인 토큰 레벨로 번역

자연어 피드백을 디자인 토큰 변경 사항으로 변환합니다.

#### 번역 매핑 예시

| 사용자 피드백 | 토큰 카테고리 | 변경 내용 |
|---|---|---|
| "좀 더 따뜻한 느낌" | `color.primary`, `color.background` | 색온도를 따뜻한 톤으로 조정 (blue → warm blue, 배경에 미세한 warm tint 추가) |
| "글자가 작아" | `typography.fontSize` | 본문 기본 사이즈 업 (text-sm → text-base 또는 text-base → text-lg) |
| "간격이 너무 좁아" | `spacing.gap`, `spacing.padding` | 간격 스케일 한 단계 증가 (space-y-4 → space-y-6, p-4 → p-6) |
| "너무 딱딱해" | `borderRadius`, `shadow` | border-radius 증가 (rounded-md → rounded-xl), 그림자 부드럽게 |
| "버튼이 눈에 안 띄어" | `color.primary`, `typography.fontWeight` | 버튼 배경색 채도 증가, font-weight 올리기 (font-medium → font-semibold) |
| "전체적으로 어두워" | `color.background`, `color.surface` | 배경색 밝기 증가, 대비 조정 |
| "좀 더 고급스러운 느낌" | `color.palette`, `typography.fontFamily`, `spacing` | 채도 낮추기, 서체 변경, 여백 넉넉히 |

### Step 3: UI 코드에 변경 사항 적용

번역된 토큰 변경 사항을 실제 코드에 반영합니다.

- 해당 컴포넌트 파일을 찾아 수정
- Tailwind 클래스 또는 CSS 변수 업데이트
- 다크 모드도 함께 조정

### Step 4: design-system.md 업데이트

변경된 토큰을 `docs/design-system.md`에 반영합니다.

```markdown
### 변경 이력
- [YYYY-MM-DD] 피드백 반영: [변경 요약]
  - Before: `color.primary: #2563EB`
  - After: `color.primary: #3B82F6`
  - 사유: "좀 더 따뜻한 느낌" 피드백
```

### Step 5: Before/After 비교 제공

사용자에게 변경 전후를 시각적으로 비교할 수 있도록 제시합니다.

```
## 변경 요약

### Before
- Primary: #2563EB (차가운 파랑)
- Font size (body): text-sm (14px)
- Card gap: space-y-4 (16px)

### After
- Primary: #3B82F6 (밝은 파랑, 따뜻한 톤)
- Font size (body): text-base (16px)
- Card gap: space-y-6 (24px)

### 영향 범위
- 수정된 파일: 3개
- 영향받는 화면: 대시보드, 설정 페이지
```

### Step 6: Knowledge Base 파일 업데이트

변경 사항을 다음 Knowledge Base 파일들에 기록합니다:

- **`docs/knowledge-base/visual-language.md`**: 현재 비주얼 랭귀지 상태 반영
- **`docs/knowledge-base/token-audit.md`**: 토큰 변경 감사 기록 추가
- **`docs/knowledge-base/_sync-log.md`**: 동기화 로그에 타임스탬프와 변경 내역 기록

---

## 피드백 품질 가이드

효과적인 피드백은 구체적이고 토큰 레벨에 가까울수록 정확하게 반영됩니다.

### 나쁜 예 vs 좋은 예

| 나쁜 예 | 좋은 예 |
|---------|---------|
| "이쁘게 만들어줘" | "카드 간 간격을 space-y-4에서 space-y-6으로, 제목을 font-semibold에서 font-bold로" |
| "색깔 좀 바꿔" | "Primary 색상을 좀 더 따뜻한 톤으로, 현재 #2563EB에서 채도를 약간 낮춰줘" |
| "다시 해줘" | "헤더 영역 패딩을 py-4에서 py-8로 늘리고, 로고 옆 간격을 ml-2에서 ml-4로" |
| "느낌이 별로야" | "전체적으로 border-radius를 rounded-md에서 rounded-xl로, 그림자를 shadow-sm에서 shadow-md로" |
| "앱처럼 만들어" | "하단에 fixed 네비게이션 바 추가, 카드에 터치 피드백(active:scale-95) 넣어줘" |

### 모호한 피드백을 받았을 때

사용자가 모호한 피드백을 제공하면, 다음과 같이 구체화 질문을 합니다:

```
사용자: "좀 더 세련되게"

응답: "세련된 느낌을 위해 몇 가지 방향이 있습니다:
1. 색상 채도 낮추기 (현재 팔레트를 muted 톤으로)
2. 여백 늘리기 (컨텐츠 간 간격을 1.5배)
3. 서체 변경 (현재 sans → serif 또는 geometric sans)
4. 그림자/보더 정리 (경계선을 최소화하고 subtle shadow로)

어떤 방향이 가장 가까운가요? 또는 레퍼런스 이미지가 있으면 공유해주세요."
```

---

## 주의 사항

- 토큰 변경 시 영향 범위를 반드시 확인합니다 (하나의 토큰이 여러 화면에 영향)
- Before/After를 항상 기록하여 롤백이 가능하도록 합니다
- 다크 모드 토큰도 함께 업데이트합니다
- `_sync-log.md`에 모든 변경을 타임스탬프와 함께 기록합니다
