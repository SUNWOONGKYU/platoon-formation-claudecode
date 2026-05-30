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
3. **자율 작전 수행** — 분대장이 분대원을 투입, 필요 시 외부 AI(Codex/Gemini/Grok/Perplexity)를 용병으로 호출, 대규모 단일 의존 작업은 **DW 부대(특수부대)**가 헤드리스 세션으로 전담
4. **결과 종합 후 해산** — 보고 수집·검증·종합 보고서 작성 후 해산

지휘관(사용자)의 개입은 **최대 4회** — ⓪ 소대 추가 여부 ① 임무·편성 ② 작전 승인 ③ 해산 승인.

## 🏗️ 편제 (Formation)

```
편제 공식:  HQ(1) + N Squads × 13(Leader+12정규) + 4 Mercenaries + M DW 부대 (선택) + 예비병 N×3 별도

  지휘관 (사용자 / PO)  ← 유일한 인간
       │
  ┌────┴─────┐
  │ 소대 HQ  │  소대장 (Opus) — Orchestrator
  └────┬─────┘
       │
  ┌────┼────────────────┬───────────────────────┐
  │    │                │                       │
1분대 Alpha   2분대 Bravo   3분대 Charlie  ...     DW 부대 (특수부대)
                                                 DW-1, DW-2, ... (M개, 선택)
                                                 부대장 = Opus 4.8 이상 필수
                                                 헤드리스 claude -p 세션에서
                                                 Workflow 도구 직접 호출 (포병)
  └─ 분대장 (Sonnet 기본)                          (제병협동: 보병 + 포병)
  └─ 정규병 12 (Haiku/Sonnet)
  └─ 예비병 3 (정원 외)

  용병 풀 (공유 자산, 특수정찰):  Codex · Gemini · Grok · Perplexity
```

> **DW 부대(Dynamic Workflow Unit)** — 일반 분대(Alpha~Lima)가 보병이라면 DW 부대는 포병(대량 화력). NATO 호출부호 없는 특수부대로, 부대장이 별도 헤드리스 `claude -p` 세션에서 `Workflow` 도구를 직접 호출해 DW 엔진을 가동한다. 소대장 컨텍스트는 무손상으로 유지된다. **Claude Max·Team·Enterprise 플랜 + Opus 4.8 이상 환경에서만 가동 (Pro 플랜 미지원)**. 일반 분대 + DW 부대 + 용병 = 제병협동(諸兵協同) 지휘 체계.

전체 구조는 [`백호-platoon-formation/platoon_formation.svg`](./백호-platoon-formation/platoon_formation.svg)를 참조하세요.

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
