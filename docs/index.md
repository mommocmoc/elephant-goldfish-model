# Elephant-Goldfish Model for Business (EGM-Biz)

## Find the right document

| Location | Purpose |
| --- | --- |
| [Getting started](getting_started.md) and [FAQ](faq.md) | 설치 및 비즈니스 실무 활용 가이드. |
| [Harnesses](harnesses.md), [Claude](claude.md), and [Codex](codex.md) | 에이전트 환경별 설치 및 실행 방법. |
| [Design documents](https://github.com/google/elephant-goldfish-model/blob/main/docs/designs/README.md) | 아키텍처, 워크플로우 설계 배경 및 상세 계획. |
| [Shared runtime resources](../resources/README.md) | 스킬과 금붕어 리뷰어가 참조하는 핵심 실행 규칙 및 문서 품질 표준. |
| [Testing playbook](https://github.com/google/elephant-goldfish-model/blob/main/TESTING.md) | 패키지 무결성 테스트 및 검증 방법. |
| [PR #103 verification reports](https://github.com/google/elephant-goldfish-model/pull/103) | 하네스별 실행 결과 및 검증 기록. |

`docs/` 폴더는 사용자를 위한 안내 문서이며, `resources/` 폴더는 AI 에이전트가 런타임에 직접 읽는 품질 기준서입니다.

AI는 우리가 기획하고, 글을 쓰고, 비즈니스를 운영하는 방식을 완전히 바꾸어 놓았습니다.

**코끼리-금붕어 모델(EGM-Biz)**은 일반 사무직 실무자와 1인 비즈니스 운영자가 AI를 활용해 **내 머릿속 맥락을 타인에게 완벽히 전달하고 고품질 비즈니스 산출물을 빠르게 완성하기 위한 실전 프로토콜**입니다. 원작자인 Dave Rensin의 글 [Elephants, Goldfish, and the New Golden Age of Software Engineering](https://medium.com/@drensin/elephants-goldfish-and-the-new-golden-age-of-software-engineering-c33641a48874)의 핵심 철학을 비즈니스·마케팅·퍼스널 브랜딩 영역으로 확장했습니다.

## Elephant-Goldfish Philosophy (코끼리-금붕어 비즈니스 철학)

### 🐘 The Elephant (코끼리)

**코끼리는 모든 것을 기억합니다.**

사업 기획서를 쓰거나, 세일즈 랜딩페이지를 기획하거나, 내 전문성을 드러내는 퍼스널 브랜딩 글을 쓸 때 우리는 AI가 내 비즈니스의 히스토리, 타겟 고객의 특성, 과거 성공/실패 사례, 내 고유한 관점을 모두 깊이 이해해주기를 바랍니다.

충분한 맥락이 주어진 AI에게 내 아이디어를 던지고, 날카로운 질문을 주고받으며 모호한 생각을 탄탄한 기획서와 산출물로 빚어내는 파트너 — 그것이 바로 **코끼리(Elephant)**입니다.

### 🐠 The Goldfish (금붕어)

**금붕어의 기억력은 3초입니다** [^not-really].

하지만 코끼리와 단둘이서만 일하면 치명적인 문제가 생깁니다. 나와 오랫동안 대화한 AI는 내 편향에 동조하기 시작하고, 나 역시 '내가 아는 배경지식'을 남들도 당연히 알 것이라 착각한 채 설명을 건너뜁니다.

가장 확실한 해결책은 **이전 대화 내용을 전혀 모르는 새로운 AI(금붕어)**를 불러와, 오직 결과물 문서만 보여주고 평가하게 하는 것입니다:
* *"처음 보는 고객/심사위원 입장에서 이 글이 무슨 뜻인지 한 번에 이해되는가?"*
* *"1인 사업가의 시간과 예산으로 이게 진짜 실행 가능한가?"*
* *"불필요한 미사여구나 근거 없는 희망회로는 없는가?"*

이처럼 사전 맥락을 철저히 차단한 채 냉정하게 문서를 검증하는 서브에이전트를 **금붕어(Goldfish)**라고 부릅니다.

### 👤 The Humans (비즈니스 오너인 당신)

AI는 강력한 파트너이자 검증 도구이지만, 비즈니스의 방향과 책임을 지는 주체는 언제나 **사람(당신)**입니다.

코끼리가 선택지를 정리해 주고 금붕어가 날카롭게 허점을 짚어주면, 어떤 리스크를 감수하고 어떤 오퍼와 메시지로 승부를 볼지 최종 결정을 내리는 것은 비즈니스 오너인 당신의 몫입니다.

### Benefits (기대 효과)

*   **압도적인 설득력과 전달력**: 내 머릿속에만 있던 맥락이 '무맥락 금붕어의 복창 테스트'를 거치며 누구나 단번에 이해하고 행동하게 만드는 명쾌한 문서로 바뀝니다.
*   **시간 낭비 및 과잉 기획 방지**: 1인 사업가나 실무자가 당장 필요 없는 일에 에너지를 쏟기 전에, 80/20 본질 금붕어와 수익성 비평가 금붕어가 과잉 기획을 미리 쳐내 줍니다.
*   **기획에서 산출물 제작까지의 일관성**: 검증이 끝난 마스터 기획서의 상세 명세(`Detailed Implementation Plan`)를 기반으로 제안서, 상세페이지 카피, 링크드인 아티클 등 실제 산출물을 한 번에 흔들림 없이 뽑아낼 수 있습니다.

## Documentation & Guides

*   [Getting Started](getting_started.md): 설치 및 비즈니스 5단계 핵심 워크플로우.
*   [Codex](codex.md): Codex 및 ChatGPT 플러그인 설치 및 실행 가이드.
*   [Claude Code](claude.md): Claude Code 플러그인 및 격리 리뷰어 가이드.
*   [Frequently Asked Questions (FAQ)](faq.md): 자주 묻는 질문과 답변.
*   [Framework Architecture](https://github.com/google/elephant-goldfish-model/blob/main/docs/designs/architecture.md): 컴포넌트 구조 및 동작 원리.
*   [Contributing Guidelines](https://github.com/google/elephant-goldfish-model/blob/main/CONTRIBUTING.md): 새로운 스킬, 페르소나, 문서 표준 추가 방법.

[^not-really]: 실제로 금붕어의 기억력이 3초라는 것은 [속설](https://www.biology.ox.ac.uk/article/goldfish-show-a-keen-ability-to-estimate-distances)에 불과하지만, 무맥락 검증의 비유로는 이만한 것이 없습니다!
