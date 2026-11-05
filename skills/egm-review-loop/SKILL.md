---
name: egm-review-loop
license: Apache-2.0
disable-model-invocation: true
description: >-
  비즈니스용 코끼리-금붕어 모델(EGM-Biz)의 맞춤형 반복 검증 루프(Antagonistic-Review Loop) 스킬입니다.
  원하는 금붕어 리뷰어 조합과 문서 표준을 지정하여 기획서나 비즈니스 산출물(제안서, 랜딩페이지 카피,
  퍼스널 브랜딩 원고 등)을 반복 감사하고 승인된 수정을 자동으로 반영합니다.
---

# Goldfish Review: Loop (맞춤형 금붕어 반복 검증 및 개선 드라이버)

When running in Codex or ChatGPT Work, first read [Codex integration](../../docs/codex.md).
In Claude Code or Cowork, read `${CLAUDE_PLUGIN_ROOT}/docs/claude.md` first.

## Overview

이 스킬은 다중 에이전트 리뷰 루프를 이끄는 중앙 오케스트레이션 엔진입니다. 서로 다른 두 가지 기억 모델을 조합하여 문서를 벼려냅니다:

*   **The Goldfish (무맥락 검증단)**: 아무런 사전 대화 맥락 없이 격리된 새 컨텍스트에서 문서를 읽고, 작성자가 당연하게 여긴 빈틈·과장·모호함을 찾아내는 서브에이전트들입니다.
*   **The Elephant (맥락 보유 해결사)**: 비즈니스 전체 배경과 의도를 기억하는 메인 세션 에이전트로, 금붕어의 피드백을 평가하고 승인된 수정을 문서에 반영합니다.

## Delegation

금붕어(Goldfish)는 이전 대화 기록이 전혀 없는 깨끗한(fresh) 컨텍스트에서 실행됩니다. 하네스의 서브에이전트 도구(Antigravity: `invoke_subagent`; Claude Code: `Agent`; Codex: `spawn_agent`)를 사용하여 각 서브에이전트를 호출하십시오. 각 서브에이전트는 `agents/<file>.md`에 정의되어 있으며, 프론트매터의 `name`(예: `egm-zoom-out`)이 서브에이전트 타입 이름입니다. 명명된 서브에이전트를 지원하지 않는 환경에서는 범용 서브에이전트를 생성하고 해당 페르소나 파일을 먼저 읽도록 지시하십시오. 서브에이전트 중첩(nesting)이 불가능한 환경에서는 synthesizer 대신 직접 병렬 호출(direct dispatch)을 사용하십시오(`docs/harnesses.md` 참조). 서브에이전트 도구가 아예 없다면 즉시 중단하고 사용자에게 "동일한 대화창에서의 셀프 리뷰는 금붕어(무맥락) 검증이 아닙니다"라고 알리십시오.

## Inputs

호출자 또는 사용자가 다음 파라미터를 제공합니다:

*   **`TargetFile`**: 검증할 대상 기획서 또는 비즈니스 산출물 파일 경로.
*   **`SemanticTarget`**: 검증할 특정 섹션(예: *"Target the H2 block '## Technical Plan'"*) 또는 `full file`(전체 파일).
*   **`Reviewers`**: 투입할 금붕어 리뷰어 이름 목록.
*   **`Standards`**: 각 리뷰어에 매핑할 문서 품질 표준 목록.
*   **`GoldfishContext`**: 금붕어에게 기준점으로 함께 제공할 참고 파일 목록(빈 리스트도 제공된 것으로 간주하여 다시 묻지 않음). URL이 포함된 경우 리뷰어가 직접 열 수 없으므로 첫 라운드 시작 전에 작업 디렉토리(또는 아티팩트 디렉토리)에 파일로 저장하여 절대 경로를 전달합니다.

## Path & Standard Resolution Logic

`Reviewers`와 `Standards` 이름은 다음과 같이 매핑합니다:

*   **Reviewer Shortnames**:
    *   `comprehension` (`egm-comprehension`): 무맥락 독자/고객 이해도 금붕어 → `agents/comprehension.md`
    *   `design_critic` (`egm-design`): 수익성·예산·리스크 비평가 금붕어 → `agents/design_critic.md`
    *   `zoom_out` (`egm-zoom-out`): 본질(80/20) 및 과잉 기획 방지 금붕어 → `agents/zoom_out.md`
    *   `implementation_readiness` (`egm-implementation-readiness`): 산출물 제작 및 실행 준비도 금붕어 → `agents/implementation_readiness.md`
    *   `codebase_details` (`egm-codebase-details`): 팩트·수치·기존 자산 정합성 금붕어 → `agents/codebase_details.md`
    *   `structural` (`egm-structural`): 논리 구조·가독성 아키텍트 금붕어 → `agents/structural.md`
    *   `style` (`egm-style`): 비즈니스 카피·설득력 에디터 금붕어 → `agents/style.md`
*   **Standard Shortnames**: `feature-spec-standard`, `implementation-readiness-standard`, `zoom-out-standard`, `heilmeier-standard`, `re-implementation-standard`, `world-of-content-standard` → `resources/document_standards/{name}.md`.
*   **Custom Paths**: 슬래시(`/`)가 포함된 경우 직접 파일 경로로 읽습니다.

## Execution Workflow

### Step 1: Intake & Configuration UX

입력된 파라미터(`TargetFile`, `SemanticTarget`, `Reviewers`, `Standards`, `GoldfishContext`)를 확인합니다.

1.  **Identify Missing Inputs**: 누락된 필수 파라미터가 있는지 확인합니다.
2.  **Interactive Onboarding**:
    *   모든 입력이 주어졌다면 Step 2로 바로 진행합니다.
    *   일부가 누락되었다면 누락된 항목만 한 번에 하나씩 채팅으로 질문합니다.
3.  **Reviewer Recommendations (문서 성격별 추천 금붕어 조합)**:
    *   *상위 사업 기획서 / 제안서 / 캠페인 전략*: `design_critic` (`resources/document_standards/feature-spec-standard.md`), `structural` (`resources/document_standards/feature-spec-standard.md`), `comprehension`, `style` 추천.
    *   *상세 산출물 제작 명세 / 실행 계획 / 외주 지시서*: `implementation_readiness` (`resources/document_standards/implementation-readiness-standard.md`), `codebase_details`, `style` 추천.
    *   *완성된 마케팅 카피 / 퍼스널 브랜딩 원고 단건 점검*: `comprehension` (무맥락 3초 이해도), `style` (`resources/document_standards/world-of-content-standard.md` — 카피 설득력, Anti-Slop 및 지적 서명 반영 검수), `codebase_details` (팩트 정합성) 추천.
4.  **Confirm Configuration**: 검증 대상, 투입할 금붕어 목록, 적용할 표준을 보여주고 사용자 확인 후 시작합니다.

### Step 2: The Sharpening Loop (반복 벼리기 사이클)

다음 순서를 루프로 실행합니다:

#### 2.1 Unconditional Target Resolution

매 라운드 시작 전 반드시 `TargetFile`을 다시 읽어 `SemanticTarget`의 최신 시작/종료 라인 번호를 동적으로 재계산합니다(이전 라운드 수정으로 라인 번호가 밀렸을 수 있기 때문).

#### 2.2 Review Swarm Execution

1.  **Launch Subagent Swarm**:
    *   **Hierarchical Dispatch (Synthesizer)**: 중간 취합 에이전트(`agents/synthesizer.md` / `egm-synthesizer`)를 호출하여 금붕어들을 병렬 실행하고 중복 제거된 리포트를 생성합니다.
    *   **Direct Dispatch (Flat Swarm)**: 또는 요청된 리뷰어 서브에이전트들을 직접 병렬로 호출하여 각자 `agents/{name}.md` 페르소나와 `resources/document_standards/` 표준을 읽고 감사하게 합니다.
    *   각 리뷰어는 심각도(🔴 Blocker, 🟡 Major, 🟢 Nit), 라인 번호, 문제점 설명, 개선 제안을 반환합니다.
2.  **Consolidate Findings**: 모든 지적 사항을 하나의 `FindingsList`로 모으고 고유 ID(`<reviewer>-<round>-<n>`)를 부여합니다.
3.  **Failed Reviewers**: 실패하거나 응답이 없는 리뷰어는 1회 재시도 후에도 실패하면 반드시 **not run**으로 기록하며, 절대 결함 0건으로 간주하지 않습니다.

#### 2.3 Aggregated Resolution Hand-off

1.  `TargetFile`, 활성 라인 범위, `FindingsList`를 **`egm-resolve`** 워크플로우로 넘깁니다.
2.  코끼리 컨텍스트에서 `CONTEST` vs `PROPOSE FIX`를 분류하고 `goldfish_review_report.md`를 저장한 뒤, 사용자 결정을 받아 승인된 수정을 `TargetFile`에 적용합니다.

#### 2.4 Iteration Check & Turn Boundary (수렴 및 종료 판정)

*   **Settled findings (합의 완료 항목)**: 반박(`CONTEST`)되었거나 수정을 생략하기로 합의된(Accepted risk) 지적은 settled 목록에 기록하여 다음 라운드에서 다시 문제 삼지 않습니다.
*   **Fixed findings (수정 적용 항목)**: 수정이 적용된 위치에서 다음 라운드 금붕어가 또다시 동일한 문제를 지적한다면 수정이 불충분했던 것이므로 다시 오픈합니다.
*   **Clean round (클린 라운드)**: 미해결된 Blocker나 Major 지적이 단 한 건도 없고 모든 리뷰어가 정상 실행(`completed`)된 라운드입니다(Nit는 블록하지 않음).
*   **Converged (완전 수렴)**: **2회 연속 클린 라운드** 달성 시 수렴 완료로 판정합니다.
*   **Cap (최대 라운드 제한)**: 최대 5라운드.
*   **Churn (진동)**: 이전에 적용한 수정을 되돌려야만 해결되는 상충 지적이 발생한 경우.

수렴(Converged) 시 Step 3로 이동합니다. Cap 도달 또는 Churn 발생 시 남은 지적 사항을 요약해 보여주고 사용자에게 "현재 상태로 확정하고 진행할지, 추가 라운드를 돌릴지, 초안 작성 단계로 돌아갈지" 한 가지 질문으로 묻습니다.

**Crucial Turn-Taking Requirement**: 각 라운드의 `egm-resolve` 단계에서는 반드시 사용자의 턴 응답을 기다려야 하며, 사용자가 명시적으로 다음 라운드 진행을 승인했을 때만 Step 2.1로 루프를 돕니다.

### Step 3: Output Boundary & Final Checkpoint

완료 시:
*   **Programmatic Callers**: `egm-review`의 서브플로우로 호출된 경우 부모 워크플로우로 깔끔하게 복귀합니다.
*   **Manual Invocations**: 채팅에 완료 요약을 출력합니다: *"The review loop has completed after [N] rounds ([converged | accepted at cap | halted]). Open findings are in the report. Would you like to proceed to the next stage, or do you have any final questions?"*
