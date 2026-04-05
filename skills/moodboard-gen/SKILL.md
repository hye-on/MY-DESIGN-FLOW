# moodboard

3~4가지 디자인 방향을 실제 코드 시안으로 생성하여 비교 선택하게 하는 스킬

## 선택적 도구
| 도구 | 용도 | 없을 때 |
|------|------|---------|
| Playwright | 생성된 시안 자동 스크린샷 비교 | 사용자가 브라우저에서 직접 비교 |
| Figma MCP | Figma에 시안 직접 생성 | HTML 파일로 preview/ 폴더에 생성 (기본 동작) |

## Triggers

- "무드보드"
- "디자인 방향"
- "톤앤매너"
- "moodboard"

---

## 인터랙션 흐름 (반드시 이 순서대로)

이 스킬은 4단계의 구체적인 인터랙션 흐름을 가진다.
각 단계를 건너뛰지 말고 순서대로 실행한다.

---

### Step 1: 시안 준비 시작 + 사용자 레퍼런스 수집 병행

사용자에게 다음 메시지를 먼저 보여준다:

```
3가지 디자인 방향을 준비할게요. (약 2~3분)

그 사이에 마음에 드는 디자인 레퍼런스가 있다면
스크린샷을 여기에 붙여주세요.
없으면 그냥 기다리시면 됩니다!

💡 팁: "이 색감이 좋아", "이 레이아웃 느낌" 같은
부분적인 레퍼런스도 좋아요.
```

핵심: 사용자가 기다리는 시간을 레퍼런스 수집에 활용한다.
부담 없는 톤 유지 — "없으면 안하셔도 됩니다".

---

### Step 2: 3개 시안을 각각 별도 폴더에 생성

**dispatching-parallel-agents 패턴을 사용하여 option-a, option-b, option-c를 병렬 생성한다.**

#### 생성할 폴더 구조

```
preview/
├── option-a/    ← 방향 A (예: 미니멀 클린)
│   └── index.html
├── option-b/    ← 방향 B (예: 따뜻한 소프트)
│   └── index.html
└── option-c/    ← 방향 C (예: 볼드 모던)
    └── index.html
```

#### 각 option 생성 시 subagent에게 전달할 컨텍스트

```markdown
## 공통 컨텍스트 (모든 subagent에 전달)

### 프로젝트 정보
- 서비스명: {PRD/프로젝트에서 추출}
- 서비스 유형: {SaaS, 이커머스, 소셜 등}
- 타겟 사용자: {PRD에서 추출}
- 핵심 기능: {PRD에서 추출}

### 기존 레퍼런스
- docs/design-references.md 내용 요약
- docs/references/ 분석 결과 요약

### 필수 포함 섹션 (각 index.html에 포함)
1. 네비게이션 바 (로고 + 메뉴)
2. 히어로 섹션 (메인 메시지 + CTA)
3. 핵심 기능/제품 카드 그리드 (3~4개)
4. 푸터
```

#### 방향별 subagent 지시

```markdown
## Subagent A: 미니멀 클린

preview/option-a/index.html을 생성하라.

디자인 방향:
- 넓은 여백, 깔끔한 라인, 최소한의 장식
- 색상: 모노크롬 베이스 + 단일 포인트 컬러
- 타이포: 가벼운 산세리프, 대비 약하게
- 간격: 넉넉한 padding, 섹션 간 큰 여백
- 그림자: 거의 없음, 미세한 보더로 구분
- 느낌: Notion, Linear, Stripe 계열

포함: {공통 컨텍스트의 필수 포함 섹션}
반드시 standalone HTML로 만들 것 — CDN 링크(Tailwind, Google Fonts)를 사용하여
별도 빌드 없이 브라우저에서 바로 열 수 있어야 한다.
```

```markdown
## Subagent B: 따뜻한 소프트

preview/option-b/index.html을 생성하라.

디자인 방향:
- 둥근 모서리, 부드러운 그림자, 따뜻한 색감
- 색상: 웜 뉴트럴 + 파스텔 계열 Primary
- 타이포: Rounded 또는 친근한 산세리프, 적당한 무게
- 간격: 적당한 padding, 요소 간 여유
- 그림자: 소프트 쉐도우 (넓고 흐린)
- 느낌: Airbnb, Headspace, Calm 계열

포함: {공통 컨텍스트의 필수 포함 섹션}
반드시 standalone HTML로 만들 것 — CDN 링크(Tailwind, Google Fonts)를 사용하여
별도 빌드 없이 브라우저에서 바로 열 수 있어야 한다.
```

```markdown
## Subagent C: 볼드 모던

preview/option-c/index.html을 생성하라.

디자인 방향:
- 강한 대비, 굵은 타이포, 대담한 색상
- 색상: 진한 Primary + 비비드 Accent, 다크 배경도 활용
- 타이포: 굵은 산세리프 또는 기하학적 폰트, 큰 제목
- 간격: 타이트한 그리드, 밀도 높은 레이아웃
- 그림자: 선명한 그림자 또는 하드 보더
- 느낌: Vercel, Figma, Arc Browser 계열

포함: {공통 컨텍스트의 필수 포함 섹션}
반드시 standalone HTML로 만들 것 — CDN 링크(Tailwind, Google Fonts)를 사용하여
별도 빌드 없이 브라우저에서 바로 열 수 있어야 한다.
```

#### 각 index.html의 공통 요구사항

```markdown
## HTML 파일 공통 규칙

1. Standalone — 외부 파일 참조 없이 단독 실행 가능
2. CDN 사용:
   - Tailwind CSS: <script src="https://cdn.tailwindcss.com"></script>
   - Google Fonts: <link> 태그로 사용할 폰트 로드
3. 인라인 <style> 태그로 커스텀 CSS 변수 정의
4. 반응형: 모바일 ~ 데스크톱 대응
5. 다크모드: prefers-color-scheme 또는 토글 버튼
6. 실제적인 더미 텍스트 (Lorem ipsum 금지 — 서비스에 맞는 현실적인 텍스트)
7. 마이크로 인터랙션: hover 효과, 트랜지션
8. 파일 상단에 주석으로 방향 설명:
   <!-- 방향 A: 미니멀 클린 — 넓은 여백과 깔끔한 라인 -->
```

---

### Step 3: 사용자 레퍼런스 반영 (option-d)

Step 2를 진행하는 동안 또는 완료 후, 사용자가 레퍼런스 이미지를 첨부했는지 확인한다.

#### 레퍼런스가 있는 경우

```markdown
## option-d 생성 조건

사용자가 레퍼런스 이미지를 1장 이상 첨부한 경우:

1. 이미지 분석:
   - 색상 팔레트 추출
   - 타이포그래피 스타일 추정
   - 레이아웃 구조 파악
   - 전체 분위기 한줄 요약

2. 여러 장인 경우:
   - 공통 요소를 추출 (색감, 레이아웃 패턴, 분위기)
   - 하나의 통합된 방향으로 정리

3. preview/option-d/index.html 생성:
   - 추출된 특성을 기반으로 시안 생성
   - 동일한 섹션 구조 (네비게이션, 히어로, 카드 그리드, 푸터)

4. 메시지:
   "올려주신 레퍼런스 기반으로 Option D도 만들었어요!
   총 4가지 방향을 비교해보세요."
```

#### 레퍼런스가 없는 경우

option-d는 생략하고 3개만 제시한다. 별도 언급 불필요.

---

### Step 4: 선택 & 정리

3~4개의 시안이 모두 준비되면 사용자에게 비교 선택을 요청한다.

#### 제시 메시지

```
시안이 준비되었습니다!

Playwright가 설치되어 있으면 자동으로 스크린샷을 캡처하여 비교 이미지를 생성합니다.
설치되어 있지 않으면 아래 안내대로 브라우저에서 직접 비교해보세요.

어떻게 할까요?
1️⃣ 지금 Playwright를 설치하고 자동 스크린샷 비교하기 (설치 명령어)
2️⃣ 브라우저에서 직접 열어서 비교할게요
3️⃣ 채팅으로 각 시안의 특징을 설명해주면 골라볼게요

각 폴더를 브라우저에서 열어 비교해보세요.

📁 preview/option-a/index.html — 미니멀 클린
   넓은 여백, 깔끔한 라인, Notion/Linear 느낌

📁 preview/option-b/index.html — 따뜻한 소프트
   둥근 모서리, 부드러운 그림자, Airbnb/Headspace 느낌

📁 preview/option-c/index.html — 볼드 모던
   강한 대비, 굵은 타이포, Vercel/Figma 느낌

{option-d가 있으면}
📁 preview/option-d/index.html — 사용자 레퍼런스 기반
   올려주신 레퍼런스에서 추출한 느낌

마음에 드는 방향을 골라주세요!
"B가 좋아", "A의 색감 + C의 레이아웃" 등 자유롭게 알려주세요.
```

#### 사용자 선택 처리

```markdown
## 선택 후 처리 흐름

1. 사용자가 하나를 선택 (예: "B로 할게")

2. 확인:
   "Option B (따뜻한 소프트)를 선택하셨네요!
   나머지 Option A, C 폴더를 삭제할까요?"

3. 사용자 동의 후:
   - preview/option-a/ 삭제
   - preview/option-c/ 삭제
   - (option-d가 있었다면 option-d/ 삭제)
   - preview/option-b/ 유지 → 메인 디자인 베이스로 승격

4. 선택된 방향의 디자인 토큰 추출:
   - option-b/index.html에서 사용된 색상, 폰트, 간격 추출
   - docs/design-system.md에 반영 (없으면 생성, 있으면 업데이트)

5. 완료 메시지:
   "Option B를 메인 디자인 베이스로 설정했습니다!
   디자인 토큰이 docs/design-system.md에 반영되었어요.

   다음 단계:
   → /design-system-gen 으로 디자인 시스템을 더 정교하게 다듬기
   → /design-preview 로 핵심 화면 프리뷰 만들기"
```

#### 조합 선택 처리

```markdown
## 사용자가 조합을 원하는 경우 (예: "A의 색감 + C의 레이아웃")

1. 각 option에서 요청된 요소를 추출
2. 새로운 preview/option-final/index.html 생성
3. 사용자에게 확인 요청
4. 확인 후 나머지 삭제, option-final을 메인 베이스로 승격
```

---

## 방향 커스터마이징

위의 3가지 방향(미니멀 클린 / 따뜻한 소프트 / 볼드 모던)은 기본값이다.
다음 경우에 방향을 조정한다:

```markdown
## 방향 조정 기준

### PRD에 디자인 힌트가 있는 경우
- "럭셔리 느낌" → 방향 중 하나를 럭셔리로 교체
- "게임 같은 느낌" → 방향 중 하나를 게이미피케이션으로 교체
- "10대 타겟" → 더 playful한 방향 추가

### 레퍼런스가 한 방향으로 치우친 경우
- 레퍼런스가 모두 미니멀 → 미니멀 변주 2개 + 대조적 1개
- 다양한 레퍼런스 → 기본 3방향 유지

### 서비스 특성에 따라
- B2B SaaS → 프로페셔널/깔끔 방향 강화
- 소비자 앱 → 친근/감성 방향 강화
- 크리에이터 도구 → 볼드/트렌디 방향 강화
```

---

## 기술적 참고

### Parallel Agent 디스패치

3개의 시안을 동시에 생성하기 위해 dispatching-parallel-agents 패턴을 사용한다:

```markdown
## 병렬 에이전트 구성

Agent 1: option-a 생성
- 작업: preview/option-a/index.html 작성
- 의존성: 없음 (독립 실행)

Agent 2: option-b 생성
- 작업: preview/option-b/index.html 작성
- 의존성: 없음 (독립 실행)

Agent 3: option-c 생성
- 작업: preview/option-c/index.html 작성
- 의존성: 없음 (독립 실행)

각 에이전트는 공통 컨텍스트를 받되, 방향별 디자인 지시만 다르다.
3개가 모두 완료되면 Step 3으로 진행.
```

### option-d는 순차 실행

option-d는 사용자 레퍼런스 분석이 선행되어야 하므로 병렬이 아닌 순차 실행.
Step 2의 3개 시안 완료 후, 레퍼런스가 있으면 추가 생성한다.
