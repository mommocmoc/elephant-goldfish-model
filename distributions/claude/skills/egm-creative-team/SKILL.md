---
name: egm-creative-team
license: Apache-2.0
disable-model-invocation: true
description: >-
  Dan Koe의 '1인 크리에이티브 팀(One-Person Creative Team)' 프레임워크를 코끼리-금붕어 모델(EGM-Biz)과
  결합한 퍼스널 브랜딩·콘텐츠 세계관 구축 및 클라이언트 수주 전용 애드온 스킬입니다. 지적 서명(Intellectual
  Signature) 8대 인터뷰, 성과 패턴 추출, 6개월 콘텐츠 맵(Content Map), 무허가 견습생(Permissionless
  Apprentice) 피칭 팩, 그리고 흩어진 메이킹 프로젝트(Build in Public)를 하나의 세계관으로 연결합니다.
---

# EGM Creative Team: Dan Koe 1인 크리에이티브 팀 & 퍼스널 브랜딩 세계관 엔진

When running in Codex or ChatGPT Work, first read [Codex integration](../../docs/codex.md).
In Claude Code or Cowork, read `${CLAUDE_PLUGIN_ROOT}/docs/claude.md` first.

## Overview

이 스킬은 Dan Koe의 **《1인 크리에이티브 팀(The One-Person Creative Team)》** 프레임워크를 코끼리-금붕어 모델(EGM-Biz)에 결합한 **애드온(Add-on) 확장 스킬**입니다.

대부분의 사람들이 AI로 무의미한 양산형 텍스트(Slop)를 찍어내거나, 반대로 자신이 만든 멋진 결과물(예: 산수화 포모도로, 바이브코딩 앱, 디자인 작업기)을 공유하면서도 **"왜 맨땅에 헤딩하는 기분이 들까? 왜 단발성 조회수로 끝나고 내 팬이나 비즈니스로 안 이어질까?"**라고 고민합니다.

그 이유는 1인 크리에이티브 팀의 3대 역할 중 3번(콘텐츠 작성)만 하고, **1번(브랜드 전략가 — 포지셔닝과 세계관)**과 **2번(콘텐츠 전략가 — 검증된 포맷과 6개월 콘텐츠 맵)**이 빠져 있기 때문입니다. 이 스킬은 코끼리의 깊은 맥락 파악 능력과 금붕어의 무맥락 검증을 결합해 다음 5가지 핵심 모드를 수행합니다.

## The 3 Roles of a One-Person Creative Team (코끼리 × 금붕어 역할 분담)

1.  👑 **브랜드 전략가 (Brand Strategist — 코끼리 & `egm-zoom-out` 금붕어)**:
    포지셔닝과 인식(Perception)을 총괄합니다. 단발성 바이럴이나 어그로를 좇느라 신뢰와 권위를 깎아먹지 않도록 당신만의 **'지적 서명(Intellectual Signature)'**과 **'콘텐츠의 세계(World of Content)'**의 중심을 잡습니다.
2.  📊 **콘텐츠 전략가 (Content Strategist — 코끼리 & `egm-design` / `egm-codebase-details` 금붕어)**:
    국내(KR) 및 글로벌(Global) 시장에서 반응이 터지는 포맷, 훅(Hook) 구조, 체류시간, 바이럴 패턴을 데이터 기반으로 리서치하여 6개월 치 **콘텐츠 맵(Content Map: 기둥 × 토픽 × 아이디어 × 각도)**을 도출합니다.
3.  ✍️ **콘텐츠 작가 / 디렉터 (Content Writer — 코끼리 & `egm-comprehension` / `egm-style` 금붕어)**:
    브랜드 전략가의 철학과 콘텐츠 전략가의 앵글을 결합해 X(트위터), 스레드, 링크드인, 릴스/쇼츠 대본, 뉴스레터 초안을 국문(KR) 및 원어민 영문(EN)으로 작성하고, 무맥락 금붕어로 'AI Slop(양산형 냄새)'을 완전히 제거합니다.

## Delegation

금붕어(Goldfish)는 이전 대화 기록이 전혀 없는 깨끗한(fresh) 컨텍스트에서 실행됩니다. 하네스의 서브에이전트 도구(Antigravity: `invoke_subagent`; Claude Code: `Agent`; Codex: `spawn_agent`)를 사용하여 각 서브에이전트를 호출하십시오(`docs/harnesses.md` 참조). 서브에이전트 도구가 없다면 즉시 중단하고 사용자에게 알리십시오.

## Execution Constraints (CRITICAL)

1.  **Artifact-First Delivery (파일 우선 저장 필수)**: 완성된 지적 서명 문서, 패턴 분석서, 6개월 콘텐츠 맵, 피칭 팩 본문을 채팅창에 길게 쏟아내지 말고 반드시 작업 디렉토리(또는 아티팩트 디렉토리)에 마크다운 파일로 저장한 뒤 링크만 안내하십시오.
2.  **One Question at a Time in Interviews (인터뷰 시 한 번에 하나씩 열린 질문)**: 지적 서명 인터뷰(Mode 1)나 프로젝트 세계관 연결(Mode 5)을 진행할 때는 8개 영역을 한꺼번에 묻지 말고, 한 턴에 한 영역씩 대화형으로 깊이 파고드십시오.
3.  **Anti-Slop & Bilingual Excellence**: 모든 산출물은 `resources/document_standards/world-of-content-standard.md` 기준을 준수해야 합니다. 영문(Global) 콘텐츠 생성 시 어색한 직역(Konglish)이나 AI 클리셰(*delve, unlock, elevate, tapestry, testament*)를 엄격히 금지합니다.
4.  **Interaction Invariants**: 모든 인터랙티브 선택 게이트(`ask_question` 등)는 `len(questions) == 1`, `is_multi_select: false`여야 하며 옵션에 `(Recommended)` 접두사를 붙이지 마십시오.

---

## Workflow

### Step 1: Select Creative Team Mode (실행할 모드 선택)

사용자가 실행할 모드를 명시하지 않았다면, 먼저 아래 단일 선택 게이트를 제시하여 어떤 작업부터 시작할지 확인하십시오:

```json
{
  "questions": [
    {
      "question": "Select the One-Person Creative Team workflow mode to execute:",
      "options": [
        "Mode 1: Intellectual Signature Interview (Build my core brand bible via Dan Koe's 8 pillars)",
        "Mode 5: Build-in-Public Worldview Bridge (Turn my scattered projects like Pomodoro into a cohesive brand)",
        "Mode 3: 6-Month Content Map Generator (Pillars x Topics x Ideas x Angles for KR & Global)",
        "Mode 4: Permissionless Apprentice Pitch Pack (10 free high-value drafts + DM to land high-ticket clients)"
      ],
      "is_multi_select": false
    }
  ]
}
```

*(사용자가 채팅으로 Mode 2: 최고 성과 콘텐츠 패턴 추출(`outlier_patterns.md`)을 요청하면 Mode 2로 바로 진입합니다.)*

---

### Mode 1: 지적 서명(Intellectual Signature) 8대 온보딩 인터뷰 → `intellectual_signature.md`

AI가 양산형 글(Slop)이 아닌 **'대체 불가능한 나만의 목소리와 세계관'**으로 모든 기획서와 콘텐츠를 쓰게 만드는 평생 자산 파일(`intellectual_signature.md`)을 구축합니다. 코끼리는 다음 **8대 영역**을 사용자와 대화하며(이미 사용자가 언급한 프로젝트나 힌트가 있다면 먼저 초안을 채워 제안하면서) 구체화합니다:

1.  **① 돕고자 하는 대상 (Who You Help)**:
    *   가장 돕고 싶은 구체적인 '단 한 사람(과거의 나, 또는 특정 페르소나)'은 누구인가?
    *   그들의 '현재 최악의 하루'는 어떤 모습인가?
    *   그들이 친구에게 털어놓는 불평을 **실제 그들의 생생한 입말(한국어 및 영문 뉘앙스)**로 적는다면?
    *   그들이 이미 시도해 보았지만 실패했던 기존 방법(앱, 강의, 습관)은 무엇인가?
2.  **② 당신만의 스토리 (Your Story & Artifacts)**:
    *   당신이 직접 겪고 통과해 온 고통과 결정적 전환점은 무엇인가?
    *   지금까지 직접 만들고, 시도하고, 실패하고, 깨달은 것들의 목록은? (예: 산수화 포모도로 등 직접 만든 프로젝트들과 그 과정에서 얻은 통찰)
3.  **③ 사고를 형성한 원천 (Who Shaped Your Mind)**:
    *   절대 그냥 지나치지 않는 저자, 크리에이터, 철학, 미학(예: 동양 철학/산수화, Dan Koe, 인디 해커 등)과 머릿속에 맴도는 핵심 아이디어는?
4.  **④ 콘텐츠 기둥과 고유한 렌즈 (Pillars & Unique Lens)**:
    *   몇 시간이고 떠들 수 있는 가장 중요한 2~3가지 핵심 기둥(Pillars)은?
    *   남들과 다른 **당신만의 독특한 관점(렌즈/교차점)**은 무엇인가? (예: *"AI 바이브코딩 메이킹 × 동양적 여백과 명상적 생산성"*)
    *   사람들이 당신에게 가장 자주 물어보는 질문은?
5.  **⑤ 사명과 신념 (Mission & Contrarian Beliefs)**:
    *   한 문장으로 독자들을 어디로 이끌고 싶은가?
    *   업계 대다수가 옳다고 믿지만 당신은 틀렸다고 생각하는 **반(反)상식적 신념**은?
6.  **⑥ 싸우고 반대하는 것 (What You're Against — The Enemy)**:
    *   당신의 독자들을 좀먹고 있는 잘못된 삶의 방식이나 업계의 신화(적)는 무엇인가? (예: "인간을 기계처럼 몰아붙이는 도파민 중독형 생산성 툴", "메이킹 없이 입만 떠드는 가짜 전문가")
7.  **⑦ 최종 목적지 (Where You're Taking Them)**:
    *   당신을 따른 후 독자들의 일상은 어떻게 변해 있는가?
    *   그들의 이야기에서 당신의 역할은 무엇인가? (완벽한 구루가 아니라, **직접 실험하며 지도를 그려 공유하는 탐험가/메이커 동료**)
8.  **⑧ 규칙과 고유 어휘 (Rules & Coined Words)**:
    *   이상적인 독자가 매일 실천해야 할 5~10가지 규칙은?
    *   당신만의 아이디어, 방법론, 미학에 붙인 **고유한 이름(조어/Named Concept, 국문 & 영문)**은 무엇인가? (예: *Ink-Wash Productivity(수묵 생산성)*, *Calm Vibe Coding*, *Mindful Maker* 등 코끼리가 3~5개 네이밍 후보를 적극 제안)

**산출물 저장 및 금붕어 검증**:
*   완성된 내용을 `intellectual_signature.md` 파일로 저장합니다.
*   저장이 끝나면 `egm-comprehension`(무맥락 독자 금붕어)과 `egm-style`(한·영 카피 금붕어)에게 `world-of-content-standard.md` 기준으로 이 지적 서명이 처음 보는 국내/글로벌 독자에게 충분히 매력적이고 뾰족한지 검증받도록 안내합니다.

---

### Mode 2: 최고 성과 콘텐츠 패턴 추출 (Outlier Pattern Extraction) → `outlier_patterns.md`

내 과거 상위 성과 게시물, 또는 내가 벤치마킹하려는 국내·글로벌 크리에이터 5~10명의 최고 성과 콘텐츠에서 성장 공식을 추출합니다:

1.  사용자에게 분석할 기존 게시물 텍스트/링크(또는 벤치마킹할 롤모델 크리에이터/사상가 목록)를 입력받습니다.
2.  다음 4가지 요소를 해부하여 `outlier_patterns.md`에 정리합니다:
    *   **Hook Anatomy (첫 3초를 사로잡는 첫 문장/썸네일 공식 — KR & EN)**
    *   **Core Idea & Contrarian Angle (사람들을 멈추게 만든 핵심 통찰)**
    *   **Voice & Rhythm (문장 길이, 호흡, 여백, 어조의 특징)**
    *   **Worldview & Tribe Rallying Cry (사람들을 팬으로 결집시키는 철학적 메시지)**

---

### Mode 3: 6개월 리서치 기반 고밀도 콘텐츠 맵 구축 → `content_map.md`

"오늘 뭐 올리지?"라는 맨땅에 헤딩을 영원히 끝내는 **6개월 치 콘텐츠 청사진(`content_map.md`)**을 생성합니다:

1.  워크스페이스에 `intellectual_signature.md`와 `outlier_patterns.md`가 있다면 자동으로 읽어 들이고, 없다면 핵심 기둥과 타겟(`KR` / `Global` / `Both`)을 빠르게 확인합니다.
2.  다음 계층 구조로 `content_map.md`를 설계합니다:
    *   **3~4개의 핵심 기둥 (Broad Pillars)**: 내 비즈니스의 권위와 매력을 보여주는 대주제 (예: Pillar 1. AI & 바이브코딩 메이킹 / Pillar 2. 동양적 여백과 몰입의 철학(산수화 등) / Pillar 3. 1인 글로벌 메이커의 비즈니스 실험기)
    *   **각 기둥당 5~7개의 세부 토픽 (Sub-Topics)**
    *   **각 토픽당 성과 가능성이 높은 5~10개의 핵심 아이디어 (High-Performing Ideas)**
    *   **각 아이디어당 4~5개의 증명된 각도 (Viral & Deep Angles)**:
        *   *Angle A (Story / Build-in-Public)*: "내가 산수화 포모도로를 만들며 깨달은 것"
        *   *Angle B (Contrarian / Enemy)*: "왜 기존 포모도로 앱들은 오히려 당신의 집중력을 파괴하는가"
        *   *Angle C (Actionable Breakdown / How-To)*: "비개발자가 AI로 하루 만에 자신만의 철학이 담긴 앱을 만드는 4단계"
        *   *Angle D (Philosophical / Essay)*: "먹물이 번지는 속도로 일한다는 것 (수묵 생산성)"
3.  각 아이디어를 한국(스레드, 링크드인, 블로그)과 글로벌(X, Global LinkedIn, Reddit, Product Hunt, Substack)에 어떻게 원소스 멀티유즈(OSMU)로 배포할지 주간 루틴표를 함께 작성합니다.

---

### Mode 4: '무허가 견습생(Permissionless Apprentice)' 클라이언트 수주 팩 → `permissionless_pitch_pack.md`

콜드 이메일 영업 스트레스 없이 월 1,000만~7,500만 원($10k~$75k) 가치의 기업/창업자 클라이언트를 수주하거나 파트너십을 따내는 실전 무기입니다:

1.  **Target Selection**: 내가 돕고 싶은 타겟 창업자, 크리에이터, 또는 기업 브랜드를 지정합니다.
2.  **Brand & Bottleneck Audit**: 타겟의 현재 SNS/웹사이트/콘텐츠에서 무엇이 막혀 있는지(단발성 홍보에 그침, 세계관 부재, 글로벌 확장 부재 등) 진단합니다.
3.  **10 High-Value Free Drafts (즉시 발행 가능한 고퀄리티 맞춤 게시물 10선)**:
    *   타겟 클라이언트가 오늘 당장 복사해서 올려도 반응이 터질 수준의 맞춤형 게시물 초안 10개(X/트위터, 링크드인, 또는 릴스/쇼츠 대본)를 작성합니다.
4.  **JK Molina Style Permissionless DM & Grant Cardone Style Public Pitch**:
    *   대가 없이 가치를 먼저 선물하는 정중하고 거부할 수 없는 **1:1 DM / 콜드메일 문구(국문 & 영문)**
    *   DM이 안 통할 때 공개 릴스나 포스팅으로 던지는 **"OO님에게는 새로운 크리에이티브 디렉터(또는 AI 프로덕트 파트너)가 필요합니다" 공개 피칭 스크립트**를 함께 담아 `permissionless_pitch_pack.md`로 저장합니다.

---

### Mode 5: Build-in-Public 세계관 브릿지 (흩어진 내 프로젝트들을 하나의 브랜드로 엮기) → `brand_worldview_bridge.md`

**"산수화 포모도로 같은 것들을 만들고 만드는 법을 공유하고는 있는데, 왜 맨땅에 헤딩하는 기분일까?"**를 정면으로 해결하는 모드입니다.

1.  **Diagnose the "Random Maker Trap" (단발성 메이커 함정 진단)**:
    *   지금까지 만든 작업물(예: 산수화 포모도로 등)과 공유 방식을 입력받아, 왜 그것들이 단발성 "오, 신기하네요!" 반응에서 끝나고 장기적인 팬덤/수익화로 안 모이는지 코끼리가 진단합니다.
    *   핵심 원인: **결과물(Artifact)과 만드는 법(How-To)만 있고, 그 모든 프로젝트를 하나로 꿰어주는 '우산(Umbrella Worldview — 나는 왜 이런 종류의 것들을 만드는 사람인가?)'과 '독자가 들어올 방(Funnel/Community)'이 없기 때문**입니다.
2.  **Construct the Umbrella Worldview (상위 세계관 우산 씌우기)**:
    *   당신이 만든 산수화 포모도로, 앞으로 만들 툴과 콘텐츠들을 하나로 묶어주는 **단 하나의 정체성 문장(One-Line Identity, 국문 & 영문)**을 설계합니다.
3.  **The 3-Layer Build-in-Public Formula (메이킹 공유 3단 공식 설계)**:
    앞으로 무언가를 만들고 공유할 때 맨땅에 헤딩하지 않도록, 하나의 프로젝트를 만들 때마다 자동으로 **3가지 레이어의 콘텐츠(KR & EN)**가 쏟아져 나오는 템플릿을 구축합니다:
    *   **Layer 1 (Why & Aesthetic — 세계관 포스트)**: *"왜 나는 삭막한 디지털 타이머 대신 먹물이 번지는 산수화 포모도로를 만들었는가"* → 나의 철학과 미학에 공감하는 진성 팬 흡수
    *   **Layer 2 (How & System — 메이커 권위 포스트)**: *"AI 에이전트와 함께 내 머릿속 미학을 실제 작동하는 웹앱으로 구현한 설계 과정"* → 기술력·기획력을 보고 찾아오는 비즈니스 리드/클라이언트 흡수
    *   **Layer 3 (What & Invitation — 프로덕트/뉴스레터 전환)**: *"직접 써보시고 당신의 가장 고요한 25분을 경험해 보세요 (+ 다음 실험 뉴스레터 구독/사전예약)"* → 내 이메일 리스트와 세계관 안으로 독자 축적
4.  완성된 전략과 당장 이번 주에 올릴 한국어/영어 대표 고정글(Pinned Post) 초안을 `brand_worldview_bridge.md`에 저장합니다.
