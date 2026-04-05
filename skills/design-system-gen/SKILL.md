# design-system-gen

수집된 레퍼런스를 기반으로 DESIGN.md(디자인 시스템) 초안을 생성하는 스킬

## 선택적 도구
| 도구 | 용도 | 없을 때 |
|------|------|---------|
| typeui.sh 디자인 스킬 | 48종 프리셋 디자인 스타일에서 선택 → 즉시 적용 | 수집된 레퍼런스 기반으로 직접 DESIGN.md 작성 |

## Triggers

- "디자인 시스템 만들어줘"
- "DESIGN.md 생성"
- "design system"

---

## 실행 단계

### Step 1: 기존 레퍼런스 분석 결과 읽기

```markdown
## 읽어야 할 파일

1. docs/design-references.md
   → 수집된 레퍼런스 종합 정리, 공통 패턴, 키워드

2. docs/references/ 내 분석 파일들 (*-analysis.md)
   → 각 레퍼런스별 색상/타이포/레이아웃 분석 결과

3. docs/references/screenshots/ 내 이미지
   → 직접 이미지를 읽어서 시각적 패턴 재확인

4. PRD 또는 프로젝트 설명 (있는 경우)
   → 서비스 특성, 타겟 사용자 정보
```

### Step 1.5: 프리셋 스타일 확인 (선택적)

typeui.sh 디자인 스킬이 설치되어 있는지 확인한다.

- **설치되어 있으면**: 48종 프리셋 디자인 스타일 목록을 보여주고 사용자가 선택하면 즉시 적용한다.
- **설치되어 있지 않으면**: 다음 메시지를 표시한다:

```
프리셋 디자인 스타일을 적용하려면 typeui.sh 스킬이 필요한데 설치되어 있지 않아요.

어떻게 할까요?
1️⃣ 지금 설치하고 프리셋 스타일 사용하기 (설치 명령어)
2️⃣ DESIGN.md를 직접 작성할게요 (수집된 레퍼런스 기반으로)
3️⃣ 채팅으로 원하는 디자인 톤을 말해주면 직접 만들게요
```

사용자가 2️⃣ 또는 3️⃣을 선택하면 typeui.sh 없이 다음 단계로 진행한다.

### Step 2: 3가지 디자인 방향 제안

레퍼런스 분석 결과와 사용자 선호를 기반으로 3가지 방향을 제안한다.
각 방향은 명확히 다른 느낌이어야 한다.

```markdown
## 제안 형식

### 🅰 방향 A: {한줄 네이밍} (예: "미니멀 클린")

**느낌 한줄 요약**: {예: "넓은 여백과 깔끔한 라인으로 신뢰감을 주는 프로페셔널한 느낌"}

**색상 팔레트**:
| 역할 | 색상 | Hex | Tailwind |
|------|------|-----|----------|
| Primary | {색상명} | #XXXXXX | {예: blue-600} |
| Secondary | {색상명} | #XXXXXX | {예: slate-700} |
| Gray Scale | {범위} | #XXX-#XXX | {예: gray-50~gray-900} |
| Accent | {색상명} | #XXXXXX | {예: amber-500} |
| Background | {색상명} | #XXXXXX | {예: white} |
| Surface | {색상명} | #XXXXXX | {예: gray-50} |

**타이포그래피**:
- 제목: {폰트명} / {weight} / {크기 범위}
- 본문: Inter / Regular / 14-16px
- 특징: {예: "제목과 본문의 대비가 강한", "모노스페이스 악센트"}

**특징 키워드**: {예: #여백 #라인 #신뢰감 #프로}

---

### 🅱 방향 B: {한줄 네이밍} (예: "따뜻한 소프트")
(동일 형식)

---

### 🅲 방향 C: {한줄 네이밍} (예: "볼드 모던")
(동일 형식)
```

### Step 3: 사용자 선택 대기

```
위 3가지 방향 중 마음에 드는 것을 골라주세요!

- 완전히 하나를 선택해도 좋고
- "A의 색상 + B의 타이포" 같은 조합도 가능합니다
- 추가로 원하는 느낌이 있으면 자유롭게 알려주세요

어떤 방향으로 갈까요?
```

### Step 4: 선택된 방향으로 docs/design-system.md 생성

사용자가 방향을 선택하면, 해당 방향의 토큰을 기반으로 완전한 디자인 시스템 문서를 생성한다.

---

## design-system.md 템플릿 구조

아래는 생성될 `docs/design-system.md`의 전체 구조이다.

```markdown
# Design System

> 이 프로젝트의 디자인 토큰 & 규칙을 정의합니다.
> 모든 UI 생성/수정 시 이 문서를 참조하세요.
> 생성일: {날짜} | 기반 방향: {선택된 방향 이름}

---

## 1. 색상 팔레트

### Light Mode

| 역할 | 변수명 | 값 | Tailwind Class |
|------|--------|-----|----------------|
| Primary | --primary | #XXXXXX | bg-primary, text-primary |
| Primary Foreground | --primary-foreground | #XXXXXX | text-primary-foreground |
| Secondary | --secondary | #XXXXXX | bg-secondary |
| Secondary Foreground | --secondary-foreground | #XXXXXX | text-secondary-foreground |
| Background | --background | #XXXXXX | bg-background |
| Foreground | --foreground | #XXXXXX | text-foreground |
| Muted | --muted | #XXXXXX | bg-muted |
| Muted Foreground | --muted-foreground | #XXXXXX | text-muted-foreground |
| Accent | --accent | #XXXXXX | bg-accent |
| Accent Foreground | --accent-foreground | #XXXXXX | text-accent-foreground |
| Card | --card | #XXXXXX | bg-card |
| Card Foreground | --card-foreground | #XXXXXX | text-card-foreground |
| Border | --border | #XXXXXX | border |
| Ring | --ring | #XXXXXX | ring |
| Destructive | --destructive | #XXXXXX | bg-destructive |
| Success | --success | #XXXXXX | (커스텀) |
| Warning | --warning | #XXXXXX | (커스텀) |

### Dark Mode

(동일 구조, 다크모드 값으로 채움)

### shadcn/ui CSS 변수 설정

\`\`\`css
@layer base {
  :root {
    --background: {HSL값};
    --foreground: {HSL값};
    --card: {HSL값};
    --card-foreground: {HSL값};
    --popover: {HSL값};
    --popover-foreground: {HSL값};
    --primary: {HSL값};
    --primary-foreground: {HSL값};
    --secondary: {HSL값};
    --secondary-foreground: {HSL값};
    --muted: {HSL값};
    --muted-foreground: {HSL값};
    --accent: {HSL값};
    --accent-foreground: {HSL값};
    --destructive: {HSL값};
    --destructive-foreground: {HSL값};
    --border: {HSL값};
    --input: {HSL값};
    --ring: {HSL값};
    --radius: {값};
  }

  .dark {
    --background: {HSL값};
    --foreground: {HSL값};
    /* ... 다크모드 전체 토큰 ... */
  }
}
\`\`\`

---

## 2. 타이포그래피

### 폰트 패밀리

| 용도 | 폰트 | Fallback | 변수 |
|------|------|----------|------|
| 본문 | Inter | system-ui, sans-serif | --font-sans |
| 제목 | {선택된 제목 폰트} | sans-serif | --font-heading |
| 코드 | JetBrains Mono | monospace | --font-mono |

### 타이포 스케일

| 이름 | 크기 | 줄 간격 | 무게 | Tailwind |
|------|------|---------|------|----------|
| Display | 48px / 3rem | 1.1 | Bold (700) | text-5xl font-bold |
| H1 | 36px / 2.25rem | 1.2 | Bold (700) | text-4xl font-bold |
| H2 | 30px / 1.875rem | 1.3 | Semibold (600) | text-3xl font-semibold |
| H3 | 24px / 1.5rem | 1.3 | Semibold (600) | text-2xl font-semibold |
| H4 | 20px / 1.25rem | 1.4 | Medium (500) | text-xl font-medium |
| Body Large | 18px / 1.125rem | 1.6 | Regular (400) | text-lg |
| Body | 16px / 1rem | 1.6 | Regular (400) | text-base |
| Body Small | 14px / 0.875rem | 1.5 | Regular (400) | text-sm |
| Caption | 12px / 0.75rem | 1.4 | Medium (500) | text-xs font-medium |

---

## 3. 간격 시스템

4px 단위 기반 시스템.

| 토큰 | 값 | Tailwind | 용도 |
|------|-----|----------|------|
| space-1 | 4px | p-1, m-1 | 아이콘-텍스트 간격 |
| space-2 | 8px | p-2, m-2 | 인라인 요소 간격 |
| space-3 | 12px | p-3, m-3 | 카드 내부 패딩 (소) |
| space-4 | 16px | p-4, m-4 | 카드 내부 패딩 (기본) |
| space-5 | 20px | p-5, m-5 | 섹션 내 간격 |
| space-6 | 24px | p-6, m-6 | 카드 내부 패딩 (대) |
| space-8 | 32px | p-8, m-8 | 섹션 간 간격 (소) |
| space-10 | 40px | p-10, m-10 | 섹션 간 간격 (중) |
| space-12 | 48px | p-12, m-12 | 섹션 간 간격 (대) |
| space-16 | 64px | p-16, m-16 | 페이지 섹션 간격 |
| space-20 | 80px | p-20, m-20 | 히어로 패딩 |

### 간격 규칙
- 같은 그룹 내 요소: space-2 ~ space-4
- 다른 그룹 간: space-6 ~ space-8
- 섹션 간: space-12 ~ space-16
- 반응형에서 간격도 함께 줄이기 (모바일: 0.75x)

---

## 4. 컴포넌트 규칙

### 기본 컴포넌트 라이브러리: shadcn/ui

IMPORTANT: shadcn/ui 기본 테마를 그대로 쓰면 "AI가 만든 티"가 남.
위의 색상 팔레트와 타이포그래피를 반드시 적용하여 커스텀 테마를 구성할 것.

### Border Radius

| 토큰 | 값 | Tailwind | 용도 |
|------|-----|----------|------|
| radius-sm | {값} | rounded-sm | 작은 요소 (뱃지, 태그) |
| radius-md | {값} | rounded-md | 기본 (버튼, 인풋) |
| radius-lg | {값} | rounded-lg | 카드, 모달 |
| radius-xl | {값} | rounded-xl | 큰 컨테이너 |
| radius-full | 9999px | rounded-full | 아바타, 원형 버튼 |

### 그림자

| 토큰 | 값 | Tailwind | 용도 |
|------|-----|----------|------|
| shadow-sm | {값} | shadow-sm | 미세한 구분 |
| shadow-md | {값} | shadow-md | 카드 기본 |
| shadow-lg | {값} | shadow-lg | 드롭다운, 팝오버 |
| shadow-xl | {값} | shadow-xl | 모달 |

### 버튼 Variants

| Variant | 배경 | 텍스트 | 보더 | 용도 |
|---------|------|--------|------|------|
| primary | --primary | --primary-foreground | none | 주요 CTA |
| secondary | --secondary | --secondary-foreground | none | 보조 액션 |
| outline | transparent | --foreground | --border | 일반 액션 |
| ghost | transparent | --foreground | none | 텍스트 액션 |
| destructive | --destructive | white | none | 삭제/경고 |

### 상태 표현

모든 인터랙티브 요소는 다음 상태를 가진다:
- Default → Hover (opacity-90 또는 색상 변화) → Active (scale-[0.98]) → Focus (ring) → Disabled (opacity-50)

---

## 5. 반응형 브레이크포인트

| 이름 | 너비 | Tailwind Prefix |
|------|------|-----------------|
| Mobile | < 640px | (기본) |
| Tablet | >= 640px | sm: |
| Laptop | >= 1024px | lg: |
| Desktop | >= 1280px | xl: |
| Wide | >= 1536px | 2xl: |

### 반응형 규칙
- 모바일 퍼스트 접근
- 간격: 모바일에서 기본값의 0.75x
- 타이포: 모바일에서 Display, H1은 한 단계 축소
- 레이아웃: 모바일에서 1컬럼, 태블릿에서 2컬럼, 데스크톱에서 풀 레이아웃

---

## 6. 프레임워크별 변환 가이드

### Tailwind CSS (기본)
- 위의 모든 토큰이 Tailwind class로 직접 매핑됨
- tailwind.config.ts에서 커스텀 색상/폰트 설정

### CSS Variables
- shadcn/ui의 CSS variable 체계 사용
- globals.css에서 :root와 .dark에 정의

### React + shadcn/ui (권장)
- cn() 유틸리티로 클래스 조합
- 컴포넌트는 shadcn/ui 기반으로 확장
- 커스텀 variant는 cva()로 정의

### Vue / Svelte / 기타
- CSS Variables를 직접 참조
- Tailwind 클래스 동일하게 사용 가능
- 컴포넌트 래핑 패턴은 프레임워크에 맞게 조정
```

---

## Step 5: knowledge-base 초기화

디자인 시스템이 생성되면 `docs/knowledge-base/visual-language.md`를 초기화한다.

```markdown
## visual-language.md 초기 내용

# 비주얼 랭귀지 현황

## 기반
- 선택된 방향: {방향 이름}
- 생성일: {날짜}
- 디자인 시스템: docs/design-system.md

## 토큰화 완료
- 색상: {Primary, Secondary, Gray scale, Accent 요약}
- 타이포: {폰트 구성 요약}
- 간격: 4px 단위 시스템

## 토큰화 미완료
- (아직 구현 시작 전이므로 항목 없음)

## 변경 이력
| 날짜 | 변경 내용 |
|------|----------|
| {오늘} | 초기 디자인 시스템 생성 (방향: {이름}) |
```

---

## 완료 메시지

```
디자인 시스템이 생성되었습니다!

📄 docs/design-system.md
   → 색상, 타이포, 간격, 컴포넌트 규칙, 다크모드 토큰 포함
   → shadcn/ui CSS 변수 설정 코드 포함

📄 docs/knowledge-base/visual-language.md
   → 디자인 시스템 현황 추적 시작

다음 단계:
→ /moodboard 로 실제 코드 시안을 만들어 비교할 수 있어요
→ /design-preview 로 핵심 화면을 바로 만들 수도 있어요
→ 프로젝트에 shadcn/ui를 설치하고 위의 CSS 변수를 globals.css에 적용하세요
```

---

## 주의사항

- shadcn/ui 기본 테마(보라색 그라데이션, 기본 회색)를 그대로 쓰지 않는다
  → 반드시 레퍼런스 기반의 커스텀 팔레트를 적용
- Inter 본문 폰트는 기본 권장이지만, 레퍼런스에서 다른 폰트가 더 적합하면 변경
- 다크모드 토큰은 처음부터 정의 (나중에 추가하면 일관성이 깨짐)
- Tailwind CSS 기준으로 정의하되, CSS Variables도 함께 제공하여 프레임워크 독립성 유지
