# design-preview

> 선택된 디자인 방향과 DESIGN.md 기반으로 핵심 화면 프리뷰를 생성하는 스킬 (JSONC 디자인 브리프 워크플로우 내장)

## 선택적 도구

| 도구 | 용도 | 없을 때 |
|------|------|---------|
| Playwright | 자동 렌더링 → 스크린샷 → 디자인 브리프와 비교 → 불일치 자동 수정 | 사용자가 브라우저에서 직접 확인 또는 채팅으로 피드백 |
| Figma MCP | Figma에 디자인 프레임 직접 생성 | HTML 프리뷰로 대체 (기본 동작) |

---

## Trigger

다음 키워드가 포함된 사용자 요청 시 이 스킬을 활성화합니다:

- "프리뷰"
- "느낌 보여줘"
- "화면 만들어줘"
- "design preview"

---

## 프로세스

### Step 1: 컨텍스트 수집

현재 프로젝트의 디자인 컨텍스트를 먼저 파악합니다.

1. `docs/design-system.md`를 읽어 현재 디자인 토큰(색상, 타이포그래피, 간격, 그림자 등)을 확인합니다.
2. `docs/knowledge-base/screen-inventory.md`를 읽어 기존 화면 목록과 구조를 파악합니다.

### Step 2: JSONC 디자인 브리프 변환 (스크린샷/목업 제공 시)

사용자가 스크린샷이나 목업을 제공한 경우, "AI 보라색 UI" 현상을 방지하기 위해 반드시 중간 단계로 구조화된 JSONC 디자인 브리프를 생성합니다.

#### 브리프 변환 프롬프트 템플릿

```
이것은 [서비스 종류]의 [화면 이름]입니다.
이 스크린샷의 UI를 디자이너가 개발자에게 전달하는 수준으로 상세히 분석하세요.

브리프에 포함할 것:
- 라이트/다크 모드 모두 커버
- Tailwind CSS 브레이크포인트에 맞는 반응형 명세
- 색상은 rough palette로 추출, Primary/Secondary 2개 + Gray + Accent
- 구조화된 JSONC로 출력
```

#### JSONC 디자인 브리프 출력 예시

```jsonc
{
  // 화면 메타 정보
  "screen": {
    "name": "대시보드 메인",
    "service": "SaaS 분석 도구",
    "description": "핵심 지표와 최근 활동을 한눈에 보여주는 메인 화면"
  },

  // 색상 팔레트
  "palette": {
    "primary": "#2563EB",     // 주요 액션, CTA 버튼
    "secondary": "#7C3AED",   // 보조 강조, 그래프 포인트
    "gray": {
      "50": "#F9FAFB",
      "100": "#F3F4F6",
      "200": "#E5E7EB",
      "500": "#6B7280",
      "700": "#374151",
      "900": "#111827"
    },
    "accent": "#F59E0B",      // 알림, 배지, 경고
    "darkMode": {
      "background": "#0F172A",
      "surface": "#1E293B",
      "text": "#F1F5F9"
    }
  },

  // 반응형 레이아웃
  "responsive": {
    "mobile": {    // < 640px (sm 미만)
      "layout": "single-column",
      "sidebar": "hidden, 햄버거 메뉴로 전환",
      "cards": "full-width, 세로 스택"
    },
    "tablet": {    // 640px - 1024px (sm ~ lg)
      "layout": "two-column",
      "sidebar": "collapsible",
      "cards": "2-column grid"
    },
    "desktop": {   // 1024px+ (lg 이상)
      "layout": "sidebar + main content",
      "sidebar": "always visible, 240px",
      "cards": "3-column grid"
    }
  },

  // 컴포넌트 계층 구조
  "components": [
    {
      "name": "TopNav",
      "type": "navigation",
      "children": ["Logo", "SearchBar", "NotificationBell", "UserAvatar"]
    },
    {
      "name": "Sidebar",
      "type": "navigation",
      "children": ["NavItem[]", "QuickActions"]
    },
    {
      "name": "KPICardGrid",
      "type": "data-display",
      "children": ["KPICard x 4"],
      "props": {
        "columns": { "mobile": 1, "tablet": 2, "desktop": 4 }
      }
    }
  ]
}
```

### Step 3: 코드 생성

JSONC 브리프 + `design-system.md` 토큰을 기반으로 코드를 생성합니다.

- **출력 형식**: HTML (Tailwind CSS) 또는 React (TSX + Tailwind) — `preview/` 폴더에 HTML 파일로 기본 출력
- **디자인 토큰 우선**: `design-system.md`에 정의된 토큰을 하드코딩 값보다 우선 적용
- **다크 모드**: `dark:` 프리픽스로 처음부터 구현

> **기본 출력은 항상 HTML 프리뷰입니다.** Figma 출력은 선택적 확장 기능입니다.

#### PostHog 엔지니어의 디자인 프롬프트 구조

코드 생성 시 다음 구조를 따릅니다:

```
1. 역할 설정: "너는 시니어 프론트엔드 엔지니어이자 UI/UX 디자이너다"
2. 컨텍스트: JSONC 브리프 전체를 제공
3. 구체적 요구사항: 사용할 기술 스택, 반응형 브레이크포인트, 컬러 팔레트
4. 제약 조건: "AI스러운 보라색/그라데이션 사용 금지", "실제 서비스처럼 보여야 함"
5. 출력 형식: 단일 파일 또는 컴포넌트 분리 여부 명시
```

#### 0xminds 80% 성공 프롬프트 공식

```
[구체적 맥락] + [명확한 제약 조건] + [원하는 출력 형식] + [품질 기준] = 80% 성공률

예시:
"SaaS 대시보드의 KPI 카드 섹션을 만들어줘.
- Tailwind CSS만 사용 (styled-components 금지)
- 모바일 퍼스트, 4개 카드가 1→2→4 컬럼으로 전환
- 숫자는 animate-in 효과 포함
- 색상은 브리프의 palette 섹션 그대로 사용
- 코드 한 파일에 완결, 외부 의존성 없이"
```

### Step 4: 4가지 상태 고려

생성되는 모든 화면은 반드시 다음 4가지 상태를 포함합니다:

1. **로딩 상태**: 스켈레톤 UI 또는 스피너
2. **에러 상태**: 에러 메시지 + 재시도 버튼
3. **빈 상태**: 빈 데이터일 때의 일러스트 + CTA
4. **성공 상태**: 실제 데이터가 표시되는 정상 상태

### Step 5: (선택) Figma 출력

Figma MCP가 연결되어 있으면 생성된 디자인을 Figma에 프레임으로 직접 생성할 수 있습니다.

Figma MCP가 설정되어 있지 않은 경우, 다음 메시지를 사용자에게 제시합니다:

```
"Figma에 직접 디자인을 만들려면 Figma MCP 연결이 필요한데 설정되어 있지 않아요.

어떻게 할까요?
1️⃣ Figma MCP 연결하기 (설정 방법 안내)
2️⃣ HTML 파일로 preview/ 폴더에 만들게요 (기본 동작)
3️⃣ 채팅으로 원하는 디자인을 설명해줄게요"
```

> Figma 연결 여부와 관계없이, HTML 프리뷰는 항상 `preview/` 폴더에 생성됩니다.

### Step 6: screen-inventory 업데이트

생성된 화면 정보를 `docs/knowledge-base/screen-inventory.md`에 추가합니다.

```markdown
## [화면 이름]
- 경로: /path/to/component
- 상태: 프리뷰 생성 완료
- 포함 상태: 로딩 / 에러 / 빈 상태 / 성공
- 반응형: 모바일 / 태블릿 / 데스크톱
- 다크모드: 지원
- 생성일: YYYY-MM-DD
```

---

## 반복 시 핵심 체크리스트

매 프리뷰 생성/수정 시 아래 항목을 확인합니다:

- [ ] **4가지 상태**: 로딩, 에러, 빈 상태, 성공
- [ ] **반응형**: 모바일(< 640px) → 태블릿(640-1024px) → 데스크톱(1024px+)
- [ ] **다크모드**: 처음부터 구현
- [ ] **마이크로 인터랙션**: hover, focus, transition
- [ ] **실제 데이터**: 현실적인 길이/형태의 데이터로 테스트
- [ ] **접근성**: ARIA 속성, 키보드 네비게이션, 색상 대비

---

## (선택) Playwright 스크린샷 비교 루프

품질 검증을 위해 Playwright를 활용한 시각적 회귀 테스트를 수행할 수 있습니다.

Playwright가 설치되어 있지 않은 경우, 다음 메시지를 사용자에게 제시합니다:

```
"프리뷰를 만들었어요!
자동으로 렌더링 결과를 비교하려면 Playwright가 필요한데 설치되어 있지 않아요.

어떻게 할까요?
1️⃣ 지금 설치하고 자동 비교 사용하기 (npm install playwright && npx playwright install chromium)
2️⃣ 직접 브라우저에서 열어서 확인할게요 → preview/ 폴더의 index.html을 열어보세요
3️⃣ 채팅으로 어떤 부분이 다른지 말해줄게요"
```

### 설정

```typescript
// playwright.config.ts (프리뷰 검증용)
import { defineConfig } from '@playwright/test';

export default defineConfig({
  use: {
    baseURL: 'http://localhost:3000',
  },
  projects: [
    { name: 'mobile', use: { viewport: { width: 375, height: 812 } } },
    { name: 'tablet', use: { viewport: { width: 768, height: 1024 } } },
    { name: 'desktop', use: { viewport: { width: 1440, height: 900 } } },
  ],
});
```

### 비교 루프 프로세스

```
1. 프리뷰 코드 생성 → 로컬 서버 실행
2. Playwright로 3개 뷰포트(모바일/태블릿/데스크톱) 스크린샷 촬영
3. 원본 스크린샷(있을 경우)과 비교
4. 차이가 임계값(예: 5%) 초과 시 → 차이 하이라이트 이미지 생성
5. 사용자에게 비교 결과 제시 → 수정 여부 결정
6. 수정 시 → Step 3부터 반복
```

### 스크린샷 비교 테스트 예시

```typescript
import { test, expect } from '@playwright/test';

test('프리뷰 시각적 검증', async ({ page }) => {
  await page.goto('/preview/dashboard');

  // 성공 상태 스크린샷
  await expect(page).toHaveScreenshot('dashboard-success.png', {
    maxDiffPixelRatio: 0.05,
  });

  // 로딩 상태 스크린샷
  await page.route('**/api/**', route => route.abort());
  await page.reload();
  await expect(page).toHaveScreenshot('dashboard-loading.png');

  // 빈 상태 스크린샷
  await page.route('**/api/**', route =>
    route.fulfill({ body: JSON.stringify({ data: [] }) })
  );
  await page.reload();
  await expect(page).toHaveScreenshot('dashboard-empty.png');
});
```
