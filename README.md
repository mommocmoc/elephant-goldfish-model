<p align="center">
  <a href="https://egm.dev"><img src="docs/assets/readme-header.svg" width="480" height="116" alt="Elephant-Goldfish Model for Business"></a>
</p>

<p align="center"><strong>코끼리와 함께 기획하고, 금붕어로 검증하여, 실제 비즈니스 산출물을 완성하세요.</strong><br>Design with an elephant. Review with a goldfish. Ship business deliverables.</p>

<p align="center">
  <a href="#install">Install</a> &ensp;·&ensp;
  <a href="#try-your-first-design">Quickstart</a> &ensp;·&ensp;
  <a href="docs/getting_started.md">Documentation</a> &ensp;·&ensp;
  <a href="https://egm.dev">Website</a>
</p>

<br>

**EGM-Biz (Elephant-Goldfish Model for Business & Solopreneurs)** 는 Google의 오픈소스 `elephant-goldfish-model` 프레임워크를 **일반 사무직 실무자, 마케터, 기획자, 그리고 1인 비즈니스 운영자**가 즉시 사용할 수 있도록 전면 개조한 한국어 비즈니스 기획·검증·산출물 제작 플러그인입니다.

내가 가진 비즈니스 배경과 암묵적 맥락은 **코끼리(Elephant)**가 깊이 이해하여 마스터 기획서를 함께 쓰고, 내 머릿속 맥락을 전혀 모르는 **금붕어(Goldfish)** 서브에이전트들이 처음 보는 고객·심사위원·외주 파트너의 눈으로 냉정하게 검증한 뒤, 검증이 끝난 명세에 따라 **실제 필요한 최종 산출물(사업기획서/제안서, 마케팅·세일즈 카피, 퍼스널 브랜딩 콘텐츠)**까지 한 번에 제작합니다.

### 왜 일반 업무와 1인 비즈니스에 '코끼리-금붕어 모델'이 필요한가요?

AI와 긴 대화를 나누며 기획서나 홍보 글을 쓰다 보면 치명적인 함정에 빠집니다:
1. **지식의 저주 (내 머릿속에만 있는 맥락)**: 나는 너무 잘 아는 이야기라 당연하게 생략했는데, 아무 맥락을 모르는 고객이나 심사위원, 협업 파트너가 읽으면 *"그래서 이게 무슨 말이지? 왜 사야 하지?"* 하고 이탈합니다.
2. **AI의 아첨과 동조 (Echo Chamber)**: 한 채팅창에서 오래 대화하면 AI가 내 아이디어에 무조건 맞장구치며("정말 훌륭한 기획입니다!") 치명적인 수익성 허점이나 리소스 과부하를 지적하지 못합니다.
3. **기획과 실제 산출물의 괴리**: 대화는 거창하게 했는데 막상 최종 제안서, 랜딩페이지 카피, 링크드인 포스팅을 뽑아보면 핵심 수치나 차별점이 빠져 있습니다.

EGM-Biz는 역할을 엄격히 분리하여 이 문제를 해결합니다:
* 🐘 **코끼리 (Elephant — 심층 맥락 파트너)**: 당신의 비즈니스 히스토리, 기존 자산, 제약 조건을 모두 기억하며 빈틈없는 마스터 기획서(`design_draft.md`)와 최종 산출물을 작성합니다.
* 🐠 **금붕어 (Goldfish — 무맥락 3초 검증단)**: 이전 대화를 전혀 모르는 백지상태(Zero-Context)로 소환되어 오직 적힌 문서만 읽고 **"내가 이해한 내용이 이게 맞나요?(복창 테스트)"**, **"1인 사업가가 이 시간/예산으로 감당 가능한가요?"**, **"처음 보는 고객이 3초 만에 가치를 이해하나요?"**를 가차 없이 검증합니다.
* 👤 **당신 (You — 비즈니스 오너)**: 코끼리와 금붕어의 분석을 바탕으로 핵심 전략(타겟, 오퍼, 채널)을 결정하고 최종 산출물을 승인합니다.

---

## 핵심 활용 분야 (3대 비즈니스 트랙)

1. **사업 기획서 / 제안서 / PRD**: 신규 비즈니스 아이템 기획, 정부지원사업 계획서, B2B 클라이언트 제안서, 서비스 기획서
2. **마케팅·세일즈 콘텐츠**: 고전환 랜딩페이지 카피, 상세페이지 기획안, 콜드메일/뉴스레터 시퀀스, 보도자료
3. **퍼스널 브랜딩 (Personal Branding)**: 링크드인/스레드/블로그 핵심 오피니언 아티클, 창업가/실무자 스토리텔링, 원소스 멀티유즈(OSMU) 콘텐츠 설계

---

## Install

사용 중인 AI 에이전트(Antigravity, Claude Code, Codex 등)에 플러그인 전체를 설치하세요.

### Antigravity CLI & App

```sh
# 글로벌 플러그인 디렉토리에 바로 설치 (로컬 포크/클론 폴더 기준)
mkdir -p ~/.gemini/config/plugins/egm
cp -R plugin.json README.md LICENSE skills agents resources docs assets ~/.gemini/config/plugins/egm/
```

또는 원격 저장소에서 설치할 경우:

```sh
agy plugin install https://github.com/google/elephant-goldfish-model
```

**Antigravity App** 및 **Antigravity for IDEs**(VS Code 확장)에서는 `/plugin`을 실행하거나 [설치 안내](docs/harnesses.md#antigravity-app-and-antigravity-for-ides)를 참고하세요.

### Claude Code

```sh
claude plugin marketplace add google/elephant-goldfish-model
claude plugin install egm@egm
```

### Codex

```sh
codex plugin marketplace add google/elephant-goldfish-model
codex plugin add egm@egm
```

설치 후 새로운 세션을 시작하세요. **Claude Cowork** 및 **ChatGPT Work** 설정은 [설치 가이드](docs/harnesses.md)를 참고하세요.

---

## Try your first design

AI 에이전트에서 작업 폴더를 열고 기획하거나 제작하고 싶은 비즈니스 주제를 던져보세요.
(Antigravity CLI 및 Claude Code에서는 `/egm:egm-brainstorm`처럼 `/egm:` 접두사를 붙여 호출합니다.)

<img src="skills/egm-brainstorm/assets/icon.svg" width="20" height="20" align="top" alt=""> **1단계: 코끼리 인터뷰 & 6대 금붕어 아이디어 검증 (`/egm-brainstorm`)**

```text
/egm-brainstorm 1인 지식창업자를 위한 B2B 뉴스레터 대행 서비스 런칭 기획서와 랜딩페이지 카피, 링크드인 홍보 글을 만들고 싶어.
```

코끼리가 타겟 고객, 가격/오퍼, 가용 시간 등 핵심 맥락을 질문하여 끌어낸 뒤, 6마리의 무맥락 금붕어가 동시에 반론을 제기하여 탄탄한 기획 브리프(`concepts_brief.md`)로 정리해 줍니다.

<img src="skills/egm-draft/assets/icon.svg" width="20" height="20" align="top" alt=""> **2단계: 마스터 기획서 및 산출물 명세 작성 (`/egm-draft`)**

```text
/egm-draft
```

코끼리가 브리프를 바탕으로 문제 정의 → 핵심 설득 전략(Hook-Story-Offer) → 대안 비교 → **상세 산출물 제작 명세(`Detailed Implementation Plan`)**까지 섹션별로 당신과 합의하며 `design_draft.md`를 작성합니다.

<img src="skills/egm-review/assets/icon.svg" width="20" height="20" align="top" alt=""> **3단계: 금붕어 무맥락 검증 및 최종 산출물 제작 (`/egm-review`)**

```text
/egm-review design_draft.md
```

사전 대화를 전혀 모르는 금붕어들이 문서를 읽고 **요약 복창 테스트(내 의도가 타인에게 똑같이 읽히는지 확인)**와 **산출물 즉각 제작 준비도 검증**을 수행합니다. 금붕어가 찾은 빈틈을 코끼리와 함께 메운 뒤, 확정된 명세에 따라 **최종 비즈니스 산출물 파일(제안서, 랜딩페이지 카피, 링크드인 원고 등)을 즉시 제작**합니다.

전체 워크플로우와 상세 활용법은 [시작 가이드](docs/getting_started.md)를 확인하세요.

---

## How it works

<table>
  <thead><tr><th align="left">Role</th><th align="left">Context</th><th align="left">Responsibility</th></tr></thead>
  <tbody>
    <tr valign="top"><td><strong>Elephant (코끼리)</strong></td><td>당신의 비즈니스 배경, 기존 자산, 제약 조건, 이전 대화 맥락 전체</td><td>심층 인터뷰로 암묵지를 끌어내고 마스터 기획서 및 최종 산출물을 작성합니다.</td></tr>
    <tr valign="top"><td><strong>Goldfish (금붕어)</strong></td><td>이전 대화 기록 Zero — 오직 작성된 문서와 참조 파일만 읽음</td><td>처음 보는 고객·심사위원·외주자의 눈으로 이해 안 되는 비약, 수익성 허점, 모호한 지시를 찾아냅니다.</td></tr>
    <tr valign="top"><td><strong>You (비즈니스 오너)</strong></td><td>비즈니스의 최종 목표와 현실적 우선순위</td><td>트레이드오프를 결정하고 기획을 승인하여 최종 산출물을 완성합니다.</td></tr>
  </tbody>
</table>

금붕어 검증은 이전 대화 기록이 격리된 새로운 서브에이전트 컨텍스트를 필요로 합니다. 자세한 내용은 [지원 환경 및 제약 사항](docs/harnesses.md#support-status--contributing-other-harnesses)을 참고하세요.

---

## Choose a skill

반드시 모든 스킬을 순서대로 쓸 필요는 없습니다. 현재 막혀 있는 업무 병목에 맞는 스킬을 바로 호출하세요.

<table>
  <thead><tr><th align="left">Skill</th><th align="left">Use it to…</th></tr></thead>
  <tbody>
    <tr valign="top"><td width="160" valign="top"><img src="skills/egm-brainstorm/assets/icon.svg" width="20" height="20" alt="" align="top">&nbsp;<a href="skills/egm-brainstorm/SKILL.md">Brainstorm</a></td><td>막연한 사업/마케팅/브랜딩 아이디어를 질문과 금붕어 반론을 통해 기획 브리프(<code>concepts_brief.md</code>)로 구체화합니다.</td></tr>
    <tr valign="top"><td width="160" valign="top"><img src="skills/egm-draft/assets/icon.svg" width="20" height="20" alt="" align="top">&nbsp;<a href="skills/egm-draft/SKILL.md">Draft</a></td><td>브리프나 메모를 바탕으로 마스터 기획서와 상세 산출물 제작 명세를 섹션별로 작성합니다.</td></tr>
    <tr valign="top"><td width="160" valign="top"><img src="skills/egm-review/assets/icon.svg" width="20" height="20" alt="" align="top">&nbsp;<a href="skills/egm-review/SKILL.md">Review</a></td><td>5단계 무맥락 금붕어 검증(본질·복창 이해도·수익성·카피·제작 준비도)을 수행하고 최종 산출물 제작으로 연결합니다.</td></tr>
    <tr valign="top"><td width="160" valign="top"><img src="skills/egm-resolve/assets/icon.svg" width="20" height="20" alt="" align="top">&nbsp;<a href="skills/egm-resolve/SKILL.md">Resolve</a></td><td>금붕어가 지적한 사항을 코끼리의 맥락으로 평가하여 원클릭 또는 항목별로 문서에 반영합니다.</td></tr>
    <tr valign="top"><td width="160" valign="top"><img src="skills/egm-review-loop/assets/icon.svg" width="20" height="20" alt="" align="top">&nbsp;<a href="skills/egm-review-loop/SKILL.md">Review&nbsp;loop</a></td><td>완성된 제안서나 카피 원고에 대해 원하는 금붕어 조합(예: 무맥락 이해도 + 카피 에디터)만 골라 집중 점검합니다.</td></tr>
    <tr valign="top"><td width="160" valign="top"><img src="skills/egm-bootstrap/assets/icon.svg" width="20" height="20" alt="" align="top">&nbsp;<a href="skills/egm-bootstrap/SKILL.md">Bootstrap</a></td><td>내 업무 폴더에 쌓인 기존 기획안·원고·노트를 탐색해 코끼리가 즉시 이해할 수 있는 비즈니스 맥락 지도(<code>README.md</code>)를 구축합니다.</td></tr>
    <tr valign="top"><td width="160" valign="top"><img src="skills/egm-drift-report/assets/icon.svg" width="20" height="20" alt="" align="top">&nbsp;<a href="skills/egm-drift-report/SKILL.md">Drift&nbsp;report</a></td><td>마스터 기획서의 의도·수치·핵심 메시지가 실제 제작된 산출물에서 누락되거나 왜곡되지 않았는지 대조합니다.</td></tr>
  </tbody>
</table>

## Learn more

- [Getting started](docs/getting_started.md): 비즈니스 5단계 워크플로우 및 산출물 제작 가이드.
- [Setup](docs/harnesses.md): 설치 방법 및 에이전트 환경 설정.
- [Architecture](https://github.com/google/elephant-goldfish-model/blob/main/docs/designs/architecture.md): 스킬과 금붕어 서브에이전트 구조.
- [Testing](https://github.com/google/elephant-goldfish-model/blob/main/TESTING.md): 패키지 검증 및 테스트 가이드.
- [Dave Rensin’s original essay](https://medium.com/@drensin/elephants-goldfish-and-the-new-golden-age-of-software-engineering-c33641a48874): 코끼리-금붕어 모델의 탄생 배경.

## Contribute

See the [accessibility statement](https://github.com/google/elephant-goldfish-model/blob/main/ACCESSIBILITY.md)
for accessibility goals, known limits, and how to report barriers.

Read [contributing](https://github.com/google/elephant-goldfish-model/blob/main/CONTRIBUTING.md) for PRs, [issues](https://github.com/google/elephant-goldfish-model/issues) for bugs, and [discussions](https://github.com/google/elephant-goldfish-model/discussions) for questions.

## License

[Apache 2.0](LICENSE). This is not an officially supported Google product.
