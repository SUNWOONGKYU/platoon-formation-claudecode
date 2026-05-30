# platoon-formation-claudecode

[![Patent Pending](https://img.shields.io/badge/Patent%20Pending-KIPO%2010--2026--0041235-orange)](./PATENT.md)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](./LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-purple)](https://docs.claude.com/en/docs/claude-code/overview)

> **백호 (白虎 · White Tiger)** — Claude Code용 **소대 편제(Platoon Formation)** 방식 다중 AI 에이전트 팀 편성·운용 스킬
>
> 사신(四神) 시리즈 (청룡·**백호**·주작·현무) 중 하나

군 소대 편제 구조(소대장 → 분대장 → 분대원)를 다중 AI 에이전트 오케스트레이션에 응용하여, **이종(異種) AI를 통합 운용**하면서 **통신 복잡도를 O(n²) → O(n)으로 저감**한다.

## ⚠️ 특허 공지 (Patent Notice)

본 저작물은 **대한민국 특허청(KIPO)에 출원된 특허의 대상이 되는 발명**을 구현합니다.

- **출원번호 (Application No.)**: `10-2026-0041235`
- **출원일 (Filing Date)**: 2026-03-07
- **출원인·발명자 (Applicant·Inventor)**: 선웅규 (Sun Woongkyu)
- **발명의 명칭**: 군 소대 편제를 응용한 계층적 다중 AI 에이전트 작업팀 편성·운용 시스템 및 방법
- **English Title**: System and Method for Hierarchical Multi-AI Agent Task Team Formation and Operation Applying Military Platoon Organization

본 저작물은 **Apache License 2.0** 하에 배포되며, Apache 2.0 제3조의 **명시적 특허 사용권 부여**에 따라 사용자는 이 저작물을 라이선스 조건 하에서 무상으로 사용할 수 있습니다. 상세 내용은 [PATENT.md](./PATENT.md)와 [NOTICE](./NOTICE)를 참조하세요.

![출원번호 통지서](백호-platoon-formation/특허출원번호통지서.png)

---

## 🎯 무엇을 하는 스킬인가?

`/platoon-formation`을 호출하면, 현재 프로젝트에 대해:

1. **소대 편성** — 소대장(Opus 1명) + 3분대 (분대장 + 정규병 12 + 예비병 3) × N + 용병 4명
2. **임무 분해 & 통합 배정** — 소대장이 작업을 분해하고, 각 분대장에게 임무 + AI 모델 등급 + 도구 권한을 한 번에 지시
3. **자율 작전 수행** — 분대장이 분대원을 투입, 필요 시 외부 AI(Codex/Gemini/Grok/Perplexity)를 용병으로 호출
4. **결과 종합 후 해산** — 보고 수집·검증·종합 보고서 작성 후 해산

지휘관(사용자)의 개입은 **단 3회** — ① 임무 부여 ② 작전 승인 ③ 해산 승인.

## 🏗️ 편제 (Formation)

```
편제 공식:  HQ(1) + N Squads × 13(Leader+12정규) + 4 Mercenaries (N≥3) + 예비병 N×3 별도

  지휘관 (사용자 / PO)  ← 유일한 인간
       │
  ┌────┴─────┐
  │ 소대 HQ  │  소대장 (Opus) — Orchestrator
  └────┬─────┘
       │
  ┌────┼────────────────┐
  │    │                │
1분대 Alpha   2분대 Bravo   3분대 Charlie  ...  (B형 5분대 / C형 N분대 확장 가능)
  │    │                │
  └─ 분대장 (Sonnet 기본)
  └─ 정규병 12 (Haiku/Sonnet)
  └─ 예비병 3 (정원 외)

  용병 풀 (공유 자산):  Codex · Gemini · Grok · Perplexity
```

전체 구조는 [`백호-platoon-formation/platoon_formation.svg`](./백호-platoon-formation/platoon_formation.svg)를 참조하세요.

## 💰 구조적 효율성 (이론적)

소대 편제 방식은 종래의 평면적(flat) 에이전트 구조 대비 **통신 복잡도** 측면에서 이론적 우위를 가질 수 있습니다.

### 통신 경로 수 비교 (수학적 도출)

평면 구조의 통신 경로 수 `n(n-1)/2` (O(n²)) 대비, 소대 편제 구조는 `O(n)`으로 도출됩니다:

| 규모 | 평면 구조 통신 경로 | 소대 편제 통신 경로 | 이론적 감소율 |
|---|---:|---:|---:|
| 42명 (1소대) | 861 | 45 | 약 94.8% |
| 420명 (10소대) | 87,990 | 423 | 약 99.5% |
| 4,200명 (100소대) | 8,817,900 | 4,203 | 약 99.95% |

> ⚠️ **본 수치는 통신 경로 수의 수학적 도출치(이론치)입니다.** 실제 토큰 사용량·API 비용·실행 시간은 워크로드 특성·LLM 응답 길이·도구 호출 빈도·재시도 횟수 등에 따라 달라지므로, 본 수치를 그대로 "비용 절감률"로 해석할 수 없습니다. 실제 효과는 사용 환경에서 별도 측정·벤치마크가 필요합니다.

### 차등 모델 배정

소대장은 Opus, 분대장은 Sonnet, 분대원은 작업 복잡도에 따라 Haiku/Sonnet — 모든 에이전트에 최상위 모델을 쓰지 않고 임무 성격에 맞는 모델을 배정하는 구조. 실제 비용 영향은 워크로드별 측정이 필요합니다.

## ⚡ 설치

### 가장 빠른 방법 — Claude Code에게 시키기

> **"`SUNWOONGKYU/platoon-formation-claudecode` 에서 백호 소대 편제 스킬을 받아서 내 `~/.claude/skills/` 폴더에 설치해 줘."**

또는 더 짧게:

> **"`platoon-formation-claudecode` 설치해 줘."**

### 직접 설치 (git clone)

**Windows (PowerShell·Git Bash·WSL 공통)**
```bash
git clone https://github.com/SUNWOONGKYU/platoon-formation-claudecode.git
cp -r platoon-formation-claudecode/백호-platoon-formation "$env:USERPROFILE\.claude\skills\"
```

**macOS·Linux**
```bash
git clone https://github.com/SUNWOONGKYU/platoon-formation-claudecode.git
cp -r platoon-formation-claudecode/백호-platoon-formation ~/.claude/skills/
```

### 호출

새 Claude Code 세션에서 다음 중 아무거나 입력하면 됩니다:

- `/platoon-formation`
- `/백호-platoon-formation`
- "백호 소대 편제로 팀 짜줘"
- "platoon-formation으로 팀 편성"

## 🐉 사신(四神) 시리즈

**백호 (白虎 · Platoon Formation)**는 사신 메타 스킬 시리즈의 일원입니다:

| 사신 | 스킬 | 역할 |
|---|---|---|
| 🐲 청룡 | `sal-grid-dev` | SAL Grid 개발방법론 (Stage-Area-Level 좌표계) |
| 🐯 **백호** | **`platoon-formation`** | **소대 편제 방식 팀구성 (본 스킬)** |
| 🦅 주작 | `sal-da` | SAL-DA 다차원 진단·감사 방법론 |
| 🐢 현무 | `buzzlab-simulation` | BuzzLab 시뮬레이션 (AI 토론 기반 예측) |

## 📜 라이선스

[Apache License 2.0](./LICENSE) — 명시적 특허 사용권 부여(Section 3) 포함.

본 저작물에 포함된 발명에 대한 특허는 출원 중이며(`10-2026-0041235`), Apache 2.0 라이선스에 따라 사용자는 무상 특허 사용권을 부여받습니다. 상세 내용: [PATENT.md](./PATENT.md), [NOTICE](./NOTICE).

## 📂 디렉토리 구조

```
platoon-formation-claudecode/
├── LICENSE                              # Apache 2.0
├── NOTICE                               # 특허 공지 (Apache 2.0 Section 4(d))
├── PATENT.md                            # 특허 출원 정보 (최소 공지)
├── README.md                            # 본 파일
└── 백호-platoon-formation/
    ├── SKILL.md                         # 스킬 본체 (Claude Code 진입점)
    ├── platoon_formation.svg            # 편제 구조도
    └── 특허출원번호통지서.png            # KIPO 출원번호 통지서 (증빙)
```

## 🤝 기여 (Contributing)

Issue·PR을 환영합니다. 특허 출원 중인 핵심 발명 청구항(Claims)을 우회·축소하지 않는 범위에서의 개선·확장에 한합니다.

## ✉️ 문의

- 출원인·발명자: 선웅규 (Sun Woongkyu)
- 이메일: wksun999@gmail.com
- GitHub: [@SUNWOONGKYU](https://github.com/SUNWOONGKYU)
