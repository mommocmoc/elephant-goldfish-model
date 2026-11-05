---
name: egm-draft
license: Apache-2.0
disable-model-invocation: true
description: >-
  비즈니스용 코끼리-금붕어 모델(EGM-Biz)의 단계별 기획서 작성 및 최종 산출물 제작 스킬입니다.
  egm-brainstorm에서 완성한 concepts_brief.md(또는 사용자 메모)를 바탕으로 섹션별 합의를 거쳐
  빈틈없는 마스터 기획서(design_draft.md)를 완성하고, 승인 시 최종 산출물까지 제작합니다.
---

# Design Drafting & Deliverable Production (마스터 기획서 작성 및 산출물 제작)

When running in Codex or ChatGPT Work, first read [Codex integration](../../docs/codex.md).
In Claude Code or Cowork, read `${CLAUDE_PLUGIN_ROOT}/docs/claude.md` first.

이 스킬은 브레인스토밍 단계에서 정렬된 비즈니스 문제와 목표를 바탕으로 **체계적인 마스터 기획서(`design_draft.md`)를 섹션별로 작성**하고, 기획이 확정된 후 **실제 필요한 비즈니스 산출물(사업기획서/제안서, 마케팅/세일즈 카피, 퍼스널 브랜딩 콘텐츠 등)을 제작**하도록 안내합니다.

## Execution Constraints (CRITICAL)

1.  반드시 `resources/execution_instructions.md`의 실행 지침을 읽고 준수해야 합니다.
2.  **Artifact-First Delivery (파일 우선 저장 필수)**: 긴 기획안, 초안, 요약, 실행 계획을 채팅창에 그대로 출력하는 것은 **엄격히 금지**됩니다. 반드시 파일 쓰기 도구를 사용해 작업 디렉토리(또는 아티팩트 디렉토리)의 `design_draft.md`(또는 `./design_draft.md`)에 직접 저장하고, 채팅창에는 파일 경로/링크를 안내하며 검토를 요청하십시오.
3.  **Plain-Language Standard (명확하고 구체적인 비즈니스 언어)**: 모든 문서는 추상적인 미사여구나 번역투를 배제하고 명확하고 직관적인 한국어로 작성하십시오. 근거 없는 과장 형용사(예: *seamless*, *robust*, *blazingly fast*, *혁신적인*, *압도적인*)나 장식적 부사(예: *seamlessly*, *effortlessly*, *원활하게*)를 사용하지 마십시오.
4.  **Default to Stress-Testing (스트레스 테스트 기본화)**: 무조건 동의하지 마십시오. 전략과 오퍼의 가장 약한 고리, 타겟 독자의 이탈 가능성, 리소스 한계를 먼저 짚어내십시오.
5.  **No Glazing (빈말 칭찬 금지)**: "훌륭한 생각입니다", "좋은 지적입니다" 같은 아첨을 배제하십시오.
6.  **Critique First (비판 우선)**: 아이디어가 좋아도 빠진 전제나 리스크부터 먼저 짚으십시오.
7.  **Reject Echo-Chamber Framing (앵무새 복창 금지)**: 사용자의 주장을 무비판적으로 따라 하지 마십시오.
8.  **Generate Counter-Arguments (반론 제시)**: 제안된 전략마다 예상되는 가장 강력한 반론과 리스크를 제시하십시오.
9.  **Earn Your Agreement**: 객관적 검증을 통과한 후에만 동의하십시오.
10. **Banned Adjectives (No Flattery)**: 사용자의 입력이나 결정에 대해 "brilliant", "great", "smart", "correct", "perfect", "excellent", "spot-on" 같은 칭찬 형용사를 절대 사용하지 마십시오.
11. **Banned Apologies (불필요한 사과 금지)**: 지적을 받았을 때 "죄송합니다(I apologize, I am sorry)" 같은 사과 대신 수정된 논리와 대안을 즉시 제시하십시오.
12. **Interaction Invariants**: 모든 인터랙티브 게이트(`ask_question` 등)는 한 턴에 딱 하나의 질문(`len(questions) == 1`)만 포함해야 하며, 해당 턴의 유일한 도구 호출이어야 하고, 옵션에 `(Recommended)` 접두사를 붙이지 마십시오.
13. **Author Decides Strategy & Architecture (핵심 전략 결정권은 작성자에게)**: 타겟 포지셔닝, 가격/오퍼 구조, 핵심 채널, 메시지 앵글 등 두 가지 이상의 타당한 선택지가 있는 중요한 비즈니스 결정은 최대 3가지 옵션을 각각의 작동 원리, 장단점(Trade-offs), 실패 조건과 함께 제시하고 단일 선택 게이트를 통해 작성자가 직접 선택하게 하십시오. 옵션을 임의로 순위 매기거나 기본값을 정하지 마십시오. 작성자가 "알아서 골라줘"라고 하더라도 트레이드오프를 다시 설명하고 동일한 선택 게이트를 제시하십시오. 파일명이나 포맷팅 같은 단순 실무적 사항만 에이전트가 직접 결정합니다.

## Inputs

*   **`Context File`** (선택): 이전 단계에서 작성된 문서 경로(예: `docs/designs/<feature_name>.md`, `concepts_brief.md`, 또는 `discussion_summary.md`). 생략된 경우 자동으로 우선순위에 따라 이전 컨텍스트를 탐색합니다.

## Execution Workflow

아래 단계를 순차적으로 진행하되, 사용자가 이전 섹션을 수정하고 싶어 하면 언제든 돌아갈 수 있게 하십시오. 각 섹션 사이에는 사용자의 명시적인 합의(승인) 없이 다음 단계로 건너뛰지 마십시오.

### Section Approval Gate (Steps 2–5 공통 승인 게이트)

파일 쓰기 도구로 `design_draft.md`의 각 섹션을 작성하거나 업데이트한 직후에는, 반드시 같은 턴에서 다음 순서를 엄격히 지켜 승인 게이트를 제시하십시오:

1.  **Visible Chat Summary & Link (첫 번째 - 채팅 요약 및 링크 출력)**: 질문 게이트 도구를 호출하기 전에 **반드시** 일반 채팅 텍스트를 먼저 출력하십시오. 어떤 섹션이 작성/업데이트되었는지 밝히고, 핵심 결정 사항 2~3가지를 요약한 뒤 `design_draft.md` 파일 링크(예: `[design draft](/absolute/project/design_draft.md)`)를 제공하십시오.
2.  **Approval Gate (두 번째 - 단일 선택 승인 모달)**: 같은 턴에서 아래의 단일 선택 게이트(`ask_question`, `len(questions) == 1`, `is_multi_select: false`)를 호출하십시오:

> [!CAUTION] **Negative Constraint (빈 채팅 버블 금지)**: 채팅 설명 텍스트 없이 질문 게이트 도구만 단독으로 호출하지 마십시오. 반드시 채팅 설명과 파일 링크를 먼저 출력해야 합니다.

```json
{
  "questions": [
    {
      "question": "Drafted [Section Name] in design_draft.md. Please review. How would you like to proceed?",
      "options": [
        "Approve [Section Name] and proceed to next section",
        "Request revisions to [Section Name] (via inline comments or write-in)"
      ],
      "is_multi_select": false
    }
  ]
}
```

*   **Option 0 (`Approve [Section Name] and proceed to next section`)**:
    *   만약 턴 컨텍스트에 `design_draft.md`에 대한 인라인 코멘트가 함께 있다면, 해당 수정 사항을 먼저 반영한 뒤 다음 섹션으로 넘어갑니다.
    *   코멘트가 없다면 즉시 다음 섹션 작성으로 넘어갑니다.
*   **Option 1 (`Request revisions to [Section Name] (via inline comments or write-in)`) 또는 인라인 코멘트와 함께 모달 스킵**:
    *   **작성된 수정 요청 또는 인라인 코멘트가 있는 경우**: 요청 사항을 모두 반영하여 `design_draft.md`를 업데이트하고, 사용자에게 알린 뒤 동일한 섹션의 승인 게이트를 다시 제시합니다.
    *   **Empty Write-in Guard (빈 입력 방어 - CRITICAL)**: 추가 입력 텍스트도 없고 인라인 코멘트도 없는 상태로 선택되었다면, 무한 루프에 빠지지 말고 채팅에 `"Please specify the adjustments or additions needed for [Section Name] (either via inline comments on design_draft.md or here in chat) so I can revise the draft."` (어떤 부분을 수정하거나 보완할지 알려주시면 초안에 반영하겠습니다)라고 출력한 뒤 턴을 종료하고 사용자의 답변을 기다리십시오.

### Step 1: Ingest Predecessor Context (이전 브레인스토밍 맥락 불러오기)

1.  **Context File**: `Context File`이 지정되었다면 즉시 읽고 파싱합니다. (선택적 애드온 연동: 작업 디렉토리에 `intellectual_signature.md` 또는 `brand_worldview_bridge.md`가 존재한다면 보조 컨텍스트로 함께 읽어 작성자의 브랜드 세계관과 고유 어휘를 유지합니다.)
2.  **Polymorphic Intake**: 지정되지 않았다면 아래 우선순위로 컨텍스트를 자동 탐색합니다:
    *   **Priority 1: Modern Ideation Brief (`concepts_brief.md`)**: `./concepts_brief.md`, 대화 아티팩트 디렉토리, 또는 `scratch/concepts_brief.md`를 확인하여 있으면 읽어 들입니다.
    *   **Priority 2: Discussion Summary (`discussion_summary.md`)**: `concepts_brief.md`가 없으면 `./discussion_summary.md`, 아티팩트 디렉토리, 또는 `scratch/discussion_summary.md`를 확인합니다. 있을 경우 `/egm-brainstorm`을 다시 실행하라고 강요하지 말고 그대로 사용합니다.
    *   **Priority 3: Interactive Chat Prompt**: 두 파일 모두 없다면 채팅을 통해 해결하려는 비즈니스 문제, 타겟 고객, 목표, 최종 필요한 산출물을 질문하거나, 호스트 명령어 형태(예: `/egm:egm-brainstorm` 또는 스킬 피커)로 `egm-brainstorm`부터 시작할 수 있음을 안내합니다.
3.  **Verify State**: 불러온 맥락을 요약하여 사용자에게 확인받습니다.
4.  **Consensus**: 마스터 기획서 초안 작성을 시작해도 좋을지 명시적 동의를 구합니다.
5.  **Resolve Author**: 브리프에 `Author`가 있으면 그대로 사용하고, 없으면 `git config user.name` → `GIT_AUTHOR_NAME` 순으로 확인한 뒤 둘 다 없으면 채팅으로 물어 문서 헤더에 기록합니다.

### Step 2: Draft Overview, Goals & Problem Statement (개요, 핵심 문제 및 목표 작성)

1.  **Review Context**: 불러온 맥락을 바탕으로 비즈니스 배경과 타겟 고객/독자의 핵심 페인포인트(Pain Point)를 정리합니다.
2.  **Draft Content**: 템플릿(`references/design_doc.md`)에 맞춰 "Overview", "The Problem", "Goals", "Non-Goals", "Critical User Journeys (CUJs)"를 명확한 한국어 마크다운으로 작성합니다.
3.  **Focus**: 세부 실행 방법보다는 *왜(Why)* 이 기획이 필요하며 타겟 고객/심사위원에게 어떤 가치를 주는지에 집중하십시오.
4.  **Write Draft Artifact (MANDATORY)**: 작성한 내용을 `design_draft.md` 파일에 저장합니다.
5.  **Notify & Verify (No Chat Dumps)**: 채팅창에 본문을 그대로 출력하지 말고, `design_draft.md` 링크와 함께 `Overview & Goals`에 대한 Section Approval Gate를 제시합니다.

### Step 3: Draft Technical Plan (핵심 비즈니스 전략 및 설득 아키텍처 작성)

1.  **Synthesize Architecture**: 타겟 독자를 설득하고 목표를 달성하기 위한 전체 전략 구조를 설계합니다.
2.  **Draft Plan**:
    *   **사업 기획서 / 제안서 / PRD**: 핵심 솔루션 구조, 비즈니스 모델/수익 구조, 마일스톤, 리소스 배분 계획.
    *   **마케팅 / 세일즈 콘텐츠**: 고객 유입부터 전환까지의 퍼널 구조, 핵심 후킹(Hook) → 문제 공감(Story) → 해결책 및 오퍼(Offer) → 행동 유도(CTA) 설계.
    *   **퍼스널 브랜딩**: 타겟 오디언스 정의, 차별화된 권위/경험 앵글, 핵심 메시지 기둥(Content Pillars), 원소스 멀티유즈(OSMU) 확산 흐름.
    *   독자 여정이나 설득 흐름을 한눈에 보여주는 텍스트 다이어그램(ASCII 또는 Mermaid)을 포함하십시오.
3.  **Update Draft Artifact (MANDATORY)**: `design_draft.md`에 "Technical Plan" 섹션을 추가/업데이트합니다.
4.  **Notify & Verify (No Chat Dumps)**: 채팅에 요약과 파일 링크를 안내하고 `Technical Plan`에 대한 Section Approval Gate를 제시합니다.

### Step 4: Draft Alternatives (검토 및 기각한 대안 작성)

1.  **Review Alternatives**: 이전 브리프의 `Alternatives Weighed` 섹션에서 검토 후 기각했던 대안들을 읽어옵니다. 기억에 의존하지 말고 반드시 파일을 확인하십시오. 만약 해당 섹션이 없다면 사용자에게 어떤 대안들을 고려했다가 제외했는지 물어보십시오.
2.  **Draft Content**: "Alternatives Considered" 섹션을 작성합니다.
3.  **Detail Reasons**: 각 대안마다 왜 채택하지 않았는지 구체적인 비즈니스 사유(예: 1인 운영 리소스 초과, 타겟 전환율 낮음, 예산 과다 등)를 명시합니다. 작성자의 결정은 `Rejected by author: [구체적 사유]`로 기록하며, 실제 출처 링크 없이 `Rejected by maintainer`라고 쓰지 마십시오.
4.  **Update Draft Artifact (MANDATORY)**: `design_draft.md`에 "Alternatives Considered" 및 "Non-Functional Requirements"(브랜드 톤앤매너, 예산/시간 한도, 리스크 플랜B) 섹션을 추가합니다.
5.  **Notify & Verify (No Chat Dumps)**: 채팅에 요약과 파일 링크를 안내하고 `Alternatives Considered`에 대한 Section Approval Gate를 제시합니다.

### Step 5: Draft Implementation (상세 산출물 제작 명세 및 실행 계획 작성)

금붕어 검증이 끝난 직후 **추가 질문 없이 곧바로 최종 비즈니스 산출물(제안서 본문, 랜딩페이지 카피, 상세페이지 기획안, 링크드인/뉴스레터 원고 등)을 100% 완성할 수 있을 만큼** 구체적인 상세 제작 명세(`## Detailed Implementation Plan`)를 작성합니다:

1.  **Single Implementation List**: 만들어야 할 모든 최종 산출물과 실행 과제를 하나의 통합된 섹션(`## Detailed Implementation Plan`) 아래에 정리합니다.
2.  **Enumerate Files (국문/영문 등 모든 산출물 파일 명시)**: 타겟 시장(`KR` / `Global` / `Both`)에 맞춰 새로 생성(`Create`), 수정(`Modify`), 또는 삭제(`Delete`)할 **모든 최종 산출물 파일 경로**(예: `deliverables/kr_proposal.md`, `deliverables/en_global_pitch.md`, `deliverables/kr_landing_copy.md`, `deliverables/en_landing_copy.md`, `deliverables/en_linkedin_post.md`)와 함께 업데이트해야 할 관련 문서나 템플릿을 빠짐없이 나열하십시오.
3.  **Rationale & Localized Section-by-Section Blueprint**: 각 산출물 파일마다 *무엇을* 작성/수정하며 *왜* 필요한지, 그리고 **해당 산출물 안에 들어갈 세부 목차, 시장별 현지화된 핵심 카피/헤드라인(국문 및/또는 Native English), 포함할 구체적 수치·사례, 오퍼 조건(KRW/USD), CTA 문구**를 단순 직역이 되지 않도록 빈틈없이 명세하십시오.
4.  **Format Links**: 알려진 전체 상대 경로(예: `./deliverables/proposal.md` 또는 `../brand/guide.md`)로 명확히 표기하십시오.
5.  **NO FINAL DELIVERABLE GENERATION YET (NO CODE)**: 이 기획서 초안 단계에서는 아직 실제 최종 산출물 파일을 별도로 생성하지 마십시오. 산출물의 구조, 목차, 핵심 카피 설계, 스키마는 `design_draft.md` 안에 명세합니다.
6.  **Update Draft Artifact (MANDATORY)**: `design_draft.md`에 "Detailed Implementation Plan" 섹션을 추가합니다.
7.  **Notify & Verify (No Chat Dumps)**: 채팅에 요약과 파일 링크를 안내하고 `Detailed Implementation`에 대한 Section Approval Gate를 제시합니다.

### Step 6: Write Design Document Finalization & Deliverable Handoff (기획서 최종 저장 및 산출물 제작 연결)

모든 섹션이 승인되어 기획서를 최종 저장할 때는 다음 규칙을 엄격히 따르십시오:

1.  **Reconcile against the Predecessor Context (초기 브리프와의 모순 대조 점검)**:
    Step 1에서 읽었던 초기 브리프(`concepts_brief.md`)를 다시 읽고 현재 완성된 `design_draft.md`와 대조하십시오. 금붕어는 초기 브리프를 읽지 않으므로, 이 대조 검증은 오직 코끼리만 할 수 있습니다.
    *   **Contradiction (모순)**: 브리프에서 결정했던 사항과 초안의 결정이 서로 다른 경우 모든 항목을 보고하십시오.
    *   **Addition (중대한 추가)**: 브리프에 없던 내용이 초안에 추가되었는데, 전체 비즈니스 구조나 외부 인터페이스/예산/핵심 오퍼를 바꾸는 경우 보고하십시오.

    모순이 발견되면 브리프 문구와 초안 문구를 한 줄씩 인용하여 보여주고, 어느 쪽으로 확정할지 사용자에게 먼저 물으십시오. 임의로 혼자 수정하지 마십시오. 모순이 해결된 후에만 Step 6.2로 넘어갑니다. 차이점이 없다면 한 줄로 이상 없음을 알리고 진행하십시오.

2.  **Target Directory**: 사용자가 저장 경로를 지정하지 않았다면 기존 기획서 폴더(`docs/designs/` 등)를 탐색하여 `[DiscoveredPath]`(기본값 `docs/designs/`)로 바인딩하고 아래 단일 선택 게이트를 제시하십시오:

    ```json
    {
      "questions": [
        {
          "question": "Where should the design be saved?",
          "options": [
            "Save to detected directory: [DiscoveredPath]",
            "Specify custom target directory"
          ],
          "is_multi_select": false
        }
      ]
    }
    ```

    *   **Option 0**: `<DESIGN_DIR>`를 `[DiscoveredPath]`로 설정합니다.
    *   **Option 1**: 사용자가 입력한 경로를 `<DESIGN_DIR>`로 설정합니다. 입력이 비어 있다면 채팅으로 `"Please specify the custom target directory path where you would like to save the design document."`라고 묻고 대기합니다.

3.  **Update Sitemap**: `<DESIGN_DIR>/SITEMAP.md`가 존재하면 새 기획서를 등록합니다.

4.  **Write doc**: 최종 확정된 `design_draft.md`의 내용을 `<DESIGN_DIR>/<FEATURE>.md` 경로에 저장합니다.

5.  **Clean Formatting**: 깔끔한 마크다운 포맷, 표 문법, 코드/예시 블록이 유지되었는지 확인합니다.

6.  **Handoff (금붕어 검증 및 최종 산출물 제작 안내)**: 최종 저장된 마스터 기획서의 클릭 가능한 링크를 제공하고, 먼저 호스트 명령어 형태(예: Claude Code 및 agy CLI에서는 `/egm:egm-review <DESIGN_PATH>`, Codex에서는 스킬 피커에서 `egm-review` 선택)로 **`egm-review` 스킬을 실행하여 무맥락 금붕어 검증을 거친 뒤, 검증이 완료된 명세(`Detailed Implementation Plan`)에 따라 최종 비즈니스 산출물 파일들을 즉시 제작(Implement)**하라고 안내하십시오.
