<!--
Copyright 2026 Google LLC.
SPDX-License-Identifier: Apache-2.0
-->

# egm-bootstrap README Templates (비즈니스 자산 및 맥락 지도 템플릿)

`egm-bootstrap` 스킬이 워크스페이스 폴더를 정리하며 생성하는 맥락 요약 문서(`${target_filename}`, 기본값 `README.md`)의 표준 템플릿입니다.

## 📄 Component Package (Peanuts / 개별 프로젝트·캠페인·자산 폴더) README Template

```markdown
# [폴더 / 프로젝트 / 자산 이름]

## Overview
[이 폴더에 담긴 비즈니스 프로젝트, 마케팅 캠페인, 또는 퍼스널 브랜딩 자산의 목적과 핵심 역할 요약]

## Interface
- [외부(고객/독자/타 부서)에 제공되는 핵심 산출물, 오퍼, 주요 파일 및 핵심 메시지 요약]
- [하위 폴더 목록 및 해당 요약 문서로의 상대 경로 링크 (예: `./subdir/README.md` 또는 `./subdir/${target_filename}`)]

## Invariants
- [이 프로젝트/브랜드에서 절대 흔들리면 안 되는 불변 원칙, 톤앤매너, 타겟 기준, 가격/정책 규칙]

## Side Effects
- [외부 발행 채널(SNS, 뉴스레터, 웹사이트), 고객 발송, 예산 집행 등 이 폴더의 산출물이 외부에 미치는 실제 영향]

## Verification
- Run: `<test-command>`
```

## 📄 Repository Root (Blueprint / Hay / 비즈니스 전체 맥락 지도) README Template

```markdown
# [비즈니스 / 브랜드 / 워크스페이스 이름] Blueprint

## System Overview
[이 비즈니스, 1인 기업, 또는 업무 워크스페이스 전체의 핵심 비전, 타겟 시장, 제공 가치에 대한 거시적 요약]

[루트 폴더에 직접적인 주요 문서나 파일이 있는 경우 선택적 포함]
## Local Interface & Invariants
- [루트 폴더 내 핵심 문서, 브랜드 원칙, 외부 발행 영향]

## Subdirectories & System Boundaries
- [하위 폴더 이름]: [하위 요약 문서 상대 경로 링크 (예: `./subdir/README.md` 또는 `./subdir/${target_filename}`) 또는 `./subdir/SKILL.md`] - [하위 폴더의 비즈니스 역할 및 핵심 자산 요약]

## Global Invariants
- [비즈니스 및 퍼스널 브랜딩 전체를 관통하는 최상위 원칙, 핵심 타겟 정의, 절대 금기 사항]

## Global Side Effects
- [운영 중인 전체 외부 채널, 고객 접점, 파트너십, 주요 플랫폼 연동 현황]

## System Verification
- Run: `<test-command>`
```

## 📄 Intermediate Blueprint README Template

```markdown
# [카테고리 / 상위 폴더 이름]

## Overview
[이 상위 폴더 아래에 묶여 있는 하위 프로젝트, 캠페인, 또는 지식 자산들의 공통 목적 요약]

## Subdirectories & Boundaries
- [하위 폴더 이름]: [하위 요약 문서 상대 경로 링크 (예: `./subdir/README.md` 또는 `./subdir/${target_filename}`) 또는 `./subdir/SKILL.md`] - [하위 폴더의 역할 요약]
```
