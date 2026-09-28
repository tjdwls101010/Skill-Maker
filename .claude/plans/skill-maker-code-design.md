# skill-maker: 스킬 코드의 설계 판단을 더하는 계획 (codebase-design 반영)

> 계획 세션(2026-09-28)에서 작성했다. 계획 모드에서는 지정 경로만 쓸 수 있어, 승인 직후 `.claude/plans/skill-maker-code-design.md`로 이름을 바꾼다(이전 두 계획과 같은 방식).

## Context

v0.2.0에서 skill-maker는 스킬 디렉터리 트리를 `--help`처럼 먼저 읽히는 인터페이스로 보고, 모든 스킬의 코드 모양을 하나로 맞췄다(C1~C13). 성진은 이번에 두 가지를 더하고 싶어 한다.

- Matt Pocock의 `codebase-design`(깊은 모듈·인터페이스·seam)을 반영해, skill-maker가 만드는 스킬의 코드가 모양뿐 아니라 **내부 설계까지** 좋게 나오게 한다.
- `--help`에도 progressive disclosure를 적용한다.

성진은 코드 설계 전문가가 아니라서 Claude가 쓴 스킬 코드의 설계 결함을 직접 잡기 어렵다. 그래서 그 판단이 skill-maker에 들어 있어야 한다.

**한 문장 목표:** skill-maker SKILL.md의 `## Code the skill bundles`를 제자리에서 고쳐, 판단 문장을 더한다. 더하는 판단은 세 갈래다.
- Pocock 지식 중 스킬에 특화된 판단: 조회를 감싸지 않기, 단위는 입구 뒤의 깊은 모듈, 외부 시스템 폴더가 그 시스템의 형식을 빠짐없이 숨기기
- `--help`의 progressive disclosure
- v0.2.0 관례로 이행된 세 스킬이 드러낸 설계 빈틈

합의된 전제:
- **skill-maker는 좋은 스킬을 만드는 판단에 집중한다(성진).** tdd·codex 같은 도구를 쓸지는 성진이 그때그때 정한다. 그래서 표준 구조 테스트 파일 같은 도구는 넣지 않는다. skill-maker는 코드 없이 SKILL.md 하나로 남는다.
- **통째 이식은 하지 않는다.** codebase-design의 용어집·도식·테스트 용이성 일반론은 모델이 이미 안다.
  - 실측의 직접 증거가 있다. Pocock 원문을 붙이자 모델이 "깊은 모듈 = 전부 숨긴다"로 읽고 DB 직접 조회까지 금지했다.
- **이전 스킬(News·codex·k-politics)은 본보기가 아니라 구버전이 낳은 실패 보고다.**
- **SKILL.md 전체 재작성은 하지 않는다.** 1~3절과 6절은 그대로 두고, 4절에는 한 문장, 5절에는 제자리 수정만 한다.
  - v0.2.0의 문장과 이유를 모두 살린다. 문장마다 무엇이 되는지는 아래 보존 대조표로 확인한다.
  - 통째 교체한 첫 초안에서 이유 다섯 가지가 빠진 것이 이 원칙의 근거다.

## 증거 요약

### 사실
- skill-maker는 SKILL.md 한 파일(101줄)이고 코드도 references도 없다. `~/.claude/skills/skill-maker`는 이 레포로 가는 심링크다.
- `.tmp/codebase-design.md`는 upstream `mattpocock/skills` `skills/engineering/codebase-design/SKILL.md`와 동일하다(MIT). upstream에서는 과정 없는 참조 스킬로, tdd와 improve-codebase-architecture가 호출한다.
- 이행된 세 스킬(9/26~27)이 드러낸 빈틈. 구버전의 실패 보고로만 읽는다.
  - **단위 경계를 넘는 내부 import가 흔하다.**
    - codex에서는 여러 기능이 `codex.registry.runs`에서 10개 넘는 이름을 가져다 쓴다.
    - News는 `news.naver.http`를 직접 import한다.
    - k-politics의 실제 입구는 비공개 `c._json(...)`이다.
  - **트리에 칸이 없다.**
    - 데이터 위치가 갈렸다: News는 스킬 루트에 NEWS.db·.cache·.env를 흩었고, k-politics는 `DBs/`를 만들었다.
    - 운영 코드: k-politics `pipeline/` 10,501줄이 들어갈 자리가 없었다.
    - `references/`: News에 있다.
  - **기능끼리의 import가 두 가지로 읽혔다.** codex는 `batch→runs` 예외를 두었고, Agentic-SNS는 "기능끼리 금지"로 읽었다.
  - **k-politics의 설계 문제.**
    - `lawgo/`에 바뀌는 이유 셋이 섞였다: API 어댑터, 파이프라인 전용 수집 엔진, DB 저장 변환.
    - 법제처 형식 지식이 기능 파일로 샌다.
    - `lookup`(요청)과 `render`(응답)가 단계로 나뉘어, 명령 하나를 바꿀 때 세 파일을 고친다.
    - `cli.py:140-171`에 예외 흐름으로 된 폴백이 있다.
  - **성진이 k-politics에서 한 교정은 모두 "더 만들지 말라"였다.** DB 조회 래퍼를 두 번 기각했다: "괜히 코드를 만들면 오히려 클로드가 데이터를 필터링하고 서칭하는데 제약이 생겨."
- 이전 결정은 유지한다.
  - C9(성진): 테스트 방법은 tdd의 몫이다.
  - C4(성진): 코드 관례는 SKILL.md 안에 둔다.
  - C7: 스킬 레포에 구조 테스트를 둔다.
  - D17: 다른 모델 계열의 검토를 완료 조건으로 둔다.

### 실측 1: 기준선 대조 (계획 세션, 격리 헤드리스)

- **실행 방식:** `claude -p --safe-mode --restricted --permission-mode acceptEdits --model claude-opus-5-5 --tools "Bash,Read,Write,Edit,Glob,Grep" --allowedTools "Bash(uv *)" …`
- **설계:** 과제 둘 × 조건 셋, 모두 6회.
  - 과제: 읽기 목록 스킬, ECOS 조회 스킬
  - A: 스킬 없음
  - B: 현 skill-maker를 스킬 로드 형식으로 주입
  - P: B + codebase-design
- **결과:** 6회 모두 4~9분에 끝났다.
- **재현 자료:** 부록 A.

| 관찰 축 | A 스킬 없음 | B 현 skill-maker | P + codebase-design |
|---|---|---|---|
| 저장소 읽기를 감싸는 명령 | reading `list`, ecos 캐시 목록 | reading `search`, ecos `cached` | reading `list`; SKILL.md가 "데이터베이스를 직접 열거나 SQL을 쓰지 않는다"(이유는 쓰기의 정규화·중복 병합) |
| 래퍼의 비용 | — | reading-B SKILL.md가 한 문단을 들여 우회법을 가르침: "run one search per plausible spelling, merge by `id`, then drop hits that match only by accident" | — |
| 단위 입구 | 해당 없음(단일 파일) | reading: `__init__` 전부 0줄, cli·기능·테스트가 내부 파일(`reading_list.db.store`·`web.title`)로 직접. ecos: cli가 `ecos_api.client.DEFAULT_BASE`, 테스트가 `client as client_mod` | 제품 코드는 입구로 일관. 테스트 하나가 내부 경로를 문자열 패치 |
| 명령별 `--help`의 실패(종료코드) | 없음 | B·P 자식 help 24개 전부 없음(최상위 epilog에만) | 같음 |
| 최상위 help 지도 | 이미 있음 | 있음 | 있음 |
| 데이터 위치 | `~/.claude/…`, `~/.cache/…` | XDG | XDG |
| 과잉 추상(단일 구현 Protocol·ABC) | 0 | 0 | 0 |
| 외부 시스템 형식의 누출 | ecos: 기능 코드가 `StatisticTableList`·`SRCH_YN` 등 원시 필드를 앎 | ecos: 대체로 번역하지만 catalog가 그룹 코드 접미사로 슬롯을 계산(누출) | ecos: B보다 나쁨, 클라이언트가 원시 행을 내주고 catalog가 서비스명·필드명을 앎 |
| 나누는 기준 | 단일 파일에 여러 이유 | 기능·외부 시스템 단위로 잘 나눔(단계 분할 없음) | 같음 |
| `cli.py`의 도메인 로직 | — | reading-B: 본문에 "no domain logic"이 있는데도 cli가 처리할 항목 고르기·결과 조립·내보내기 쓰기를 함(`cli.py:154-174`). ecos-B는 대체로 얇음 | reading-P: 같은 위반(`cli.py:147-167`) |
| 명령으로 만든 작업 | 프로토콜·형식·쓰기는 모두 명령 | 같음 | 같음 |

- **codex 독립 감사.** gpt-6-astra medium, read-only. run `20260928-225911-baseline-audit-8971`, thread `01a0e84f-ff0c-7763-966a-67024649bcfa`.
  - 조회 래핑은 partly: reading 셋과 ecos-A·B는 맞다. 보편적 기본값이 아니라 "시험할 지침"으로 다룬다.
  - 입구는 partly: B 위반은 확인됐고, P의 제품 코드는 일관된다.
  - 과잉 추상 없음과 데이터 위치는 yes.
  - help는 partly: 최상위 지도는 기본값이다. 명령별 자기완결이 빈틈이다.
  - 놓친 결함 가운데 손잡이 덮어쓰기(ecos-B `series.py:183-192`)는 skill-maker의 "비싼 원문은 손잡이로" 원칙의 실패라 반영한다. 나머지 셋(비밀값 인코딩, 캐시 갱신 순서, 커밋 전 성공 출력)은 일반 버그라 넣지 않는다.
- **결론.**
  - **조회 래핑:** 모델에 없는 판단이다. Pocock 원문은 오히려 반대로 민다. 그래서 읽기와 쓰기를 가른다.
  - **단위 입구:** 현 본문으로는 들쭉날쭉하다. 깊은 모듈 어휘가 붙으면 좋아진다.
  - **명령별 help의 실패:** 조건에 관계없이 비어 있다.
  - **데이터 위치:** 모델의 기본값은 XDG다. 그래서 트리에 적어야 바뀐다.
  - **`cli.py`의 결정 로직:** "no domain logic"이 있어도 처리할 항목 고르기는 cli에 남는다. 예를 들어야 바뀐다.
  - **외부 시스템 형식의 누출:** 조건에 관계없이 기능 코드로 샌다. Pocock 조건이 가장 나쁘다.
  - **과잉 추상, 단계 분할:** 교정할 기본값이 아니다.

## 결정 장부

| # | 결정 | 소유자 | 근거·영향 |
|---|---|---|---|
| M1 | Pocock 지식 중 스킬에 특화된 판단만 옮긴다. 통째 이식은 하지 않는다 | 성진 | 일반론은 모델이 이미 앎. P 조건의 과잉 은닉 |
| M2 | 세 스킬이 드러낸 설계 빈틈도 판단 문장으로 넣는다 | 성진 | 반영 자리가 같은 절 |
| M3 | 이전 스킬은 실패 보고로만 쓴다. 관례 이전의 모양과 성진의 직접 결정은 걸러낸다 | 성진 | — |
| M4 | 기준선 실측(계획 세션) + 구현 후 같은 과제 재측. 원샷이라 실사용 품질의 증명은 아니다 | 성진 | C12(사후 재실행 없음)를 이번엔 뒤집음 |
| M5 | 표준 구조 테스트 파일은 넣지 않는다. skill-maker는 좋은 스킬을 만드는 판단에 집중하고, tdd·codex 사용은 성진이 그때그때 정한다. skill-maker는 코드 없이 유지하고, 이번 변경에 TDD 대상은 없다 | 성진 | 세 레포의 구조 테스트 재구현·편차는 알려진 한계로 남는다 |
| M6 | `--help`: 최상위는 지도, 명령별 help는 옵션·출력·실패를 스스로 다 말한다. argparse는 부모 epilog를 최상위에만 보여 준다. 지도가 한 화면을 넘으면 세 번째 단계가 아니라 명령 수를 줄인다 | 성진 제안 → 실측으로 좁힘 | B·P 자식 help 24개 모두 종료코드 없음 |
| M7 | 구현 후 GitHub 릴리스 `v0.3.0`. 형식은 v0.2.0을 따른다 | 성진 | — |
| M8 | tdd는 흡수하지 않고 본문에 포인터도 두지 않는다 | 성진 | M5와 같은 원칙 |
| M9 | 단위(패키지 바로 아래 모듈 또는 하위 패키지)는 입구(그 모듈 또는 `__init__.py`) 뒤의 깊은 모듈이다. 단위 밖은 입구를 건너뛰지 않는다. 레포의 구조 테스트 검사 목록에 "단위의 입구"를 더한다(C7 유지) | 성진 | Pocock 규칙 1·3, 증거는 위 |
| M10 | 런타임 데이터는 스킬 폴더의 `data/`에 둔다. 경계: 플러그인 배포 스킬은 업데이트를 넘어 남아야 할 상태를 플러그인 데이터 폴더에 둔다. 변수명은 본문에 쓰지 않는다(로드 시 치환 위험) | 성진. 경계는 사실(changelog: "`${CLAUDE_PLUGIN_DATA}` … persistent state that survives plugin updates") | 모델 기본값이 XDG |
| M11 | 실측 과제 둘: 읽기 목록(SQLite·Pocket CSV·제목 수집), ECOS 조회+로컬 캐시 | 성진 | 부록 A |
| M12 | 명령은 두 경우에만 둔다: 매번 같게 가거나 모델이 믿을 만하게 못 하는 일(프로토콜·파일 형식·페이지 넘김·함께 지켜야 할 규칙이 딸린 쓰기)을 숨길 때. 모델이 직접 쓸 수 있는 조회는 감싸지 않는다. 읽을 수 있는 스키마를 주고 모델이 쿼리를 쓴다 | 읽기: 성진(k-politics D6). 쓰기: Claude 판단(성진은 판단 없음). 실측 증거: B의 우회 문단, P의 과잉 은닉 | 읽기 경계는 4절, 명령 기준은 5절 — 한 뜻은 한 곳에 |
| M13 | 기능끼리의 import는 구조 테스트에 적은 간선만 허용한다(예: 실행 묶음이 실행 하나를 N번 쓰는 경우) | 성진 | 같은 문장의 두 해석 |
| M14 | 레포의 `tests/`에는 테스트·픽스처·시뮬레이터를 둔다. 유지보수자·예약 작업의 코드는 레포 루트의 하는 일 이름 폴더에 둔다. 그 코드는 스킬 단위의 입구를 import할 수 있지만, 스킬은 그 코드를 import하지 않는다 | 성진 | k-politics `pipeline/` |
| M15 | system 단위는 그 시스템과 말하는 법을 빠짐없이 담는다. 그걸로 하는 일(수집 엔진·저장 변환)은 목적 쪽에 둔다 | 성진 | `lawgo/` 혼합과 누출 |
| M16 | "처리 단계(요청/응답)가 아니라 바뀌는 이유로 나눈다"는 넣지 않는다 | 성진 (분량 검토에서 뺌) | 새 실측의 B·P는 기능·외부 시스템으로 잘 나눴다. 단계 분할은 기존 코드 이행(k-politics)에서만 나왔다 |
| M17 | 코드 절은 SKILL.md 안에 둔다(C4 유지). references 파일은 없다 | 성진 | 대부분의 스킬이 코드를 가짐 |
| M18 | stdout의 손잡이는 그 결과만을 가리킨다(다음 호출이 덮어쓰지 않음) | Claude 판단 (codex 감사 근거) | ecos-B 손잡이 덮어쓰기 |
| M19 | 머지 전에 SKILL.md 전문을 성진이 승인한다 | 성진 | skill-maker 자신의 완료 조건 |
| M20 | 기존 세 스킬의 이행은 범위 밖. 각 레포에서 새 skill-maker로 따로 한다 | 성진 | C11과 같은 방침 |
| M21 | 계획 파일명은 `skill-maker-code-design.md` | 성진 | — |
| M22 | 1~3절·6절은 그대로 둔다. 4절은 한 문장을 더한다. 5절은 제자리 수정이다: v0.2.0 문장을 모두 유지·수정·이동 중 하나로 처리하고, 삭제는 없다 | 성진 ("아예 다시 쓰려는거야?") | 통째 교체 초안에서 이유 다섯 가지가 빠졌던 실패 |
| M23 | 기존 두 줄은 유지한다: C7(레포에 구조 테스트)과 D17(다른 모델 계열의 검토 완료 조건) | 성진 | 도구 지시가 아니라 스킬 품질에 대한 판단 |

**넣지 않는 것** (이유)
- **표준 구조 테스트 파일, 스캐폴드:** M5.
- **codebase-design의 용어집·도식·"Rejected framings"·테스트 용이성 일반론:** 모델이 이미 안다. 테스트는 tdd의 몫이다(C9).
- **"한 어댑터는 가설 seam", 포트·어댑터:** 과잉 추상이 실측 0건이다.
- **삭제 테스트(pass-through 합치기):** 실측 근거가 없다. 게다가 "호출자가 하나면 합쳐라"로 읽히면 형식 지식을 가진 system 단위까지 합쳐 M15와 충돌한다(codex 리뷰). 재개 조건: 실사용에서 전달만 하는 단위가 반복될 때.
- **DESIGN-IT-TWICE:** 과정 스킬이라 skill-maker의 판단이 아니다.
- **leverage·locality 용어 해설:** 뒤의 구체적 기준이 같은 뜻을 말한다. "deep module"만 leading word로 쓴다.
- **`assets/` 칸:** 세 스킬에 쓰인 증거가 없다. 필요한 스킬이 나오면 "어긋나면 사용자에게" 원칙대로 올린다.
- **감사가 찾은 일반 버그 셋:** 스킬 특화가 아니다.
- **"처리 단계가 아니라 바뀌는 이유로 나눈다"(M16):** 새 스킬 제작에서는 모델이 이미 잘 나눴다. 재개 조건: 새로 만든 스킬에서 단계 분할이 나올 때.

## 최종 디렉터리 구조

```
Skill Maker/                                  (레포)
├── .claude/
│   ├── plans/skill-maker-code-design.md      (이 계획, 이름 변경 후)
│   └── skills/skill-maker/
│       └── SKILL.md                          (유일한 파일: 4절 한 문장 + 5절 제자리 수정)
~/.claude/skills/skill-maker -> <레포>/.claude/skills/skill-maker   (기존 심링크, 변경 없음)
```

skill-maker가 앞으로 만드는 스킬의 트리는 아래 5절 초안의 트리 블록과 같다.

## SKILL.md 변경 후 섹션 구조

frontmatter는 변경 없음. 본문은 영어, 하드랩 없음. `${` 형태는 쓰지 않는다(skill-maker 로드 시 치환된다). `$`와 `{CLAUDE_SKILL_DIR}`로 쪼개 쓰는 기존 표기는 유지한다.

1. `# Turning a user's expertise into a skill`: 변경 없음
2. `## What earns a place in a skill`: 변경 없음
3. `## Drawing out what the user knows`: 변경 없음
4. `## Writing judgment down`: "Put knowledge where it can act" 문단 끝에 한 문장(M12의 읽기 경계)
5. `## Code the skill bundles`: 제자리 수정(아래 초안과 보존 대조표)
   - 도입 문단: 변경 없음
   - 트리: `references/`·`data/`·`<helper>.py`·레포 `<job>/` 칸 추가, `tests/` 주석 수정
   - 항목 10개(기존 6개 + 새 3개 + 기존 하위 패키지 항목을 둘로 나눔): sys.path / 호출·런타임 / **명령의 자리(새)** / `cli.py` / **`--help`(새)** / stdout·종료코드 / 단위 / system·import 방향 / **`data/`(새)** / 테스트·레포 코드·구조 테스트
   - 레거시 이행 문단: 변경 없음
6. `## When the skill is done`: 변경 없음

### 4절 추가 문장 ("Put knowledge where it can act" 문단 끝에)

```markdown
An interface that re-expresses what the model does directly narrows it instead: a search command over a database the model could query offers only the filters its author thought of, and the skill then has to teach workarounds such as one search per spelling. Give the model a schema it can read and let it write the query.
```

### 5절 초안 (구현 시 codex 리뷰로 확정)

굵게 표시하지 않은 문장은 v0.2.0 원문 그대로다.

````markdown
## Code the skill bundles

A skill's directory is an interface like its `--help`: whoever opens the skill, the running model or a later session changing it, reads the tree before any file. So every skill's code has the same tree, folder names say what they hold, the skill folder holds only what the running model uses, and a test rather than this text keeps the tree in shape. The tree's value lies in being the same everywhere, so where a domain seems to want another shape, bend the contents rather than the tree, and bring a real misfit to the user instead of deviating quietly.

```
<skill>/
├── SKILL.md
├── references/           # read on one branch, behind a pointer that says when
├── data/                 # state the skill reads or writes at runtime
└── scripts/
    ├── cli.py            # the only entry point
    └── <skill_name>/     # the only package: the skill's name as a Python identifier (hyphens become underscores)
        ├── <feature>/    # one per reason to change, as many as there are
        ├── <system>/     # one per system or format someone else owns, named after it
        ├── <store>/      # one per kind of state, when there is any
        └── <helper>.py   # shared by every kind
<skill's source repository>/
├── tests/                # tests, fixtures, simulators
└── <job>/                # code only a maintainer or a scheduled job runs, named for the job
```

- Running `cli.py` directly puts `scripts/` first on `sys.path`, which already makes the package importable, so nothing edits `sys.path`; it also means a top-level name can shadow a standard-library module of the same name. Two fixed names make that one check; if the package name is a stdlib module, suffix it.
- Every skill is called as `uv run "<dir>/scripts/cli.py" <command> …`, with `<dir>` written as `$` followed by `{CLAUDE_SKILL_DIR}` in both the call and the `allowed-tools` pattern, so both read the same in every skill. A PEP 723 header at the top of `cli.py` states `requires-python` and every dependency, because the `python3` on PATH differs between a terminal, a hook and a scheduled job and can be too old for the code.
- **The model is the caller of every command, so a command earns its place by hiding work that goes the same way every time or that the model cannot do reliably, such as a write that has to keep several things consistent.**
- `cli.py` holds the whole command surface (parser, help text, dispatch, exit codes) and no domain logic, so the contract the model sees lives in one file**; choosing which items to act on, a fallback, a retry or a follow-up call that depends on the first one's result is domain logic and belongs to the unit the command calls.**
- **`--help` is read in two levels, often cut short: the top-level map, then the one command about to be called. So each `<command> --help` explains every argument it takes, offers closed sets as choices, and states its output and failures on its own, because argparse shows a parent's epilog only at the top. A map that outgrows a screen calls for fewer commands, not a third level.**
- stdout carries only the result the model acts on, one JSON document when it will parse it, with a cheap signal first (a summary, a size, the next action) and a handle to the costly detail **that names that result alone**, so the model decides before it pays **and a later call cannot overwrite what an earlier one returned**. stderr carries progress and diagnostics. 0 is success and 2 is bad arguments; `--help` defines any other exit code.
- Subpackage names come from the domain; the criteria for them do not. **Each unit, a module directly in the package or a subpackage, is a deep module: a small interface (the module itself, or the subpackage's `__init__.py`) in front of what it hides, and nothing outside the unit reaches past it, so its inside can change without touching callers or tests.** Split when a second reason to change appears, not before; until then modules sit directly in the package.
- Code that deals with a system or format someone else owns (a site's HTML, another CLI's output, an external API, an exported file) sits in one subpackage named after it, because it changes without notice and the fix should touch one folder**; it holds everything needed to talk to that system and hands features results in the skill's own terms, while what is done with the system, such as a bulk collector or a conversion for storage, belongs to the unit or job with that purpose**. Imports run one way, from features to systems and stores to shared helpers, so what features rely on never depends on them and a feature can change or go without touching the rest**; a feature built from another, such as a batch built from single runs, is an edge the structure test names**.
- **State the skill reads or writes lives in `data/`, so the tree says where it is, not in a home or cache directory; a skill shipped in a plugin keeps state that must outlive an update in the plugin's data directory instead, because an update replaces the skill folder.**
- Tests and anything only a maintainer **or a scheduled job** runs live in the repository the skill's source is kept in (for a symlinked install, the link target's), outside the skill folder; if the skill has no repository, agree on one with the user before writing tests. **That code may import the skill's unit interfaces, and the skill never imports it, because the skill must run from its own folder.** A tool the model itself must run becomes a subcommand. A structure test there checks the two top-level names, the import direction**, that nothing outside a unit reaches past its interface** and that nothing edits `sys.path`, so a change that breaks the tree fails there.

When a change to an existing skill touches the structure of code that predates this layout, offer the migration to the user as a decision of its own; until it is agreed, the old layout stands and the structure test does not apply to it.
````

(초안의 `**…**`는 계획 안에서 바뀐 부분을 보이려는 표시다. 실제 SKILL.md에는 굵게 쓰지 않는다.)

### 보존 대조표 (v0.2.0 5절 → 새 5절)

| v0.2.0 문장 | 처리 |
|---|---|
| 도입 문단 전체 | 유지 |
| 트리 블록 | 수정: `references/` `data/` `<helper>.py` `<job>/` 칸 추가, `tests/` 주석 "tests, fixtures, simulators, admin tools" → "tests, fixtures, simulators"(관리 도구는 `<job>/`로, M14) |
| sys.path·두 이름 항목 | 유지 |
| `uv run`·PEP 723 항목 | 유지 |
| "`cli.py` holds the whole command surface … lives in one file." | 유지 + 절 추가(결정하는 로직의 예) |
| "It describes itself: every argument explained in `--help`, closed sets offered as choices." | 이동: `--help` 항목의 "each `<command> --help` explains every argument it takes, offers closed sets as choices"로 옮기며 명령별로 좁힘(M6) |
| stdout 항목 | 유지 + 손잡이 절 추가(M18) |
| "Subpackage names come from the domain; the criteria for them do not." | 유지 |
| "Split when a second reason to change appears, not before; until then modules sit directly in the package." | 유지 + 앞에 깊은 모듈 문장(M9) |
| system 하위 패키지 문장(예시 넷 포함) | 유지 + 절 추가(M15). 기존 항목을 두 항목으로 나눈다 |
| import 방향 문장과 이유 | 유지 + 기능 간선 절(M13) |
| 테스트 위치 문장 | 유지 + "or a scheduled job", 레포 코드 import 방향 문장(M14) |
| "A tool the model itself must run becomes a subcommand." | 유지 |
| 구조 테스트 검사 목록 | 유지 + "that nothing outside a unit reaches past its interface"(M9) |
| 레거시 이행 문단 | 유지 |

네 프레임 자체 점검(구현 시 줄 단위로 다시 한다):
- **principle over rail:** 새 규칙마다 이유나 대비 사례가 붙어 있다.
  - 명령: 모델이 호출자다
  - help: 잘려 읽힌다, argparse 동작
  - 손잡이: 덮어쓰기
  - 단위: 안을 바꿔도 밖이 안 바뀐다
  - system: 목적 코드는 목적 쪽
  - 간선: 묶음 예
  - `data/`: 트리가 위치를 말한다, 플러그인 경계
  - 레포 코드: 스킬은 자기 폴더만으로 돈다
  - 기존 이유는 모두 유지했다.
- **interface over document:** 명령 계약은 `--help`가, 트리 준수는 각 레포의 구조 테스트가 소유한다. 본문은 검사 목록 한 마디만 더한다.
- **for the model:** 실측 수치, 스킬 이름, Pocock 출처, `${…}` 변수명이 없다. 판단 사례는 일반형(검색 명령, 처리할 항목 고르기, 실행 묶음)으로 썼다.
- **dense:** 일반론·테스트 방법·포트·삭제 테스트를 뺐다. 명령 기준은 5절 한 곳에만 있고, 4절은 인터페이스 일반의 읽기 경계만 말한다.
  - 모델이 이미 하는 것은 한 마디로 줄였다(실측 근거). 최상위 help 지도는 "두 단계로 읽힌다"로, 프로토콜·형식을 명령으로 만드는 것은 생략하고 새 정보인 "규칙이 딸린 쓰기"만 남겼다.
  - `cli.py` 절은 실측에서 "no domain logic"이 지켜지지 않은 모양(처리할 항목 고르기)을 예로 넣었다.
  - 남는 위험이 있다. M13(기능 간선)과 M14(유지보수 코드 자리), 그리고 M15의 뒷부분(목적 코드는 목적 쪽)은 실사용 실패 보고가 근거이고, 재측 과제로는 행동 변화가 검증되지 않는다. 기존 스킬을 이행할 때 관찰하고, 릴리스 노트에 적는다. M15의 앞부분(형식을 빠짐없이 숨김)은 재측 (viii)로 확인한다.

### codex 계획 리뷰

- **thread:** `01a0e85f-d7a1-7140-b1d6-226dd8467056`. 이 판에 대한 마지막 라운드는 `findings: []`였다.
- **초안에 반영된 지적 네 가지:**
  - "호출자가 하나면 합쳐라"로 읽히는 삭제 테스트 문장을 뺐다.
  - 명령 기준을 5절 한 곳에만 둔다.
  - "a second call"을 "a follow-up call that depends on the first one's result"로 좁혔다.
  - 도입 문단을 v0.2.0 그대로 둔다.

## 구현 단계 (다음 세션)

파일을 바꾸는 단계가 3개 이상이므로 시작할 때 `TaskCreate` 트래커를 열고, 각 단계의 완료 판정을 description에 적는다. 이번 변경에는 코드가 없어 TDD 단계가 없다(M5).

0. **준비**
   - 이 계획 파일의 이름을 `.claude/plans/skill-maker-code-design.md`로 바꾼다. 승인 직후 이미 바꿨으면 생략한다.
   - `main`에서 `feat/skill-maker-code-design` 브랜치를 판다. 첫 커밋에는 계획 파일만 넣는다.
   - 관련 없는 작업 트리 변경은 커밋하지 않는다: `.claude/plans/user-inputs/…` 삭제, `.claude/histories`, `.codex-runs/`.
   - 실측 자료 `/private/tmp/claude-501/-Users-seongjin-Coding-Skill-Maker/21f8bf6d-91dd-46f6-9d5c-cb74a81b642a/scratchpad/measure/`를 레포 `.tmp/measure/`(gitignore됨)로 복사한다. 사라졌으면 부록 A로 다시 만든다.
   - 완료 판정: 브랜치가 있고, 첫 커밋에 계획 파일만 있으며, `.tmp/measure/{run.sh,prompts,inputs}`가 있다.
1. **SKILL.md 수정**
   - 4절 끝에 한 문장을 더하고, 5절은 초안대로 제자리에서 고친다.
   - 완료 판정:
     - `git diff`가 4절 한 곳과 5절만 바꾼다.
     - 보존 대조표의 모든 행이 diff에서 확인된다. 대조표에 없는 삭제는 없다.
     - `grep -c '\${' "/Users/seongjin/Coding/Skill Maker/.claude/skills/skill-maker/SKILL.md"`가 `0`을 출력한다. 매치가 없으면 grep의 종료코드는 1이고, 이것이 기대값이다.
     - `claude plugin validate --strict "/Users/seongjin/Coding/Skill Maker/.claude/skills"`가 exit 0이다.
2. **codex 문서 리뷰 루프**
   - `codex` 스킬, gpt-6-astra medium, read-only.
   - 계획 리뷰 thread `01a0e85f-d7a1-7140-b1d6-226dd8467056`을 resume한다. thread를 쓸 수 없으면 새로 시작하고, 이 계획의 '넣지 않는 것'을 입력으로 함께 넘긴다.
   - 입력:
     - 변경 후 SKILL.md 전문
     - v0.2.0 원문(`git show main:.claude/skills/skill-maker/SKILL.md`)
     - 이전 결정 C1~C13(`.claude/plans/skill-maker-bundled-code.md`)
     - 이 계획의 결정 장부와 보존 대조표
   - 질문: 네 프레임 위반, 보존 누락(v0.2.0의 이유·결정이 사라졌는가), 두 모델 실행이 다르게 읽을 문장.
   - `--schema {line, frame, execution_failure, proposed_fix, severity: execution|taste}`
   - execution 0건까지 반복한다. 장부와 충돌하는 지적은 성진에게 AskUserQuestion으로 올린다. taste는 반영하거나 기각 사유를 PR 본문에 적는다.
   - 완료 판정: 최신 라운드에서 execution 0건이다.
3. **구현 후 재측(M4)**
   - `CONDS=C .tmp/measure/run.sh post "/Users/seongjin/Coding/Skill Maker/.claude/skills/skill-maker/SKILL.md"`로 과제 둘을 새 본문으로 돌린다. 조건 C의 프롬프트 형식은 B와 같다.
   - 과제별로 판정할 것:
     - (i) 모델이 쿼리할 수 있는 저장소(SQLite 등)가 있으면, 그 읽기를 감싸는 명령이 없고 SKILL.md가 모델에게 스키마를 읽고 쿼리하게 한다. 규칙이 딸린 쓰기는 명령이다.
     - (ii) 단위가 입구로만 드나든다(cli·기능·테스트). 레포의 구조 테스트가 단위 입구를 검사한다.
     - (iii) 데이터가 `data/`에 있다.
     - (iv) 모든 `<command> --help`가 제 실패를 말한다.
     - (v) B 대비 다른 축(진입점·트리·테스트 위치)에 퇴행이 없다.
     - (vi) 큰 결과를 파일 손잡이로 넘기는 스킬이면, 서로 다른 조회 두 번과 같은 조회의 갱신 한 번을 실행해도 먼저 받은 손잡이의 내용이 그대로다. 손잡이가 없으면 "해당 없음"과 근거를 적는다.
     - (vii) `cli.py`에 처리할 항목 고르기·폴백·결과에 따른 후속 호출 같은 결정이 없다.
     - (viii) 외부 시스템의 원시 형식(ECOS의 서비스명·필드명·코드 규칙, Pocket CSV의 열 이름)이 system 단위 밖의 기능 코드에 나오지 않는다.
   - codex 감사 thread `01a0e84f-ff0c-7763-966a-67024649bcfa`를 resume해 같은 축으로 판정한다.
   - 기준을 못 넘으면: 지식 부족인지 실행 실패인지 먼저 가른다. 가장 작은 범위의 문장을 고치고 그 과제만 한 번 더 돈다. 그래도 안 되면 사실대로 기록하고 성진에게 올린다.
   - 재측 중 SKILL.md를 고쳤다면, 4단계로 가기 전에 1단계 검증(diff·보존 대조·`${` grep·validate)과 2단계 문서 리뷰(같은 thread resume)를 고친 본문으로 다시 통과시킨다.
   - 완료 판정: (i)~(viii)의 판정이 과제별로 기록돼 있다. 마지막 1·2단계 결과가 지금의 SKILL.md에 대한 것이다.
4. **전문 승인(M19)**
   - 성진에게 보여 줄 것: 변경 후 SKILL.md 전문, v0.2.0 대비 diff, 재측 결과.
   - AskUserQuestion으로 승인받는다. 부분 승인이나 침묵은 승인이 아니다.
   - 완료 판정: 성진이 명시적으로 승인했다.
5. **PR과 머지**
   - 커밋하고 푸시한 뒤 PR을 연다. `gh pr merge --squash`로 머지한다.
   - 제목 예: `feat: skill-maker에 스킬 코드 설계 판단을 더한다`
   - 본문: `## 무엇을 바꿨나` / `## 왜` / `## 영향` / `## 검증`
   - `## 왜`에 담을 것(개발 기록 = 스킬 밖):
     - 실측 1·재측 표 요약
     - codex 감사·리뷰 처리와 기각 사유
     - '넣지 않는 것'과 재개 조건
     - `${…}` 미사용과 `$`+`{CLAUDE_SKILL_DIR}` 분리 표기의 이유
     - 읽기/쓰기 구분의 증거
   - 완료 판정: PR이 머지되고 `main`에 반영됐다. 심링크라 `~/.claude/skills/skill-maker/SKILL.md`에 바로 반영된다(`diff`로 확인).
6. **GitHub 릴리스(M7)**
   - 머지 커밋에 `v0.3.0` 태그를 붙인다. 제목은 `v0.3.0: 스킬 코드 설계 판단`이다.
   - 본문 섹션은 v0.2.0과 같은 이름·순서다.
     - `## 무엇이 들어 있나`: 명령의 자리와 조회를 감싸지 않기, `cli.py`의 결정 로직, 명령별 help, 손잡이, 단위=깊은 모듈, system 단위, 기능 간선, `data/`, 레포 코드 자리, 구조 테스트의 입구 검사
     - `## 설치`: 심링크, 이미 설치했으면 `git pull`
     - `## 알려진 한계`: 원샷 실측(과제 둘), M13·M14·M15 뒷부분의 행동 변화 미검증, 기존 스킬 미이행, 레포마다 구조 테스트를 따로 짬
   - 명령: 본문을 스크래치패드 파일로 쓰고 `gh release create v0.3.0 --target main --title "v0.3.0: 스킬 코드 설계 판단" --notes-file <파일>`
   - 완료 판정: `gh release view v0.3.0`이 Latest이고, `git rev-parse v0.3.0^{commit}`이 `origin/main`과 같다.

## Verification

- **구조:** `claude plugin validate --strict "/Users/seongjin/Coding/Skill Maker/.claude/skills"`가 exit 0이다. `${` grep 출력은 0이다.
- **보존:** 보존 대조표의 모든 행이 diff에서 확인되고, 대조표 밖의 삭제는 없다.
- **문서 품질:** codex 문서 리뷰의 최신 라운드는 execution 0건이고, 그 라운드가 검토한 본문이 승인받을 SKILL.md와 같다.
- **행동:** 재측에서 (i)~(viii)를 과제별로 판정한다. 원샷 표본 둘이라 실사용 품질의 증명이 아니다.
- **승인:** 성진이 전문을 승인했다.
- **배포:** `v0.3.0`이 Latest이고 머지 커밋을 가리킨다.
- **검증되지 않는 것:**
  - 수십~수백 번의 실사용 품질
  - M13·M14와 M15 뒷부분의 행동 변화
  - 레포마다 따로 짜는 구조 테스트가 입구 검사를 제대로 구현하는지(기존 스킬 이행 때 관찰)

## 부록 A: 실측 재현 자료

- **위치:** 이 세션 스크래치패드 `measure/`. 0단계에서 `.tmp/measure/`로 복사한다.
  - `run.sh <label> [SKILL.md 경로]`는 환경변수 `CONDS`(기본 `A B P`)의 조건마다 과제 둘을 병렬로 돈다. 각 실행은 새 git 레포(`work/`, `inputs/` 포함)에서 돈다.
  - 산출물은 `runs/<label>-<task>-<cond>/{prompt.md,result.json,work/}`이다. 조건 B·C·P의 프롬프트는 "Base directory for this skill: …" + 본문 + `ARGUMENTS: <과제>` 형식이다.
- **입력.** 경로가 사라졌을 때 다시 만들 기준이다.
  - `inputs/reading/pocket_export.csv`: Pocket 내보내기 형식(`title,url,time_added,tags,status`) 8행. 제목 빈 행 1개, 추적 파라미터만 다른 중복 1쌍, 한글 제목 1개, 태그는 `|` 구분, 상태는 unread/archive다.
  - `inputs/ecos/API.md`: ECOS 요청 URL 형식, 서비스 5종(`StatisticTableList` `StatisticItemList` `StatisticSearch` `KeyStatisticList` `StatisticWord`)의 인자와 row 필드, 주기별 날짜 형식, 정상·오류 응답 형식, 오류 코드 5개(INFO-100/200, ERROR-101/300/602), 인증키 환경변수 `ECOS_API_KEY`.
  - `inputs/ecos/samples/`: 응답 샘플 다섯 개.
    - 기준금리 722Y001 월별 2024-01~12 12행(3.5×9, 3.25, 3, 3)
    - 901Y009 항목 4행
    - 통계표 목록 6행(`list_total_count` 1248)
    - 100대 지표 4행
    - `INFO-200` 오류
- **`run.sh`가 사라졌을 때 다시 만드는 법:**
  - 과제마다 새 폴더에서 돌린다: 입력 사본을 넣고 `git init` 한다.
  - 조건 C의 프롬프트는 `Base directory for this skill: /skills/skill-maker` + 새 SKILL.md 본문(frontmatter 제외) + `ARGUMENTS: <과제 프롬프트>` 형식으로 만든다.
  - 명령: 실측 1의 `claude -p …`에 `--output-format json`을 붙이고, 허용 명령을 `Bash(uv *)` `Bash(python3 *)` `Bash(pytest *)` `Bash(mkdir *)` `Bash(ls *)` `Bash(cat *)` `Bash(git *)` `Bash(sqlite3 *)`로 둔다. 프롬프트는 stdin으로 넘긴다. 두 과제는 병렬로 돌린다.

- **과제 프롬프트(원문):**

```text
[task_reading.md]
읽기 목록을 대화 중에 관리하는 Claude Code 스킬을 만들어 줘. 스킬 이름은 `reading-list`이고, 이 레포의 `skills/reading-list/`에 만든다(설치는 나중에 내가 심링크로 한다).

하고 싶은 일:
- 대화 중에 URL을 저장하면 글 제목을 알아서 가져와 함께 저장한다.
- 저장한 글을 대화 중에 찾아볼 수 있어야 한다. 예: "지난달 저장한 SQLite 관련 글 중 안 읽은 것".
- 읽은 글은 읽음으로 표시한다.
- 목록을 Markdown으로 내보낸다.
- Pocket에서 내보낸 CSV를 가져온다(`inputs/pocket_export.csv`가 실제 예시). 같은 글이 추적 파라미터만 다른 URL로 두 번 들어 있을 수 있다.
- 데이터는 SQLite에 둔다.

코드와 테스트까지 만들고, 테스트가 통과하는 상태로 끝내 줘.

지금 나는 질문에 답할 수 없다. 필요한 결정은 스스로 내리고, 내린 가정은 마지막 답에 목록으로 남겨 줘. 이 환경에서는 `claude` CLI와 다른 모델의 검토를 쓸 수 없고, 테스트는 네트워크 없이 돌아야 한다.

[task_ecos.md]
한국은행 ECOS Open API로 경제 통계를 대화 중에 찾아 쓰는 Claude Code 스킬을 만들어 줘. 스킬 이름은 `ecos`이고, 이 레포의 `skills/ecos/`에 만든다(설치는 나중에 내가 심링크로 한다).

하고 싶은 일:
- "기준금리 최근 1년 추이", "소비자물가가 1년 전보다 얼마나 올랐나" 같은 질문에 답하려면 필요한 통계를 찾아 가져와야 한다. 나는 어떤 통계표·항목 코드인지 모른다.
- 주요 지표(100대 통계)를 빠르게 본다.
- 한 번 받은 시계열은 로컬에 저장해 두고, 같은 것을 다시 API에 묻지 않는다.
- API 문서 요약은 `inputs/API.md`, 실제 응답 샘플은 `inputs/samples/`에 있다. 인증키는 환경변수 `ECOS_API_KEY`로 받는다.

코드와 테스트까지 만들고, 테스트가 네트워크 없이 통과하는 상태로 끝내 줘.

지금 나는 질문에 답할 수 없다. 필요한 결정은 스스로 내리고, 내린 가정은 마지막 답에 목록으로 남겨 줘. 이 환경에서는 `claude` CLI와 다른 모델의 검토를 쓸 수 없고, 테스트는 네트워크 없이 돌아야 한다.
```
