# 운영본 ↔ 공개 배포본 차이점 (Internal vs Public)

본 문서는 백호 platoon-formation 스킬의 **PO 사내 운영본** 과 **GitHub 공개 배포본** 간의 차이점을 명시한다.

## 운영 구조

| 구분 | 위치 | 용도 |
|---|---|---|
| **운영본 (Internal)** | `~/.claude/skills/백호-platoon-formation/skill.md` (PO PC) | PO 본인 작업 — SAL Grid Dev Suite + 18종 사신 시리즈 스킬이 모두 설치된 환경 가정 |
| **공개 배포본 (Public)** | `github.com/SUNWOONGKYU/platoon-formation-claudecode/백호-platoon-formation/SKILL.md` | 다른 사용자 — 외부 의존 없이 자기완결 동작 |
| **G드라이브 템플릿** | `G:\내 드라이브\SAL_Grid_Dev_Suite_Template\.claude\skills\백호-platoon-formation\skill.md` | 청룡 스킬이 신규 SAL Grid 프로젝트 부트스트랩할 때 사용 — 운영본과 동기화 |

## 공통 — 두 버전 모두에 적용된 자기완결성 개선

다음 항목은 운영본·공개본·G드라이브 템플릿 **세 곳 모두**에 동일하게 적용된다:

| 항목 | 변경 전 | 변경 후 |
|---|---|---|
| **절대 경로** | `C:/Users/home/Desktop/architecture.svg` (PO PC 종속) | `{프로젝트루트}/output/architecture.svg` |
| **외부 참조** | "용병 CLI 미설치 시 설치: **공식 문서 참조**" (모호) | Codex / Gemini / Grok / Perplexity 각 CLI 공식 URL + 환경변수명 + npm 명령어 명시 |

## 운영본과 공개본의 의도적 차이

다음 두 가지는 **공개본에서만** 자기완결적으로 상세화되었다. 운영본은 PO 환경(SAL Grid 사용 + 18종 스킬 설치 완비)을 가정하므로 간결한 형태로 유지된다.

### 1. Phase 6.3 — SAL Grid 반영 의무

**운영본 (간결)**: "코드 수정하면 SAL Grid 반영 필수. 예외 없음." — PO는 청룡 스킬을 알고 있으므로 추가 설명 불요.

**공개본 (자기완결)**: 다음을 본문에 통합 기술
- SAL Grid란 무엇인가? — Stage(S) × Area(A) × Level(L) 3차원 좌표계 설명
- 선행 조건 — `SAL_Grid_Dev_Suite/`·`grid_records/`·`TASK_PLAN.md` 존재 여부로 SAL Grid 프로젝트 식별
- `grid_records/{TaskID}.json` 구조 — 5개 필드(task_status / verification_status / generated_files / build_verification / modification_history) 명세 + 예제 JSON
- 글로벌 최소 완료 기준 3가지 — 파일 존재 + 빌드 통과 + 실제 호출/실행
- 청룡 스킬 보조 활용 안내 — 있으면 `/sal-grid-dev add/modify/status` 자동화, 없으면 수동 절차

> 결과: 공개본 사용자는 청룡 스킬이 없어도 **수동으로 grid_records JSON을 작성·갱신하여 SAL Grid 방법론을 적용**할 수 있다.

### 2. Phase 4 — 스킬 18종 핵심 역할 본문 기술

**운영본 (표만)**: 18종 스킬 표 — `/sal-grid-dev`, `/deploy-subagent`, `/review-evaluate`, … (PO는 모든 스킬을 알고 있으므로 표로 충분).

**공개본 (자기완결)**: 표 아래에 각 스킬의 핵심 역할 + 자기완결 대안 추가
- 18종 각각에 대해 — 이 스킬이 무엇을 자동화하는지 1-2줄 요약
- **"자기완결 대안"** — 스킬 미설치 시 분대장이 동일 결과를 얻기 위해 수행할 분대원 투입 절차 명시
- "부재 시 행동 원칙" 절 추가 — Skill 호출 시도 → 실패 시 자기완결 대안 자동 적용

> 결과: 공개본 사용자는 18종 스킬을 한 개도 추가 설치하지 않아도 **백호 단독으로 같은 임무를 수행**할 수 있다. 스킬이 설치돼 있으면 호출로 가속, 없으면 분대원으로 수동 수행.

### 3. Phase 2.5 DW 부대 — 청구항 표현 일반화

2026-05-30 추가된 **Phase 2.5 (DW 부대, Dynamic Workflow Unit)** 섹션에서:

**운영본 (간결)**: *"🎯 핵심 설계 목적 — 소대장 컨텍스트 경량화 **(백호 청구항 12)**"*

**공개본 (일반화)**: *"🎯 핵심 설계 목적 — 소대장 컨텍스트 경량화 **(계층 분산 설계)**"*

> 결과: 공개본은 청구항 번호·청구항 직접 의도를 노출하지 않는다. 운영본은 PO 본인용이므로 청구항 번호로 정확한 위치를 박는다.

또한 운영본의 실증 근거(Run ID `wf_01d2c4b1-5ae`, `subagent_tokens=42883` 같은 구체적 수치)는 공개본에서 "Run ID, agent_count, subagent_tokens 검증" 같이 일반화한다.

## 동기화 정책

1. **운영본 ↔ G드라이브 템플릿**: 즉시 동기화. 운영본 수정 시 G드라이브에도 같은 내용 복사. (G드라이브 템플릿은 청룡 신규 프로젝트 부트스트랩의 SSOT)
2. **운영본 → 공개본**: 자기완결성을 해치지 않는 범위에서만 반영. 새로운 PO 종속 사항(절대 경로, 사내 인프라)이 추가되면 공개본에서는 자기완결 형태로 일반화.
3. **공개본 자체 개선 (자기완결 강화)**: 공개본에만 적용, 운영본·G드라이브 템플릿에는 반영하지 않음 (운영본은 간결성 우선).

## 검증 체크리스트

공개본을 push하기 전에 다음을 확인한다:

- [ ] 절대 경로(`C:/Users/`, `D:/`, `/Users/`, `/home/`) 없음
- [ ] "공식 문서 참조" 같은 막연한 외부 참조 없음
- [ ] 다른 스킬에 대한 강제 호출 없음 (있으면 "자기완결 대안" 함께 명시)
- [ ] SAL Grid 등 외부 방법론 참조 시 핵심 개념·구조·절차가 본문에 포함됨
- [ ] PO 본인 환경에서만 작동하는 가정(특정 폴더 구조, 특정 도구 설치) 없음

---

*마지막 갱신: 2026-05-30*
