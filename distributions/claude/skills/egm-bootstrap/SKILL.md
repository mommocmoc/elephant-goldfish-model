---
name: egm-bootstrap
license: Apache-2.0
disable-model-invocation: true
description: >-
  비즈니스용 코끼리-금붕어 모델(EGM-Biz)의 워크스페이스 맥락 지도 자동 구축 스킬입니다.
  폴더 안에 흩어져 있는 기존 사업 문서, 마케팅 자료, 퍼스널 브랜딩 노트, 코드/데이터를 탐색하여
  코끼리 에이전트가 사용자의 비즈니스 맥락을 단번에 이해할 수 있도록 폴더별 요약 문서(README.md)를 생성합니다.
---

# EGM Peanuts and Hay Bootstrap (`egm-bootstrap` — 내 비즈니스 맥락 지도 구축)

When running in Codex or ChatGPT Work, first read [Codex integration](../../docs/codex.md).
In Claude Code or Cowork, read `${CLAUDE_PLUGIN_ROOT}/docs/claude.md` first.

## Overview

이 스킬은 일반 사무직 실무자나 1인 비즈니스 운영자의 워크스페이스 폴더 트리를 탐색하여, **코끼리(Elephant) 에이전트가 사용자의 비즈니스 배경·기존 자산·브랜드 원칙을 즉시 파악할 수 있도록 표준화된 `README.md` 맥락 지도**를 자동으로 생성·보정·검증합니다.

*   **Peanuts (말단 폴더 요약서)**: 개별 프로젝트, 캠페인, 상품, 또는 콘텐츠 폴더의 핵심 내용(제공 가치/산출물, 불변 원칙/브랜드 룰, 외부 발행 영향)을 담은 요약서입니다.
*   **Hay (상위 폴더 블루프린트)**: 하위 폴더들의 요약을 하나로 묶어 비즈니스 전체의 지형도를 한눈에 보여주는 상위 인덱스 문서입니다.

## Delegation

금붕어 스카우트(Goldfish explorer)는 이전 대화 기록이 없는 깨끗한 컨텍스트에서 실행됩니다. 하네스의 서브에이전트 도구(Antigravity: `invoke_subagent`; Claude Code: `Agent`; Codex: `spawn_agent`)를 사용하여 호출하십시오. 각 서브에이전트는 `agents/<file>.md`에 정의되어 있으며, 명명된 서브에이전트를 지원하지 않는 환경에서는 범용 서브에이전트를 생성하고 해당 페르소나 파일을 먼저 읽도록 지시하십시오. 서브에이전트 중첩이 불가능하면 직접 병렬 호출을 사용하고(`docs/harnesses.md` 참조), 서브에이전트 도구가 아예 없다면 스카우트 단계를 인라인으로 실행하십시오.

## Inputs

*   **`path`**: 맥락 지도를 구축할 대상 루트 폴더 경로 (필수).
*   **`is_worker`**: 내부 파라미터. 워커 서브 오케스트레이터로 실행 중이면 `true` (선택, 기본값: `false`).
*   **`sub_tree_root`**: 내부 파라미터. 할당된 서브트리 루트 경로 (선택).
*   **`target_filename`**: 생성하거나 업데이트할 요약 파일 이름 (선택, 기본값: `"README.md"`). 기존에 직접 관리 중인 `README.md`를 건드리고 싶지 않을 때 `"EGM.md"`나 `"CONTEXT.md"` 등으로 지정할 수 있습니다.

## Execution Constraints (CRITICAL)

1.  **No Code / Source Modification**: 오직 맥락 요약 문서(`README.md` 또는 `target_filename`)만 생성/수정하십시오. 원본 업무 문서, 원고, 데이터, 기능성 코드는 절대 건드리지 마십시오.
2.  **VCS Isolation**: `git commit`, `git add` 같은 버전 관리 커밋을 임의로 실행하지 마십시오.
3.  **Temporary File Isolation**: 각 워커는 상태, 체크포인트, 로그 파일을 워크스페이스 외부의 개별 디렉토리(호스트 스크래치 또는 `mktemp -d`)에 보관하며, never a fixed, shared path (절대 고정된 공유 경로를 사용하지 마십시오). If neither exists, skip them.
4.  **Tool Usage (CRITICAL)**: 셸, 파일 읽기/쓰기/수정 도구를 사용하여 디렉토리를 탐색하고 깔끔한 마크다운(표준 헤더, 표, 코드 블록)으로 요약 문서를 작성하십시오.
5.  **Subagent Delegation**: 서브 오케스트레이터 워커들을 병렬로 실행하십시오.
6.  **Mandatory Safety Gate (CRITICAL)**: 하위 디렉토리가 **15개**를 초과하면, 전체 실행이나 대량 서브에이전트 생성 전에 반드시 사용자에게 승인 게이트를 제시하고 응답을 기다려야 합니다.

---

## Workflow

### Step 0: Ecosystem & Verification Discovery

1.  루트 폴더에 검증이나 빌드 설정 파일이 있는지 확인합니다:
    *   Node.js / TypeScript: `package.json` -> `npm test`
    *   Rust: `Cargo.toml` -> `cargo test`
    *   Go: `go.mod` -> `go test ./...`
    *   Python: `pyproject.toml` / `pytest.ini` / `setup.py` -> `pytest`
    *   C/C++ / Make: `Makefile` -> `make test`
    *   Bazel: `BUILD` / `BUILD.bazel` -> `bazel test //...`
    *   일반 비즈니스 문서/노트 폴더(위 설정 없음): `<test-command>` (또는 수동/드리프트 검수 기준)
2.  확인된 값을 `VerificationCommand`로 기록합니다.

---

### Step 1: Startup Branching Control

1.  `is_worker` 파라미터를 확인합니다.
2.  `is_worker`가 `true`이면 즉시 **Step 4: Sub-Orchestrator Worker Flow**로 이동하여 할당된 `sub_tree_root`를 처리합니다.
3.  `is_worker`가 `false`이거나 생략되었으면 루트 오케스트레이터로서 **Step 2: Incremental Scoping & Safety Gate**를 진행합니다.

---

### Step 2: Incremental Scoping & Safety Gate (Root Orchestrator)

1.  **Incremental BFS Discovery**: 대상 `path` 트리를 한 레벨씩 너비 우선 탐색(BFS)으로 파악합니다 (`find`나 `ls -R` 같은 전체 재귀 명령은 사용하지 마십시오).
    *   심볼릭 링크 및 숨김/시스템 폴더(`.git`, `.antigravity`, `node_modules`, `venv`, `dist`, `build`, `third_party`)는 무시합니다.
    *   `.gitignore` 및 `.egmignore` 패턴을 준수합니다.

2.  **Safety Gate & Unbounded Spawning Approval (CRITICAL)**: 탐색 중 발견된 디렉토리 수가 **15개**를 초과하면 즉시 멈추고 사용자 승인을 요청합니다:
    *   `question`: "The target directory tree contains more than 15 subdirectories. This is a large execution scope. Do you want to proceed with full-tree bootstrapping?"
    *   `options`: `["Yes, proceed with full tree bootstrapping", "No, abort execution"]`
    *   사용자의 응답을 받기 전에는 절대 진행하거나 워커를 생성하지 마십시오.

3.  **Bottom-up Post-Pruning Pass**: 매핑이 완료되면 말단에서 루트 방향으로 올라오며 빈 폴더나 단순 테스트 전용 폴더를 걸러냅니다:
    *   *Recognized Business & Source Files (인식 대상 업무 문서 및 소스 파일)*:
        - 비즈니스 기획·원고·노트 문서: `.md`, `.txt`, `.html` (단, 자동 생성할 `target_filename` 자체만 있는 경우는 제외하고 실제 업무 문서, 제안서, 카피 원고, 리서치 노트가 담긴 폴더를 포함)
        - 구조화 데이터 및 설정: `.json`, `.yaml`, `.yml`, `.sql`, `.graphql`, `.prisma`
        - 스크립트 및 코드 파일: `.py`, `.ts`, `.tsx`, `.js`, `.jsx`, `.sh`, `.go`, `.rs`, `.java`, `.kt`, `.c`, `.cc`, `.cpp`, `.h` 등
    *   *Test Directory Exclusion*: 오직 테스트 파일(`tests`, `testing`, `testdata`, `*_test.*`, `*.spec.*`)만 담긴 폴더는 제외합니다.
    *   *Empty Filtering*: 대상 파일도 없고 유효한 하위 폴더도 없는 빈 폴더는 제외합니다.

4.  유효 디렉토리 총개수와 최대 깊이를 계산한 뒤 **Step 3**로 이동합니다.

---

### Step 3: Execution Strategy Selection (Root Orchestrator)

1.  **Direct Execution Strategy (Directory Count <= 5)**:
    *   대상 디렉토리가 5개 이하이면 현재 스레드에서 직접 **Step 4** 및 **Step 5**를 순차 수행합니다.

2.  **Unbounded Parallel Subagent Swarm Strategy (Directory Count > 5)**:
    *   (디렉토리가 15개를 초과한 경우 반드시 사용자 승인을 먼저 확인하십시오.)
    *   트리를 3~5개의 서브트리 파티션으로 나누어 각각의 파티션 루트에 대해 병렬 워커 서브에이전트(`egm-bootstrap-orchestrator`, `agents/bootstrap_orchestrator.md`)를 실행합니다:
        *   `Prompt`: "Run egm-bootstrap with path=`[path]`, is_worker=true, sub_tree_root=`[partition_root]`, target_filename=`[target_filename]`."
    *   모든 워커가 JSON `subtree_index` 페이로드를 반환할 때까지 대기한 후 **Step 5**로 이동합니다.

---

### Step 4: Sub-Orchestrator Worker Flow

1.  **Scout Registration**: 읽기 전용 `goldfish-explorer` 스카우트를 사용합니다. 런타임 서브에이전트 정의를 지원한다면 다음과 같이 등록합니다:
    *   `name`: `goldfish-explorer`
    *   `description`: `Read-only workspace and codebase explorer scout.`
    *   `system_prompt`: >- You are a read-only workspace and codebase explorer. Your task is to analyze the business documents, content assets, or source files in the target directory and return a concise, factual summary. Do not write or modify any files. Return a JSON object containing the directory's key deliverables/public symbols (interface), core business/brand rules or invariants (invariants), and external channels/side effects (side_effects) matching schema: {"interface": [], "invariants": [], "side_effects": []}. Never guess facts from file or directory names alone; cite the exact `[file:section_or_symbol]` for every entry, and output `["None"]` if no explicit invariants or side effects exist.
    *   `enable_write_tools`: `false`
    *   `enable_subagent_tools`: `false`

2.  **Post-Order Traversal**: 할당된 `sub_tree_root`를 가장 깊은 말단 폴더부터 역순(Post-order)으로 순회합니다. 각 디렉토리 `D`에 대해:
    *   **Correction & Modification Policy**: `D`에 이미 `target_filename`(기본값 `README.md`)이 존재한다면 **기존 내용을 통째로 덮어쓰지 마십시오**. 기존 내용을 읽고 작성자의 메모와 설명을 보존하면서, `goldfish-explorer` 스카우트가 추출한 실제 파일 내용과 병합하여 EGM 표준 구조(`Interface`, `Invariants`, `Side Effects`)에 맞게 보정·업데이트하십시오.
    *   **Scout Spawning**: `D` 안의 파일들을 약 1,500라인 단위 청크로 나누어 `goldfish-explorer`를 실행하고 결과를 병합합니다.
    *   **Parent Context Propagation**: 상위 폴더 `P`를 처리할 때는 직계 하위 폴더들의 요약 정보를 프롬프트 컨텍스트로 전달합니다.
    *   **Directory Classification & Template Generation**:
        *   **Component Package (Peanut)**: `D`에 직접적인 업무 문서나 소스 파일이 있는 경우 **Component Package README Template**을 사용합니다.
        *   **Repository Blueprint (Hay)**: `D`에 직접 파일은 없고 하위 폴더들만 있는 경우 **Intermediate Blueprint README Template**을 사용합니다.
        *   **Link Formatting Policy (CRITICAL)**: 하위 폴더 및 형제 파일 링크는 반드시 **실제로 존재하는 파일**에 대한 유효한 상대 경로(예: `./child/${target_filename}` 또는 `./child/SKILL.md`)만 사용하십시오. 존재하지 않는 파일에 대한 추측성 링크를 절대 만들지 마십시오.

3.  **Worker Reporting**: 순회가 끝나면 모든 처리된 디렉토리의 `path`, `summary`, `interface`, `invariants`, `side_effects`를 담은 `subtree_index` 배열을 구성합니다.
4.  `is_worker`가 `true`이면 [Worker Return Interface Schema](references/worker-return.schema.json) 형식에 맞춰 JSON을 반환하고 종료합니다. `false`이면 **Step 5**로 진행합니다.

---

### Step 5: Root Compilation & Termination (Root Orchestrator)

1.  **Receive Worker Payloads**: 병렬 워커들의 `subtree_index` 결과를 수집합니다.
2.  **Intermediate Directory Documentation**: 루트와 파티션 사이의 중간 폴더들을 바텀업으로 문서화합니다.
3.  **Root Directory Documentation**: 최상위 `path`에 대해 **Repository Root Blueprint Template**을 사용하여 전체 비즈니스/워크스페이스 맥락 지도를 완성합니다.
4.  **Final Correctness Verification (CRITICAL)**: 생성되거나 수정된 **모든 요약 파일(`README.md`)을 빠짐없이 직접 다시 읽고** 다음을 최종 검증하십시오:
    *   **EGM Compliance & Fact Grounding**: `Interface`, `Invariants`, `Side Effects` 섹션이 존재하며 실제 파일 내용에 근거하고 있는지 확인.
    *   **Markdown Syntax**: 깔끔한 헤더, 표, 코드 블록 문법 확인.
    *   **Valid & Resolvable Links (CRITICAL)**: 모든 상대 경로 링크의 대상 파일이 실제로 디스크에 존재하는지 확인하고 깨진 링크는 즉시 수정/제거.
5.  **Optional Drift Review (CRITICAL)**: 검증 완료 후 사용자에게 생성된 맥락 지도에 대해 드리프트 검사를 실행할지 묻습니다:
    *   `question`: "Do you want to run the EGM Drift Report on the generated contract updates to check for consistency with the codebase?"
    *   `options`: `["(Recommended) Yes, run drift report on modified contracts", "No, skip drift review"]`
    *   "Yes" 선택 시 `egm-drift-report`를 실행하여 요약 문서와 실제 파일 간의 정합성을 최종 점검합니다.
6.  성공적으로 실행을 종료합니다.

---

## Templates

요약 파일을 작성하기 전에 반드시 이 스킬 폴더 기준 [README templates](references/templates.md)를 읽고 **Component Package (Peanuts)**, **Repository Root (Blueprint / Hay)**, **Intermediate Blueprint** 템플릿 구조를 준수하십시오.
