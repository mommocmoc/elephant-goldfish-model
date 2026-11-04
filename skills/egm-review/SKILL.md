---
name: egm-review
license: Apache-2.0
disable-model-invocation: true
description: >-
  비즈니스용 코끼리-금붕어 모델(EGM-Biz)의 5단계 무맥락 금붕어 검증 프로토콜을 오케스트레이션합니다.
  Initial Zoom Out Review(거시적 본질 점검), Comprehension Review(무맥락 독자 복창 테스트),
  Design Critique(수익성·구조·카피 비평), Intermediate Zoom Out Review(중간 본질 점검),
  Implementation Readiness(산출물 즉각 제작 준비도 검증)의 5단계를 수행합니다.
---

# Review, Validation & Automated Remediation (5단계 금붕어 무맥락 검증 및 보완)

When running in Codex or ChatGPT Work, first read [Codex integration](../../docs/codex.md).
In Claude Code or Cowork, read `${CLAUDE_PLUGIN_ROOT}/docs/claude.md` first.

작성자와 코끼리가 함께 만든 비즈니스 기획서 또는 산출물 초안을 **사전 대화 맥락이 전혀 없는 전문 금붕어(Goldfish) 서브에이전트들**에게 넘겨 다각도로 검증하고, 발견된 허점을 수정한 뒤 최종 비즈니스 산출물을 제작할 수 있도록 돕습니다.

## Delegation

금붕어(Goldfish)는 이전 대화 기록이 전혀 없는 깨끗한(fresh) 컨텍스트에서 실행됩니다. 하네스의 서브에이전트 도구(Antigravity: `invoke_subagent`; Claude Code: `Agent`; Codex: `spawn_agent`)를 사용하여 각 서브에이전트를 호출하십시오. 각 서브에이전트는 `agents/<file>.md`에 정의되어 있으며, 프론트매터의 `name`(예: `egm-zoom-out`)이 서브에이전트 타입 이름입니다. 명명된 서브에이전트를 지원하지 않는 환경에서는 범용 서브에이전트를 생성하고 해당 페르소나 파일을 먼저 읽도록 지시하십시오. 서브에이전트 중첩(nesting)이 불가능한 환경에서는 synthesizer 대신 직접 병렬 호출(direct dispatch)을 사용하십시오(`docs/harnesses.md` 참조). 서브에이전트 도구가 아예 없다면 즉시 중단하고 사용자에게 "동일한 대화창에서의 셀프 리뷰는 금붕어(무맥락) 검증이 아닙니다"라고 알리십시오.

## Execution Constraints (CRITICAL)

1.  **Artifact-First Delivery (파일 우선 저장 필수)**: 긴 리뷰 리포트, 분석 결과, 수정 초안을 채팅창에 그대로 출력하는 것은 **엄격히 금지**됩니다. 반드시 파일 쓰기 도구를 사용해 작업 디렉토리(또는 아티팩트 디렉토리)에 리포트 파일로 저장한 뒤, 채팅창에는 파일 경로와 핵심 요약만 안내하십시오.
2.  **Plain-Language Standard (명확하고 구체적인 비즈니스 언어)**: 모든 리포트와 소통은 추상적인 유행어나 번역투를 배제하고 명확하고 직관적인 한국어로 작성하십시오. 근거 없는 과장 수식어(*seamless*, *robust*, *blazingly fast*, *혁신적인*, *압도적인*)를 사용하지 마십시오.
3.  **Default to Stress-Testing (스트레스 테스트 기본화)**: 기본값으로 동의하지 마십시오. 기획안과 산출물의 가장 약한 고리, 무맥락 독자가 오해할 지점, 리소스 병목을 집요하게 찾아내십시오.
4.  **No Glazing (빈말 칭찬 금지)**: "훌륭한 기획입니다", "완벽합니다" 같은 빈말 칭찬을 배제하십시오.
5.  **Critique First (비판 우선)**: 무엇이 빠져 있고 위험한지부터 먼저 보고하십시오.
6.  **Reject Echo-Chamber Framing (앵무새 복창 금지)**: 작성자의 가정을 무비판적으로 수용하지 마십시오.
7.  **Generate Counter-Arguments (반론 제시)**: 전략마다 고객이나 심사위원이 거절할 수 있는 가장 강력한 반론을 제시하십시오.
8.  **Earn Your Agreement**: 철저한 객관적 검증을 통과한 후에만 승인하십시오.
9.  **Banned Adjectives (No Flattery)**: 사용자의 입력이나 결정에 대해 "brilliant", "great", "smart", "correct", "perfect", "excellent", "spot-on" 같은 칭찬 형용사를 절대 사용하지 마십시오.
10. **Banned Apologies (불필요한 사과 금지)**: "죄송합니다(I apologize, I am sorry)" 같은 사과 대신 수정된 논리와 대안을 즉시 제시하십시오.
11. **Interaction Invariants**: 모든 인터랙티브 게이트(`ask_question` 등)는 한 턴에 딱 하나의 질문(`len(questions) == 1`)만 포함해야 하며, 해당 턴의 유일한 도구 호출이어야 하고, 옵션에 `(Recommended)` 접두사를 붙이지 마십시오.

## Inputs

*   **`TargetFile`**: 검증할 기획서 또는 문서의 경로.
*   **`GoldfishContext`** (선택): 금붕어들이 배경지식(Baseline)으로 함께 읽어야 할 참고 파일 목록(예: 브랜드 가이드, 기존 상품 스펙 등). 생략된 경우 Step 1.3에서 자동 추출된 내부 링크 파일들만 포함됩니다.
*   **`Step` (선택)**: 실행할 특정 파이프라인 프리셋 또는 스테이지(예: `interactive`, `uninterrupted`, `feature_spec`, `implementation`, 또는 개별 스테이지 `5`, `6`, `7`, `8`, `9`). 생략 시 사용자에게 선택 게이트를 제시합니다.

## Workflow

### Step 1: Intake & Setup (검증 대상 문서 및 참고 자료 확인)

1.  `TargetFile`이 제공되었는지 확인합니다. 누락된 경우:
    *   문서 구조를 분석하고 전략 파트와 상세 산출물 제작 계획 파트를 구분하기 위해 대상 문서 경로가 필요함을 설명합니다.
    *   일반 채팅 메시지로 파일 경로를 물어보고(`ask_question` 같은 객관식 도구는 절대 사용하지 마십시오) 턴을 종료합니다.
2.  `TargetFile`이 확인되면 `GoldfishContext`가 제공되었는지 확인합니다(빈 리스트가 전달되었거나 사용자가 프롬프트에서 추가 참고 파일 없이 진행하라고 명시한 경우도 제공된 것으로 간주). 누락된 경우:
    *   금붕어에게 함께 읽힐 참고 파일 목록(`GoldfishContext`)은 선택 사항이지만, 브랜드 가이드나 기존 자료를 넣어주면 기준점을 잡는 데 도움이 됨을 안내합니다.
    *   문서 내에 링크된 작업 디렉토리 내부 파일들은 자동으로 추가되며, 외부 링크나 URL은 승인이 필요함을 설명합니다.
    *   일반 채팅 메시지로 추가 참고 파일을 지정할지 물어보고(`ask_question` 사용 금지) 턴을 종료합니다.
3.  **Linked Files (Step 2 프리셋 선택 후, Stage 6를 제외한 첫 스테이지 직전에 1회 실행)**:
    Intake 단계에서는 실행하지 마십시오. Step 2 프리셋이 결정된 후 `GoldfishContext`를 사용하는 첫 스테이지 직전에 딱 한 번 실행합니다. `TargetFile`을 읽어 앵커 전용 링크(`#...`)는 건너뛰고, 상대 경로 링크는 대상 문서 폴더 기준으로 해석합니다. 심볼릭 링크를 따라간 실제 경로가 작업 디렉토리 내부에 머무는 존재하는 파일들은 자동으로 `GoldfishContext`에 추가합니다. 작업 디렉토리 외부 파일이거나 URL인 경우 채팅에 목록을 보여주고 어떤 것을 포함할지 물은 뒤 턴을 종료합니다(응답할 사람이 없는 헤드리스 실행에서는 모두 제외하고 계속 진행). 승인된 URL은 리뷰어가 직접 열 수 없으므로 작업 디렉토리(또는 아티팩트 디렉토리)에 파일로 저장한 뒤 그 절대 경로를 전달합니다. 저장할 수 없다면 로컬 사본을 요청하고 턴을 종료합니다. **단, Stage 6(무맥락 독자 복창 테스트)에는 `GoldfishContext`를 전달하지 않고 오직 문서 단독으로만 이해되는지 시험합니다.**

### Step 2: Curated Pipeline Preset Selection (검증 모드 선택)

1.  **Preset Gate Check**:
    *   호출 시 `Step` 파라미터가 명시적으로 주어졌다면 해당 프리셋/서브플로우로 바로 진입합니다.
    *   `Step`이 생략되었다면 아래의 단일 선택 게이트를 제시합니다:
2.  **Modal Invariants & Wire Payload**:
    *   반드시 `len(questions) == 1`, `is_multi_select: false`여야 하며, 해당 턴의 유일한 도구 호출이어야 합니다.

        ```json
        {
          "questions": [
            {
              "question": "Select the review execution mode for this document:",
              "options": [
                "Interactive review with all review rubrics (step-by-step, check in after each review)",
                "Consolidated report with all review rubrics (runs full flow without stopping)",
                "Feature spec only (evaluate design without considering implementation)",
                "Implementation plan only (evaluate design for implementation readiness)"
              ],
              "is_multi_select": false
            }
          ]
        }
        ```

3.  **Action Dispatch Based on Preset**:
    *   **Preset 0 (`Interactive review with all review rubrics` - 단계별 대화형 전체 검증)**: Step 3에서 전략/실행 경계를 나눈 뒤 Stage 5 → 6 → 7 → 8 → 9를 단계별 체크포인트와 함께 순차 실행합니다.
    *   **Preset 1 (`Consolidated report with all review rubrics` - 논스톱 통합 리포트 생성)**: 중간 멈춤 없이 5단계를 모두 실행하여 `consolidated_review_report.md` 하나로 통합 보고서를 생성합니다.
    *   **Preset 2 (`Feature spec only` - 상위 비즈니스 전략/기획만 검증)**: Step 3 분할 탐색을 건너뛰고 문서 전체(`1..End`)에 대해 Stage 5, 6, 7, 8만 수행한 뒤 종료합니다.
    *   **Preset 3 (`Implementation plan only` - 상세 산출물 제작 계획만 즉시 검증)**: Step 3에서 `DetailedRange`를 확인한 뒤 곧바로 Stage 9(산출물 즉각 제작 준비도 검증)로 점프합니다.

### Step 3: Semantic Split Discovery (상위 기획 파트 vs 상세 산출물 명세 파트 분할)

1.  **Split Bypass Check**: Preset 2(`Feature spec only`)가 선택되었다면 이 단계를 완전히 건너뛰고 `FeatureRange = 1..End`로 설정한 뒤 Step 4로 이동합니다.
2.  **Inspect Document**: `TargetFile`을 읽어 전체 구조와 라인 수를 파악합니다.
3.  **Discover Split Boundary**: 상위 전략 파트와 하위 상세 제작/실행 파트를 나누는 기준 헤더인 `## Detailed Implementation Plan` (또는 동의어: `## Implementation Plan`, `## Work Breakdown`, `## Implementation Steps`)을 찾습니다.
    *   **Standard Layout Detected**: 해당 헤더가 $L$번째 라인에서 명확히 발견되면:
        *   자동으로 상위 기획 범위 `FeatureRange = 1..(L-1)`, 하위 산출물 제작 명세 범위 `DetailedRange = L..End`로 할당합니다.
        *   채팅에 간결한 확인 메시지를 출력합니다: `"Discovered semantic boundary at line L. FeatureRange: 1..(L-1), DetailedRange: L..End."`
        *   별도 확인 모달 없이 곧바로 Step 4로 진행합니다.
    *   **Ambiguous Boundary**: 후보 헤더가 여러 개이거나 모호하면 가장 유력한 분할 라인을 Option 0으로, 직접 라인 범위를 입력하는 옵션을 Option 1로 하는 단일 선택 게이트를 제시합니다.
    *   **No Implementation Plan Found**: 상세 실행/제작 계획 섹션이 없다면 사용자에게 알리고 `FeatureRange = 1..End`, `DetailedRange = N/A`로 설정한 뒤 Stage 5~8만 진행합니다.

### Step 4: Staging Strategy (5단계 금붕어 검증 파이프라인 실행)

1.  `FeatureRange`와 `DetailedRange`가 설정되었는지 확인합니다.
2.  선택된 프리셋에 따라 파이프라인을 실행합니다:
    *   **Stage 5 (Initial Zoom Out Review)**: 거시적 본질(80/20), 과잉 기획 여부, 기존 자산 재활용 가능성 검증 (`zoom_out_report.md`)
    *   **Stage 6 (Comprehension Test)**: **무맥락 독자 요약 복창 테스트** — 아무 배경지식 없는 금붕어가 문서를 읽고 "무엇을 제안하는지" 스스로 요약하게 하여 내 머릿속 맥락이 제대로 전달되었는지 확인 (`comprehension_report.md`)
    *   **Stage 7 (Critic Review)**: 수익성/리소스(`egm-design`), 문서 구조(`egm-structural`), 비즈니스 문장/설득력(`egm-style`) 3마리 금붕어의 정밀 비평 (`critic_report.md`)
    *   **Stage 8 (Intermediate Zoom Out Review)**: 수정 과정에서 사족이 붙지 않았는지 다시 큰 그림 점검 (`intermediate_zoom_out_report.md`)
    *   **Stage 9 (Implementation Readiness Test)**: 이 문서만 보고 즉시 최종 산출물(제안서, 카피, 브랜딩 원고 등)을 제작할 수 있는지 실무 준비도(`egm-implementation-readiness`), 팩트 정합성(`egm-codebase-details`), 문장 스타일(`egm-style`) 최종 검증 (`implementation_readiness_report.md`)

--------------------------------------------------------------------------------

## Subflows

### Subflow Checkpoint Gates & Pause/Resume Protocol

모든 대화형 리뷰 스테이지는 리포트 파일 저장 후 아래와 같은 단일 선택 게이트(`len(questions) == 1`, `is_multi_select: false`)로 마무리됩니다:

1.  **Stage Checkpoint Patterns**:
    *   **Stages 5, 6, 8 (거시적 본질 및 무맥락 이해도 체크포인트)**:
        *   Option 0: `Proceed to next stage: Stage [N+1] ([Next Stage Name])` (Stage 8 종료 시에는 `Conclude review and begin detailed implementation planning`).
        *   Option 1: `I will update doc first (pause session; notify me in chat when edits are complete to re-run)`.
        *   Option 2: `Pause Review (save progress and exit)`.
    *   **Stages 7, 9 (비평 반영 및 산출물 제작 준비 완료 체크포인트)**:
        *   Option 0: `Proceed to next stage: Stage [N+1] ([Next Stage Name])` (Stage 9 종료 시에는 `Conclude review and finalize document`).
        *   Option 1: `Resolve findings with agent (evaluate findings, propose fixes, and apply approved edits)`.
        *   Option 2: `I will update doc manually (pause session; notify me in chat when edits are complete to re-run)`.
        *   Option 3: `Pause Review (save progress and exit)`.
2.  **Resolution Semantics (Stages 7, 9에서 Option 1 선택 시)**:
    *   **Hand-off to Resolver**: 코끼리 컨텍스트에서 `egm-resolve` 워크플로우를 실행하여 `critic_report.md` 또는 `implementation_readiness_report.md`의 지적 사항을 분류(`PROPOSE FIX` vs `CONTEST`)하고, `goldfish_review_report.md`를 작성한 뒤 승인된 수정안을 `TargetFile`에 자동 반영합니다.
    *   **Convergence**: 수정 반영 후 `TargetFile`을 다시 읽어 라인 범위를 재계산하고 다음 스테이지로 이동합니다(스테이지당 최대 1회 재감사 한도 적용).
3.  **Pause & Resume Semantics (수동 문서 수정)**:
    *   사용자가 직접 문서를 수정하겠다고 선택하면 즉시 턴을 종료하고 대기합니다. 사용자가 수정 완료를 알리면 파일을 다시 읽어 라인 변화를 반영하고 현재 스테이지를 재실행합니다(최대 1회 재감사).
4.  **Graceful Halt Semantics (`Pause Review`)**:
    *   지금까지 생성된 모든 리포트 파일을 보존한 채 즉시 실행을 멈춥니다.

### Subflow: Uninterrupted Full Flow (논스톱 전체 검증 및 통합 리포트)

1.  **Uninterrupted Stage Execution**: 중간 체크포인트 없이 5개 스테이지를 순차 실행하며 각 리포트를 파일로 저장합니다(Stage 6를 제외한 모든 스테이지에 `GoldfishContext` 전달):
    *   **Step 5 (Initial Zoom Out)**: `full file` 대상 `[egm-zoom-out]` 실행 → `zoom_out_report.md` 저장.
    *   **Step 6 (Comprehension Test)**: `FeatureRange` & `DetailedRange` 대상 `egm-comprehension` 서브에이전트 직접 호출(synthesizer 거치지 않음) → 요약 복창을 포함한 전체 결과를 `comprehension_report.md`에 저장.
    *   **Step 7 (Critic Review)**: `FeatureRange` 대상 `[egm-design, egm-structural, egm-style]` 실행 → `critic_report.md` 저장.
    *   **Step 8 (Intermediate Zoom Out)**: `full file` 대상 `[egm-zoom-out]` 실행 → `intermediate_zoom_out_report.md` 저장.
    *   **Step 9 (Implementation Readiness Test)**: `DetailedRange` 대상 `[egm-implementation-readiness, egm-codebase-details, egm-style]` 실행 → `implementation_readiness_report.md` 저장.
2.  **Cross-Stage Synthesis & Deduplication**:
    *   5개 리포트의 모든 지적 사항을 수집하여 우선순위(🔴 Blocker, 🟡 Major, 🟢 Nit)별로 정리하고 추적 매트릭스(Traceability Matrix)를 구성합니다.
3.  **Deliverable Delivery**: `consolidated_review_report.md` 파일로 저장합니다.
4.  **Notify User**: 채팅으로 `consolidated_review_report.md` 경로를 안내합니다.

### Subflow: Zoom Out Review (Stage 5 & Stage 8)

1.  **Delegate to Synthesizer Subagent**: `full file`에 대해 `resources/document_standards/zoom-out-standard.md`를 적용하여 `egm-synthesizer`(또는 `agents/zoom_out.md`의 `egm-zoom-out`)를 실행합니다. Stage 8에서는 `zoom_out_report.md` 경로를 함께 전달하여 Stage 6~7 수정 과정에서 추가된 복잡성이나 사족을 중점 점검합니다. 두 스테이지 모두 `GoldfishContext`를 전달합니다.
2.  **Delivery**: 결과를 `zoom_out_report.md` (Stage 5) 또는 `intermediate_zoom_out_report.md` (Stage 8)에 저장합니다.
3.  **Notify User (No Chat Dumps)**: 리포트 파일 경로만 간결하게 안내합니다.
4.  **Checkpoint Gate**:
    *   **Stage 5**: Question: `"Stage 5 (Initial Zoom Out Review) completed. Findings written to zoom_out_report.md. How would you like to proceed?"`, Option 0: `"Proceed to next stage: Stage 6 (Comprehension Test)"`.
    *   **Stage 8 (Feature Spec Gates Only)**: Question: `"Stage 8 (Intermediate Zoom Out) completed. High-level architecture verified. How would you like to proceed?"`, Option 0: `"Conclude review and begin detailed implementation planning"`.
    *   **Stage 8 (Interactive Full Flow)**: Question: `"Stage 8 (Intermediate Zoom Out) completed. Findings written to intermediate_zoom_out_report.md. How would you like to proceed?"`, Option 0: `"Proceed to next stage: Stage 9 (Implementation Readiness Test)"`.
    *   모든 경우 Option 1: `"I will update doc first (pause session; notify me in chat when edits are complete to re-run)"`, Option 2: `"Pause Review (save progress and exit)"`를 제공합니다.

### Subflow: Comprehension Test (Stage 6 - 무맥락 독자 요약 복창 테스트)

1.  **Spawn the Reviewer Directly**: `FeatureRange` & `DetailedRange`에 대해 별도 문서 표준 없이 `agents/comprehension.md`의 `egm-comprehension` 서브에이전트를 **직접 호출**하십시오. Synthesizer를 사용하지 마십시오(작성자가 금붕어의 요약 복창문을 원문 그대로 읽고 자신의 의도가 제대로 전달되었는지 확인해야 하기 때문입니다).
2.  **Delivery**: 두 개의 요약 복창 헤딩(`What this proposes`, `How the existing system works, per this document`)을 가장 먼저 원문 그대로 포함하고, 이어서 발견된 이해도 결함을 담아 `comprehension_report.md`에 저장합니다. 결함이 없더라도 복창문은 반드시 유지합니다.
3.  **Notify User (No Chat Dumps)**: `comprehension_report.md` 경로를 안내합니다.
4.  **Checkpoint Gate**:
    *   Question: `"Stage 6 (Comprehension Test) completed. Findings written to comprehension_report.md. How would you like to proceed?"`
    *   Option 0: `"Proceed to next stage: Stage 7 (Critic Review)"`
    *   Option 1: `"I will update doc first (pause session; notify me in chat when edits are complete to re-run)"`
    *   Option 2: `"Pause Review (save progress and exit)"`

### Subflow: Critic Review (Stage 7 - 비즈니스·구조·설득력 비평)

1.  **Invoke Antagonistic Review Loop**: `FeatureRange`에 대해 리뷰어 `[egm-design, egm-structural, egm-style]`, 표준 `{"egm-design": "feature-spec-standard", "egm-structural": "feature-spec-standard"}`, 그리고 `GoldfishContext`를 전달하여 `egm-review-loop`를 실행합니다.
2.  **Notify User (No Chat Dumps)**: `critic_report.md` 경로를 안내합니다.
3.  **Checkpoint Gate**:
    *   Question: `"Stage 7 (Critic Review) completed. Findings written to critic_report.md. How would you like to proceed?"`
    *   Option 0: `"Proceed to next stage: Stage 8 (Intermediate Zoom Out Review)"`
    *   Option 1: `"Resolve findings with agent (evaluate findings, propose fixes, and apply approved edits)"`
    *   Option 2: `"I will update doc manually (pause session; notify me in chat when edits are complete to re-run)"`
    *   Option 3: `"Pause Review (save progress and exit)"`
4.  **Remediation & Resolution (Option 1)**: `egm-resolve`를 호출해 승인된 수정을 `TargetFile`에 반영하고 라인 범위를 재계산한 뒤 Stage 8로 진행합니다.

### Subflow: Implementation Readiness Test (Stage 9 - 산출물 즉각 제작 준비도 검증 및 최종 산출물 제작 연결)

1.  **Invoke Antagonistic Review Loop**: `DetailedRange`에 대해 리뷰어 `[egm-implementation-readiness, egm-codebase-details, egm-style]`, 표준 `{"egm-implementation-readiness": "implementation-readiness-standard", "egm-codebase-details": "implementation-readiness-standard"}`, 그리고 `GoldfishContext`를 전달하여 `egm-review-loop`를 실행합니다.
2.  **Notify User (No Chat Dumps)**: `implementation_readiness_report.md` 경로를 안내합니다.
3.  **Terminal Checkpoint Gate**:
    *   Question: `"Stage 9 (Implementation Readiness Test) completed. Report written to implementation_readiness_report.md. How would you like to proceed?"`
    *   Option 0: `"Conclude review and finalize document"`
    *   Option 1: `"Resolve findings with agent (evaluate findings, propose fixes, and apply approved edits)"`
    *   Option 2: `"I will update doc manually (pause session; notify me in chat when edits are complete to re-run)"`
    *   Option 3: `"Pause Review (save progress and exit)"`
4.  **Remediation & Resolution (Option 1)**: `egm-resolve`를 호출해 승인된 수정을 `TargetFile`에 반영합니다.
5.  **Deliverable Production Handoff (최종 산출물 제작 실행)**: Stage 9가 최종 완료(`Conclude review and finalize document`)되면, 이제 기획서가 금붕어 검증까지 완벽히 통과했음을 알리고, 기획서의 `## Detailed Implementation Plan`에 명시된 **최종 비즈니스 산출물 파일들(예: 사업기획서/제안서 완성본, 랜딩페이지/상세페이지 카피 원고, 퍼스널 브랜딩 포스팅 등)을 즉시 생성해 줄지** 사용자에게 안내하여 실제 산출물 제작까지 매끄럽게 완수하십시오.
