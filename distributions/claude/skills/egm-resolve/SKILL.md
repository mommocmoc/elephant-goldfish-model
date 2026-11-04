---
name: egm-resolve
license: Apache-2.0
disable-model-invocation: true
description: >-
  비즈니스용 코끼리-금붕어 모델(EGM-Biz)의 코끼리 피드백 반영 및 협상 스킬입니다. 무맥락 금붕어들이 제기한
  결함 리포트를 코끼리의 전체 비즈니스 맥락에서 평가하여 수정안(PROPOSE FIX), 반론(CONTEST),
  작성자 확인 요청(ESCALATE)으로 분류하고, 승인된 수정을 기획서 및 산출물에 자동 반영합니다.
---

# Goldfish Review: Resolve (금붕어 피드백 조율 및 자동 반영)

When running in Codex or ChatGPT Work, first read [Codex integration](../../docs/codex.md).
In Claude Code or Cowork, read `${CLAUDE_PLUGIN_ROOT}/docs/claude.md` first.

## Overview

이 스킬은 전체 비즈니스 배경과 의도를 기억하고 있는 **코끼리(Elephant) 컨텍스트**에서 실행됩니다. 무맥락 금붕어(Goldfish) 리뷰어들이 쏟아낸 피드백을 받아, 타당한 지적은 구체적인 문장/구조 수정안(`PROPOSE FIX`)으로 만들고, 금붕어가 오해한 부분은 근거를 들어 방어(`CONTEST`)하며, 작성자의 결단이 필요한 암묵적 비즈니스 판단은 질문으로 올려(`ESCALATE`) 빠르고 정확하게 문서를 개선합니다.

## Prerequisites & Context Strategy

*   **Context Recommendation (Elephant Memory)**: 이 스킬은 반드시 대상 문서를 기획하고 작성했던 메인 대화 세션(코끼리)에서 실행되어야 이전 의사결정 맥락을 살려 고품질의 수정을 수행할 수 있습니다.

## Inputs

호출자 또는 사용자가 다음 파라미터를 제공합니다:

*   **`TargetFile`**: 수정할 대상 기획서 또는 산출물 파일 경로.
*   **`LineRange`**: 대상 라인 범위 (또는 `full file`).
*   **`FindingsList`**: 금붕어 리뷰어들이 보고한 지적 사항 목록.

## Execution Workflow

### Step 1: Intake Validation & Gather Context

1.  **Hybrid Execution UX**:
    *   *Automated Mode*: 모든 파라미터가 주어졌다면 서브플로우로서 즉시 실행합니다.
    *   *Standalone Human Mode*: 파라미터가 생략되었다면 친절한 한국어 안내로 필요한 입력을 한 번에 하나씩 질문하여 확인합니다.
2.  `TargetFile`의 `LineRange`를 읽고 `FindingsList`를 파싱합니다.

### Step 2: Evaluate & Categorize (4가지 카테고리로 분류)

각 지적 사항을 정확히 다음 중 하나로 분류하십시오:

*   **CONTEST (반박/유지)**: 금붕어의 지적이 부정확하거나 이미 문서/참고자료에 명시된 팩트를 오해한 경우입니다. 반드시 리뷰어가 읽을 수 있었던 파일 위치나 문서 내 기존 결정 사항을 **근거로 인용**하십시오.
*   **ESCALATE (작성자 의사결정 요청)**: 금붕어의 지적은 틀렸지만, 그 근거가 문서나 파일 어디에도 적혀 있지 않은 경우(예: 1인 사업가의 개인적 선호, 구두 합의, 아직 안 적은 브랜드 철학)입니다. 임의로 고치지 말고 작성자에게 결정을 넘기십시오.
*   **PROPOSE FIX (수정안 제안)**: 금붕어의 지적이 타당한 경우입니다. 무맥락 독자도 완벽히 이해할 수 있도록 구체적인 대체 문장이나 표/목차 수정안을 작성하십시오.
*   **DECLINE (사소한 지적 생략)**: 사소한 지적(Nit)이어서 굳이 고칠 필요가 없는 경우 기록만 남기고 수정을 생략합니다(단, 고치는 것이 문서를 더 좋게 만든다면 Nit라도 `PROPOSE FIX`로 제안할 수 있습니다).

다음 원칙을 엄격히 적용하십시오:

*   **반박(CONTEST)은 반드시 검증 가능해야 합니다.** 단순히 "원래 이렇게 의도한 것임"은 근거가 될 수 없습니다. 문서나 파일에서 근거를 인용할 수 없다면 `ESCALATE` 또는 `PROPOSE FIX`를 사용하십시오.
*   **금붕어만 만족시키기 위한 불필요한 사족을 넣지 마십시오.** 실제 인간 독자(고객, 심사위원, 외주 파트너)에게도 꼭 필요한 맥락일 때만 문서에 추가하십시오.
*   **무맥락 독자의 이해도 결함(Comprehension gap)은 절대 반박(CONTEST)하지 마십시오.** 처음 읽는 독자가 이해하지 못했다면 그것은 100% 문서의 설명이 부족한 것입니다.
*   **심각도(Severity)를 임의로 낮추지 마십시오.** Blocker가 Nit라고 생각되면 등급을 바꾸는 대신 이유를 들어 `CONTEST` 하십시오.

### Step 3: Report Fixes (`goldfish_review_report.md` 작성)

분석 결과를 작업 디렉토리(또는 아티팩트 디렉토리)의 `goldfish_review_report.md` 파일에 저장하십시오:

```markdown
# Goldfish Annotated Review Report (금붕어 지적 사항 및 코끼리 수정 제안서)

**Target:** `[TargetFile]` (`[LineRange]`)

## Prioritized Findings & AI Suggestions

{{range findings, ordered by Severity}}
### [이슈 1] [`[ReviewerPersona]`] - Severity: [Blocker | Major | Nit]
*   **Affected Scope:** [Lines Start-End]([RelativePath]#L[Start]-L[End]) ([Sub-header])
*   **Discrepancy Description:** [금붕어의 지적 내용]
*   **Rationale:** [금붕어의 지적 근거]
*   ***AI Suggestion:*** [CONTEST | ESCALATE | PROPOSE FIX | DECLINE]
*   ***Justification:*** [코끼리가 왜 수정을 제안하거나 반박하는지에 대한 구체적 이유]
*   ***Proposed Edit:***
    *   Location: [수정할 라인/섹션 헤더]
    *   Change: [교체할 구체적인 한국어 문장 또는 수정 내역]
{{end}}
```

### Step 4: Interactive Resolution Gate (수정 반영 방식 선택)

만약 `ESCALATE` 항목이 있다면 채팅 메시지 가장 위에 먼저 보여주고 작성자의 판단을 구하십시오.

진행하기 전에 사용자에게 다음 안내 메시지를 출력하십시오: *"I have compiled the audit findings into `goldfish_review_report.md`. Please review the findings there and choose how you would like to proceed:* (검증 결과와 코끼리의 수정 제안을 `goldfish_review_report.md`에 정리했습니다. 어떻게 반영할지 선택해 주세요:)

-   **Comment directly on the file**: `goldfish_review_report.md` 파일을 보고 직접 코멘트를 남기거나 특정 항목만 골라 수정 지시.
-   **Auto accept**: `"auto accept"` (또는 `"전체 수락"`)이라고 답하면 반박(`CONTEST`) 항목을 제외한 모든 수정 제안(`PROPOSE FIX`)을 일괄 자동 적용.
-   **Continue to next phase**: `"continue"` 또는 `"no changes"` (또는 `"수정 없이 진행"`)라고 답하면 수정 없이 다음 단계로 이동.
-   **Interactive breakdown**: `"interactive breakdown"` (또는 `"하나씩 확인"`)이라고 답하면 각 항목을 채팅에서 하나씩 설명 듣고 결정.
-   **Custom instruction / Wildcard request**: 원하는 수정 방향이나 추가 지시를 자유롭게 입력."

이 메시지를 출력한 턴에는 **어떠한 도구도 추가로 호출하지 말고(zero tools)** 즉시 대기하여 사용자의 응답을 기다리십시오.

사용자의 응답에 따라 다음과 같이 처리합니다:

-   **"Auto accept" / "Accept all suggestions" / "전체 수락" / "Proceed"**: `PROPOSE FIX`로 분류된 모든 수정안을 파일 편집 도구로 `TargetFile`에 일괄 적용하고(`CONTEST`, `ESCALATE`, `DECLINE`은 건너뜀) Step 5로 이동합니다.
-   **"Continue to next phase" / "No changes" / "수정 없이 진행"**: 수정을 적용하지 않고 Step 5로 이동합니다.
-   **"Interactive breakdown" / "하나씩 확인"**: `DECLINE`을 제외한 모든 항목을 순차적으로 하나씩 채팅에 보여주고 결정을 받습니다:

    1.  각 항목 `i`에 대해 다음 마크다운을 출력합니다:

        ```markdown
        ### Breakdown Item [i]: [이슈 제목] [`[ReviewerPersona]`]
        * **Scope:** [Lines Start-End]([RelativePath]#L[Start]-L[End]) ([Sub-header])
        * **Description:** [지적 내용]
        * **AI Suggestion:** [CONTEST | ESCALATE | PROPOSE FIX]
        * **Explanation & Justification:** [상세 배경 설명 및 코끼리의 판단 근거]
        * **Proposed Edit:** [제안하는 구체적 수정 문구]
        ```

        그리고 사용자에게 결정을 묻고 대기합니다(도구 호출 없음):
        *   `PROPOSE FIX`인 경우: **Accept proposed fix (수정 수락)**, **Skip / ignore (건너뛰기)**, 또는 **Discuss (논의)**
        *   `CONTEST`인 경우: **Skip / agree with contest (반박 동의 및 유지)**, **Overrule contest (propose fix) (반박 기각 및 수정)**, 또는 **Discuss (논의)**
        *   `ESCALATE`인 경우: **Confirm (no change) (현재 유지)**, **Add the fact to the document (해당 맥락을 문서에 추가)**, 또는 **Discuss (논의)**

    2.  개별 항목 진행 중에는 파일을 바로 수정하지 않고 결정을 메모해 두었다가, 모든 항목 결정이 끝나면 승인된 수정 사항들을 한 번에 `TargetFile`에 반영하고 Step 5로 이동합니다.

-   **Custom instruction**: 사용자의 맞춤형 지시에 따라 수정하고 진행합니다.

### Step 5: Post-Resolution Synchronization & Exit

1.  **Re-verify Lines**: `TargetFile`이 수정되었다면 마크다운 구조와 문맥이 자연스러운지 다시 읽고 확인합니다.
2.  리포트 상단에 `Status: resolved`를 표시합니다.
3.  **Handoff**: 반영된 수정 건수, 반박 건수, 보류 건수를 요약하여 상위 호출자(예: `egm-review-loop` 또는 `egm-review`)에게 제어권을 반환합니다.
