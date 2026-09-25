# skill-maker: 번들 코드 구조 관례 추가 계획

> 계획 세션에서 승인됨. 파일명은 승인 직후 `.claude/plans/skill-maker-bundled-code.md`로 변경 완료.

## Context

skill-maker로 코드를 포함한 스킬을 만들 때마다, 단일 CLI 진입점 + 책임 단위 하위 폴더 구조로 만들려면 성진이 여러 번 지적해야 했고, 그렇게 만든 스킬끼리도 구조가 일관되지 않았다. 목표는 skill-maker에 **스킬 번들 코드의 구조 관례**를 담아 앞으로 만들 스킬이 지적 없이 같은 구조로 나오게 하는 것이다.

핵심 원리(C13): 스킬 디렉터리 트리는 `--help`보다 먼저 읽히는 인터페이스이므로 문서·CLI와 똑같이 네 프레임을 반영해야 한다.

성격: 이것은 팀 관례다. 관례는 모든 스킬에서 같을 때만 가치가 있으므로 고정 형태(rail)가 정당화된다. principle over rail에 따라 그 이유와, 고정을 뒤집는 조건(진짜 부적합은 조용히 어기지 않고 사용자에게 올린다)을 함께 적어 관례가 다루지 않는 경우에도 같은 방향으로 판단하게 한다.

범위: 이 레포의 `.claude/skills/skill-maker/SKILL.md` 한 파일. 재작성은 하지 않는다 — 기존 본문은 리뷰를 거친 판단이고 빠진 것은 코드 구조 관례뿐이다(아래 실측). 기존 세 스킬의 이행은 범위 밖(C11).

### 실측 1: 기존 스킬 세 개의 불일치 (2026-09-25)

| 축 | News (`Naver-News`) | codex (`codex in claude`) | ultra-search (`Ultra-Search`) |
|---|---|---|---|
| 진입점 파일명 | `scripts/news.py` | `scripts/cli_codex.py` | `scripts/cli.py` |
| 진입점 두께 | 191줄: 파서·help 전부 | 15줄: `from cli import main` | 490줄: 파서·help 전부 |
| 패키지 루트 | 평평한 최상위 여러 개 | 평평한 최상위 + `util.py` `worktree.py` | 단일 `ultra_search/` |
| 분할 축 | 기능 + 저장소 | 계층 | 하위 시스템 |
| sys.path | 안 건드림 | 안 건드림 + 테스트로 금지 | `sys.path.insert` |
| 런타임 | PEP 723 + `uv run` | `python3` stdlib | `python3` stdlib |
| 유지보수 코드 | `scripts/dev/` (스킬 안) | 레포 `tests/` | 레포 `tests/` |
| 구조 강제 | 없음 | `tests/test_layering.py` | 없음 |

### 실측 2: 기본값 대조 (격리 헤드리스, `claude -p --safe-mode --restricted --permission-mode acceptEdits --model claude-opus-5-5`)

같은 과제(SQLite 읽기 목록 스킬: URL 추가·제목 수집·검색·읽음 표시·Markdown 내보내기·Pocket CSV 가져오기, 코드와 테스트 포함)를 두 조건에서 실행.
- A 스킬 없음 / B 현재 skill-maker 본문을 `--append-system-prompt-file`로 부여.
- 둘 다 `.claude/` 아래 쓰기가 권한으로 막혀 파일은 못 만들었지만(`permission_denials`에 B의 전체 스크립트 본문이 남음), 계획한 구조는 A·B 동일: `scripts/reading_list.py` 한 파일(B 약 330줄, `# --- storage ---` `# --- title fetching ---` `# --- CLI ---` 주석 구획), `python3` stdlib, 테스트는 스킬 폴더 안 `tests/`.
- 결론: 현재 본문은 구조를 바꾸지 못한다 → 관례는 모델에 없는 지식이다. 모델이 주석으로 나눈 구획(저장소·외부 시스템·기능)은 C3 기준과 일치하므로 C3는 모델의 자연스러운 경계를 폴더로 옮기는 것이다.
- 참고: 이 실측을 다시 돌릴 일이 생기면 스킬 경로를 `.claude/` 밖(`skills/<name>/`)으로 줘야 쓰기가 막히지 않는다.

## 결정 장부

| # | 결정 | 소유자 | 근거·영향 |
|---|---|---|---|
| C1 | 최상위는 `scripts/cli.py` + `scripts/<스킬명_snake>/`(패키지 하나)뿐. 다른 코드는 전부 그 패키지 안. 스킬명이 stdlib 모듈명이면 패키지명에 접미사. `sys.path`는 건드리지 않는다 | 성진 | 사실: Python은 진입점 폴더를 `sys.path` 맨 앞에 넣어 최상위 이름이 전역 import 이름이 되고 stdlib를 가릴 수 있다. 이름 둘 → 충돌 검사 한 번. 진입점 고정 → 호출문·`allowed-tools` 패턴이 모든 스킬에서 같다 |
| C2 | `cli.py`는 규모와 상관없이 명령 표면 전부(파서·help/epilog·분기·종료코드)를 들고 도메인 로직은 두지 않는다 | 성진 | 모델이 보는 계약이 한 파일에. 길이를 묶는 것은 "로직 없음" 조건 |
| C3 | 하위 패키지 이름은 도메인이 정하고 기준 셋만 고정: ① 바뀌는 이유 하나, 둘째 이유 전엔 나누지 않음 ② 남이 소유한 시스템을 다루는 코드는 그 시스템 이름의 하위 패키지 하나에 ③ import는 기능 → 시스템·저장소 → 공용 한 방향 | 성진 | ② 예고 없이 깨지는 곳이라 고칠 곳이 한 폴더여야. 이름까지 고정하면 도메인에 안 맞는 칸이 생김 |
| C4 | 관례·트리는 references로 떼지 않고 SKILL.md 안 | 성진 (Claude는 references 추천) | 코드 없는 스킬 제작에도 읽히므로 최소 분량으로. 기존 D4 유지 |
| C5 | 항상 `uv run` + `cli.py` 맨 위 PEP 723 헤더(`requires-python`, `dependencies`) | 성진 | 사실: 이 맥의 `python3`는 PATH에 따라 3.12 또는 3.9.6. 대가: uv 없는 환경엔 공유 불가 |
| C6 | 테스트·유지보수자 전용 코드는 스킬 폴더 밖, 스킬 소스가 있는 레포(심링크 설치면 대상 쪽)의 `tests/` 등. 레포가 없으면 테스트를 쓰기 전에 사용자와 정한다. 모델이 돌려야 하는 도구는 `cli.py` 서브커맨드 | 성진 | 스킬 폴더는 실행 모델이 읽고 패키지되는 곳 |
| C7 | 코드를 만들 때 레포 `tests/`에 구조 테스트: 최상위 이름, import 방향, `sys.path` 편집 없음 | 성진 | interface over document — 어기면 테스트가 알린다 |
| C8 | 출력은 보편적인 것만 고정: stdout은 모델이 쓸 결과만(파싱할 거면 JSON 한 장), stderr는 진행·진단, 0 성공, 2 인자 오류. 나머지 종료코드는 스킬별 `--help` | 성진 | 도메인마다 실패 종류가 달라 번호표 통일은 억지 칸을 만든다 |
| C9 | 테스트 seam·방법론은 skill-maker에 쓰지 않는다 — tdd 스킬(전역 CLAUDE.md가 이미 연결)의 몫 | 성진 | skill-maker의 목적은 좋은 스킬. 중복 제거(dense) |
| C10 | 관례 이전의 기존 스킬을 고칠 때 부수 효과로 이행하지 않는다. 변경이 코드 구조에 닿으면 이행을 별도 결정으로 제안하고, 합의 전까지 기존 레이아웃이 유지되며 구조 테스트도 적용하지 않는다 | 성진 | 전역 CLAUDE.md "건드려야 하는 것만" |
| C11 | 기존 세 스킬 이행은 이번 범위 밖. 각 레포에서 개선된 skill-maker로 따로 | 성진 | 이행 자체가 새 관례의 실전 검증 기회 |
| C13 | 스킬 디렉터리 트리 자체가 인터페이스다 — 파일보다 먼저 읽히므로 네 프레임을 트리에도 적용한다(같은 트리, 스스로 설명하는 폴더명, 스킬 폴더엔 실행 모델이 쓰는 것만, 관례는 테스트가 지킴). 5절 첫 문단의 근거 | 성진 | 관례의 정당화를 '탐색 편의'에서 '인터페이스'로 |
| C12 | TDD·e2e 없음. 바꾸는 것이 SKILL.md 한 파일이라 TDD 대상 코드가 없고(기존 D11), 사후 헤드리스 재실행(before/after)도 하지 않는다. 검증은 `claude plugin validate --strict` + codex 자체 리뷰뿐. 모델에 없는 지식이라는 근거는 계획 세션의 실측 2로 충분 | 성진 | TDD는 skill-maker가 만드는 스킬의 코드에 전역 CLAUDE.md 경로로 적용된다. 원샷 실행은 실사용 품질을 증명하지 못한다(기존 D10 방침) |

**넣지 않는 것** (이유)
- help 문구 언어: 본문 언어를 사용자와 정하는 기존 원칙(P3)에서 따라 나온다.
- 테스트 환경의 의존성 선언(PEP 723 의존성을 pytest 환경에도 두는 법): 모델이 테스트를 돌리다 바로 만나는 오류라 스스로 해결한다. 실사용에서 반복되면 재개.
- 스캐폴드 스크립트: 새로 유지보수할 코드이고, 트리 몇 줄이 같은 일을 한다. 관례 위반이 실사용에서 반복되면 재개.
- 구조 테스트 코드 템플릿: 무엇을 강제하는지만 쓰고 코드는 그 자리에서 짧게. 같은 이유로 재개 조건 동일.

## 최종 스킬 디렉터리 구조 (변경 없음)

```
Skill Maker/                              (레포)
└── .claude/skills/skill-maker/
    └── SKILL.md                          (유일한 파일, 이번 변경 대상)
~/.claude/skills/skill-maker -> <레포>/.claude/skills/skill-maker   (기존 심링크)
```

## SKILL.md 변경 후 섹션 구조

frontmatter 변경 없음. 본문 영어, 하드랩 없음(D13).

1. `# Turning a user's expertise into a skill` — 변경 없음
2. `## What earns a place in a skill` — 변경 없음
3. `## Drawing out what the user knows` — 변경 없음
4. `## Writing judgment down` — 한 곳 수정: "Put knowledge where it can act" 문단에서 "A bundled CLI describes itself…" 문장을 떼어 5로 옮긴다(코드 관련 판단을 한 곳에 모음). 나머지 그대로.
5. **`## Code the skill bundles`** — 신설(아래 초안)
6. `## When the skill is done` — 코드 항목 한 줄 수정: "…and its `--help` matches what it does" 뒤에 "and the repo's structure test passes"를 잇는다.

### 5절 초안 (영어. codex 1차 리뷰 반영본 — 구현 시 이 문장을 기준으로 2차 리뷰를 거쳐 확정)

````markdown
## Code the skill bundles

A skill's directory is an interface like its `--help`: whoever opens the skill, the running model or a later session changing it, reads the tree before any file. So every skill's code has the same tree, folder names say what they hold, the skill folder holds only what the running model uses, and a test rather than this text keeps the tree in shape. The tree's value lies in being the same everywhere, so where a domain seems to want another shape, bend the contents rather than the tree, and bring a real misfit to the user instead of deviating quietly.

```
<skill>/
├── SKILL.md
└── scripts/
    ├── cli.py            # the only entry point
    └── <skill_name>/     # the only package: the skill's name as a Python identifier (hyphens become underscores)
        ├── <feature>/    # one per reason to change, as many as there are
        ├── <system>/     # one per system or format someone else owns, named after it
        └── <store>/      # one per kind of state, when there is any
<skill's source repository>/tests/   # tests, fixtures, simulators, admin tools
```

- Running `cli.py` directly puts `scripts/` first on `sys.path`, which already makes the package importable, so nothing edits `sys.path`; it also means a top-level name can shadow a standard-library module of the same name. Two fixed names make that one check; if the package name is a stdlib module, suffix it.
- Every skill is called as `uv run "<dir>/scripts/cli.py" <command> …`, with `<dir>` written as `$` followed by `{CLAUDE_SKILL_DIR}` in both the call and the `allowed-tools` pattern, so both read the same in every skill. A PEP 723 header at the top of `cli.py` states `requires-python` and every dependency, because the `python3` on PATH differs between a terminal, a hook and a scheduled job and can be too old for the code.
- `cli.py` holds the whole command surface (parser, help text, dispatch, exit codes) and no domain logic, so the contract the model sees lives in one file. It describes itself: every argument explained in `--help`, closed sets offered as choices.
- Subpackage names come from the domain; the criteria for them do not. Split when a second reason to change appears, not before; until then modules sit directly in the package. Code that deals with a system or format someone else owns (a site's HTML, another CLI's output, an external API, an exported file) sits in one subpackage named after it, because it changes without notice and the fix should touch one folder. Imports run one way, from features to systems and stores to shared helpers, so what features rely on never depends on them and a feature can change or go without touching the rest.
- stdout carries only the result the model acts on, one JSON document when it will parse it, with a cheap signal first (a summary, a size, the next action) and a handle to the costly detail, so the model decides before it pays. stderr carries progress and diagnostics. 0 is success and 2 is bad arguments; `--help` defines any other exit code.
- Tests and anything only a maintainer runs live in the repository the skill's source is kept in (for a symlinked install, the link target's), outside the skill folder; if the skill has no repository, agree on one with the user before writing tests. A tool the model itself must run becomes a subcommand. A structure test there checks the two top-level names, the import direction and that nothing edits `sys.path`, so a change that breaks the tree fails there.

When a change to an existing skill touches the structure of code that predates this layout, offer the migration to the user as a decision of its own; until it is agreed, the old layout stands and the structure test does not apply to it.
````

6절 수정 문장(기존 코드 항목을 이렇게 바꾼다):

```markdown
- Code you created or changed was checked against its own contract (inputs, outputs, failures) by the means that code is verified with, and its `--help` matches what it does; new code, and code migrated with the user's agreement, also passes the structure test.
```

네 프레임 자체 점검(구현 시 줄 단위로 다시 한다):
- principle over rail: 첫 문단이 트리를 인터페이스로 정당화하고(C13), 고정 형태마다 이유 한 절, 뒤집는 조건(부적합은 사용자에게, 기존 레이아웃은 합의 전까지 유지)과 가변 부분(하위 패키지 이름)을 명시.
- interface over document: 트리 자체가 인터페이스이고, 관례 준수는 구조 테스트가, 명령 계약은 `--help`가 소유. 본문은 둘 다 복제하지 않는다.
- for the model: 기존 스킬 이름, 실측 날짜, 특정 머신의 파이썬 버전 없음. `$`+`{CLAUDE_SKILL_DIR}`를 쪼개 쓰는 이유(skill-maker 로드 시 치환됨)는 본문이 아니라 커밋 본문에.
- dense: 모델이 이미 아는 것(argparse, PEP 723 문법, pytest 설정) 없음. 기존 CLI 문장은 옮겨 합쳐 중복 없음. 1차 리뷰의 방어 절("stays readable however many commands") 삭제.

### codex 1차 리뷰 (계획 세션, gpt-6-astra medium, read-only, thread `01a0d8f8-c98e-7b82-b20c-b868d0fc3d4f`)

| 지적 | 등급 | 처리 |
|---|---|---|
| 본문의 `${CLAUDE_SKILL_DIR}`가 skill-maker 로드 시 skill-maker 경로로 치환되어 생성 스킬에 잘못된 경로가 들어간다 | execution | 반영: `$`와 `{CLAUDE_SKILL_DIR}`로 쪼개 기술. 1단계 완료 판정에 grep 검사 추가 |
| 레포가 없는 스킬·심링크 설치에서 테스트 위치가 모호 | execution | 반영: 스킬 소스 레포(심링크면 대상 쪽), 없으면 사용자와 정함 |
| 패키지명 snake_case 규칙 누락(하이픈) | execution | 반영: 트리 주석에 명시 |
| 외부 소유 파일 형식(Pocket CSV)이 '시스템'으로 안 읽힘 | execution | 반영: "system or format … an exported file" |
| 이행 제안 트리거가 C10보다 넓고, 구조 테스트 완료 조건이 레거시 수정을 막음 | execution | 반영: "touches the structure", 합의 전 레거시엔 구조 테스트 비적용, 6절 문장 범위 한정 |
| stdlib 가림 서술이 과함(built-in·frozen은 못 가림) | taste | 반영: "can shadow" + sys.path 금지의 이유(직접 실행이 이미 import 가능하게 함) 추가 |
| "worth something only because…"가 방어 논증 | taste | 부분 반영: 삭제하지 않고 행동 규칙("bend the contents rather than the tree")의 이유로 재배치 — 관례를 지역 최적화로 깨지 않게 하는 근거라 필요 |
| "stays readable however many commands" 근거 없음 | taste | 반영: 삭제 |
| (성진 지적) 트리의 `<feature>/` 한 줄이 '하위 폴더 하나'로 읽힌다 | execution | 반영: 트리 주석을 "one per …, as many as there are"로, 작은 스킬은 패키지 바로 아래 모듈이라고 명시 |
| import 방향에 이유 없음 | taste | 반영: 이유 절 추가 |

## 구현 단계 (다음 세션)

파일을 바꾸는 단계가 3개 이상이므로 시작 시 `TaskCreate` 트래커를 열고 각 단계의 완료 판정을 description에 그대로 적는다.

0. **준비** — 이 계획 파일 이름을 `.claude/plans/skill-maker-bundled-code.md`로 바꾸고(계획 승인 직후 이미 바꿨으면 생략), `main`에서 `feat/skill-maker-code-layout` 브랜치를 판다. 계획 파일을 첫 커밋에 포함한다. 작업 트리의 관련 없는 변경(`.claude/plans/user-inputs/…` 삭제, `.claude/histories`, `.codex-runs/`)은 커밋하지 않는다.
   - 완료 판정: 브랜치 존재, 첫 커밋에 계획 파일만 있음.
1. **SKILL.md 수정** — 위 섹션 구조대로 4절에서 CLI 문장을 떼고, 5절을 초안대로 신설하고, 6절 한 줄을 고친다. 하드랩 없음.
   - 완료 판정: `git diff`가 세 곳만 바꾼다. `grep -cF '${CLAUDE_SKILL_DIR}' .claude/skills/skill-maker/SKILL.md`가 0(로드 시 치환 방지). `claude plugin validate --strict "/Users/seongjin/Coding/Skill Maker/.claude/skills"` exit 0.
2. **codex 리뷰 루프** — `codex` 스킬로 gpt-6-astra, medium. 입력: 변경 후 SKILL.md 전체, 네 프레임 정의(SKILL.md의 "What earns a place" 절), 이 계획의 결정 장부 C1~C13과 1차 리뷰 처리표. 계획 세션의 1차 리뷰 스레드를 resume해 기각·반영 이력을 이어 간다. `--schema` 응답: `{line, frame, execution_failure, proposed_fix, severity: execution|taste}`. execution 0건까지 고치고 같은 스레드를 resume해 재리뷰. 결정 장부와 충돌하는 지적은 성진에게 AskUserQuestion으로 올린다(임의로 결정을 뒤집지 않음). taste는 반영하거나 기각 사유를 PR 본문에 기록.
   - 완료 판정: 최신 라운드 execution 0건, 장부 충돌 지적은 성진 결정이 있음.
3. **PR과 머지** — 논리 단위로 커밋, 푸시, PR, `gh pr merge --squash`. 제목 예: `feat: skill-maker에 번들 코드 구조 관례 추가`. 템플릿이 없으므로 `## 무엇을 바꿨나` / `## 왜` / `## 영향` / `## 검증`. `## 왜`에는 실측 1·2 요약, codex 리뷰 처리(기각 사유 포함), `$`+`{CLAUDE_SKILL_DIR}` 분리 표기의 이유, '넣지 않는 것'의 재개 조건(= 개발 기록, 스킬 밖)을 담는다. 커밋 본문에도 분리 표기의 이유를 한 줄 남긴다(다음 세션이 합쳐 버리지 않게).
   - 완료 판정: PR 머지, `main` 반영. 심링크라 `~/.claude/skills/skill-maker`에 즉시 반영됨을 `diff`로 확인.

4. **GitHub 릴리스** — 머지 커밋에 `v0.2.0` 태그로 릴리스한다(기존 `v0.1.0`에 기능 추가 → 0.x의 minor 증가). 제목과 본문은 `v0.1.0` 릴리스 형식을 따른다.
   - 제목: `v0.2.0: 번들 코드 구조 관례`
   - 본문 섹션(`v0.1.0`과 같은 이름·순서): `## 무엇이 들어 있나`(새 `## Code the skill bundles` 절 요약 — 트리는 인터페이스, `cli.py` + 패키지 하나, 하위 패키지 기준 셋, `uv run` + PEP 723, 출력 관례, 테스트·구조 테스트는 스킬 폴더 밖, 기존 스킬은 합의 후 이행) / `## 설치`(v0.1.0과 같은 심링크, 이미 설치했으면 `git pull`만) / `## 알려진 한계`(새 본문이 실제로 관례대로 된 코드를 낳는지는 사후 재실행 없이 codex 리뷰로만 확인, 기존 스킬들은 아직 이행 전).
   - 명령: 본문을 스크래치패드 파일로 쓰고 `gh release create v0.2.0 --target main --title "v0.2.0: 번들 코드 구조 관례" --notes-file <파일>`.
   - 완료 판정: `gh release view v0.2.0`이 Latest이고 태그가 3단계의 머지 커밋을 가리킨다(`git rev-parse v0.2.0^{commit}` = `origin/main`).

## Verification

- 구조: `claude plugin validate --strict "/Users/seongjin/Coding/Skill Maker/.claude/skills"` → exit 0.
- 문서 품질: codex 리뷰 최신 라운드 execution 0건.
- 배포: `v0.2.0` 릴리스가 Latest이고 머지 커밋을 가리킨다.
- 검증되지 않는 것: 새 본문이 실제로 관례대로 된 코드를 낳는지(사후 재실행 없음, C12)와, 실제 인터뷰 세션에서 관례가 사용자 요구와 부딪히는 경우의 처리. 기존 스킬 이행(C11) 때 관찰한다.
