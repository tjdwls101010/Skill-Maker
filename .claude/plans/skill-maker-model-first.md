# skill-maker: 모델의 판단을 좁히지 않는 스킬 (v0.6.0 계획)

> 계획 모드에서는 지정 경로(`skill-maker-nested-globe.md`)에만 쓸 수 있다. 승인 즉시 `.claude/plans/skill-maker-model-first.md`로 이름을 바꾼다. 다음 세션이 바뀐 이름으로 시작하지 않았으면 구현 0단계에서 바꾼다.

## Context

- skill-maker(v0.5.0, SKILL.md 한 파일, 3,937단어, 공개 레포 `tjdwls101010/Skill-Maker`)를 쓴 세션에서 성진이 같은 이의를 반복했다.
  - 역량 과잉: 네이티브 도구로 충분한 일에 프로그램 역량을 더해 Claude를 좁힌다.
  - 상황별 처방: 어느 상황에 어떻게 행동할지를 일일이 정의한다.
- 뿌리는 같다. 실제 상황을 보는 실행 시점의 Claude에게서 판단을 빼앗아, 상황을 상상만 하는 작성 시점으로 옮긴다.
- 성공 기준은 품질 하나이고, 전면 재작성도 허용됐다. 실측 결과 제자리 개정이 맞다(H8).

## 진단

### 성진의 이의 (원문, 2026-10-25~11-05, 6개 프로젝트)
- documents: "기능은 포괄적으로 담되, 각각의 상황에 어떻게 행동할지까지 일일이 다 정의하는건 레일 아닌가?"
- design: "스킬에 모드를 두는건 레일 아닌가? 어떤 경우에 아떻게 행동하라고 다 정의할거야?"
- politics: "이걸 일일이 어떤걸 `ultra-search`를 활용해야 한다고 나열할거야??"
- ultra-search: "그 대안이 뭔지까지 다 정의하려고? 그럼 레일아닌가?", "목적 중립적으로 구성해야 하잖아 … 왜 이런점을 너가 놓치는 것처럼 느껴지지?"
- video: "이거도 굳이 정의를 해야하나? 레일처럼?", "굳이 점검 항목을 스킬에 정의하겠다고?", 역량 기준 발언(`--sheet` 철회로 이어짐)

### v0.5.0에서 이것을 만드는 문장
| 문장 | 결과 |
|---|---|
| Interface over document: "when to use which" | 사용 상황을 쓰라고 직접 지시 |
| Writing judgment down: 원칙은 조건·뒤집는 조건·사례를 갖는다 | 원칙이 경우의 수 목록으로 불어남 |
| Your phrasing: 새 사례를 적용해 묻는다 | "새 사례입니다… Claude가 어떻게 해야 할까요?" |
| When the skill is done: 모든 차이는 자리나 뺀 이유를 갖는다 | 차이마다 줄이나 기능을 찾음 |
| Colleague / Writing judgment down: 먼저 합의, 무인이면 정한다 | "이 답이 스킬에 '먼저 합의' 줄을 넣을지 정합니다" |
| 명령 자격이 코드 절 불릿 하나에 묻힘 | "반복해서 썼다"를 "도구가 필요하다"로 오독 |

## 결정 장부

| # | 결정 | 소유자 | 근거·영향 |
|---|---|---|---|
| H1 | 경계는 '상황 vs 범용'이 아니라 '사실 vs 처방'이다. 구체적 함정도 사실이면 남기고, 사실로 도달할 행동은 빼며 대안 목록·모드·점검표도 쓰지 않는다 | 성진 | 질문 1 |
| H2 | 처방은 증거가 있을 때만: 사실만 줘도 틀리는 것이 관찰됐고 결과를 사람이 못 보거나 되돌리기 비쌀 때, 이유와 함께 | 성진 | 질문 2 |
| H3 | 포괄성: 네이티브로 닿지 못하는 통로가 자격을 얻으면 목적 중립으로 넓게 덮는다. 편의·가공 기능은 각자 시험한다 | 성진 | 질문 3 |
| H4 | 이웃 스킬 문단(`skill-doctor`, 양쪽 description)을 없애고 한 문장으로 교체한다. 라우팅은 사용자 CLAUDE.md 몫이다. 실측 O12에서 이 문장이 일을 하므로(이름 없이 범위만 말함) 유지한다 | 성진 | 질문 4. "스킬은 다른 스킬을 의식하지 않고 독립적으로 완결적으로" |
| H5 | 코드 절은 SKILL.md 안에 두고 역량 시험 뒤에 온다. 레퍼런스로 나누지 않는다 | 성진 | 질문 5 (Claude 추천안인 분리를 고르지 않음) |
| H6 | 역량 시험은 여섯 번째 속성 **Model over machinery**다 | 성진 | 질문 6 |
| H7 | Colleague over autopilot은 유지하되 네 항목을 사실로 담게 하고, 생성 스킬이 합의·무인·인계 틀을 복사하지 않게 한다 | 성진 | 질문 7 |
| H8 | 제자리 개정 + 보존 대조표 + 실측 통과 | 성진 | 질문 8. v0.5.0이 이미 스킬 없음보다 크게 낫다 |
| H9 | '누가 쓰나, 사람이 있나, 위치, 언어'는 요청과 프로젝트에서 읽고 가정을 한 줄로 말한다(기본: 저자, 대화 중). 단서가 있을 때만 묻는다 | 성진 | 질문 9. politics에서 "이게 무슨 뜻인지 모르겠네" |
| H10 | H2의 예외: 예약 실행이 확실한 스킬은 요건을 깨거나 되돌릴 수 없는 경우의 행동을 관찰 없이 미리 정한다. 대화형 스킬에는 무인 분기를 넣지 않는다 | 성진 | 질문 10 |
| H11 | 이미 만든 스킬의 재감사와 수정은 범위 밖이다. 다음에 손볼 때 새 기준을 따른다 | 성진 | 질문 11 |
| H12 | 경계 시험 3건의 판정이 예측과 모두 같다. (A) news의 `record` 전 절차 → 사실만 남긴다. (B) politics의 "동의는 유예한다", news의 URL 형식 → 저자 전용 스킬에서는 남긴다(사람이 정한 것은 사실). (C) ultra-search의 search/fetch → 차이는 남기고 '언제 어느 것을', 사용 순서는 뺀다 | 성진 | 질문 12~14. (B)가 'What the people the skill serves have settled…' 문장을 낳았다 |
| H13 | 본문 길이는 검증한 문구 그대로 간다(4,480단어, v0.5.0보다 543단어, +14%). 감량은 실사용 뒤 다음 개정에서 본다 | 성진 | 질문 15 |
| H14 | 계획 파일명은 `skill-maker-model-first.md`다. 릴리스는 `v0.6.0`이다 | Claude | 이전 계획의 `skill-maker-<주제>.md` 꼴, 지난 릴리스마다 마이너 올림(`v0.1.0`~`v0.5.0`) |

## 실측

과거 세션에서 성진이 판정한 사례로 틀을 만들었다. 격리 실행(`claude -p --safe-mode --restricted --tools ""`, Opus 5.5 high), 사례당 2회. 28사례: 과잉 판정 O 16, 대조 C 9, 질문 생성 G 3. 개정 문구를 쓸 때 쓰지 않은 사례를 남겨 뒀다. 자료는 `bench/`(아래 0단계 참조).

### 스킬 없음 / v0.5.0 / 최종판
- 스킬 없음 대비 v0.5.0: O 사례 실행당 명령 0.93 → 0.14, 줄 4.4 → 2.1. **그래서 전면 재작성이 아니라 제자리 개정이다.**
- v0.5.0은 실제 실패를 재현한다: `--sheet` 2/2, `frames --captions` 2/2, G2의 `verify-render`·`check-subs` 2/2.
- 틀 복사: '이미 정했거나 맡긴 경우가 아니면'(실행당 0.19, 스킬 없음 0.05), '물을 사람이 없으면'(0.19, 스킬 없음 0). 입력에 무인 실행 언급이 없었다.

### 최종판 가림 채점 (v0.5.0 → 최종판, 조건을 가리고 Codex와 Claude가 채점)
28사례, 조건당 56회. 최종판은 SHA `7baf7544…`의 문구다.

| 지표 | Codex 채점 | Claude 채점 |
|---|---|---|
| 부적절한 명령 | 16 → **0** | 16 → **0** |
| 상황별 처방 줄 | 101 → **23** | 99 → **19** |
| 사실 줄 | 5 → **36** | 17 → **44** |
| 불필요한 줄 | 101 → **28** | 89 → **17** |
| "Claude가 어떻게 해야 하나요" 질문 | 12 → **4** | 10 → **4** |
| 판정 일치 / 부분 / 불일치 | 11/24/21 → **32/23/1** | 13/19/24 → **38/16/2** |

- 남겨 둔 과잉 사례(6건 12회)의 불일치는 두 채점 모두 3 → 0이고, 일치는 4 → 8(Codex), 4 → 10(Claude)이다. 다만 이 사례들은 c2 이후 리뷰 문장에 간접적으로 영향을 줬고, P1·P2·U1·U2는 작성 도중 더한 사례라 완전한 홀드아웃은 아니다.
- 두 채점자의 판정 일치는 112건 중 89건이다.
- '필요 역량 누락' 2건은 양쪽 조건에서 같고, C1·C2의 도구 이름과 범위가 판정 문구와 달라 표시된 것으로 읽었다. 실제 도구는 양쪽 다 제안됐다.
- 개별 재현: `--sheet`·`frames --captions`·`verify-render`·`check-subs`·`apply-cuts`·`unrecord`·`status`/`resume` 제안이 사라졌고, 필요한 C1·C2의 도구는 2/2로 유지됐다. U1은 예약 실행의 멈춤을 쓰고 U2는 분기 없이 사실만 쓴다(H10). P1은 사람이 정한 형식을 담고 P2는 공개 스킬에 취향을 넣지 않는다(H12 B).

### 런타임 시험: 사실만 줘도 옳게 행동하는가
10상황 × 3회에서 사실만 준 조건이 모두 합리적이었다(Claude 판정, Claude 리뷰어가 파일을 직접 읽고 동의).
- OCR: "대충 봐줘"에서는 돌리지 않고, "감사 답변서라 끝까지"에서는 묻지 않고 돌린다. 처방 조건은 한 가지 행동만 따른다.
- 전사 엔진, `record`, 체크포인트, WebFetch 대체, `cuts:0`, 넘길 때 한계 말하기: 모두 사실만으로 충분했다.
- ultra-search의 "문구가 없으면 14회 중 12회 네이티브" 측정은 문구 없음과의 비교이고 사실만 준 경우와의 비교가 아니다. 사실만 줬을 때 WebFetch로 넘어간 경우는 0/3이다.

### 검증되지 않는 것
- 모든 실험이 한 번의 응답만 본다. 긴 대화에서 사실이 멀리 있을 때 같은지는 모른다.
- 정답지(ruling)는 Claude가 세션 기록에서 옮겨 적었고, 런타임 시험의 good 기준도 Claude가 썼다.
- 사례당 2회라 1/2 갈림은 약한 증거다. 완료 점검 단계(6절)는 문구로만 확인했다.
- Opus 5.5 기준이다. 다른 모델에서는 사실 충분성이 다를 수 있다.

## SKILL.md 개정 후 섹션 구조

SKILL.md 한 파일이다. `references/`는 만들지 않는다(H5). frontmatter는 바꾸지 않는다. 본문은 영어, 한 덩어리 한 줄, `${` 형태를 쓰지 않는다.

1. `# Turning a user's expertise into a skill`: P1~P4. P2 마지막 문장만 수정.
2. `## What earns a place in a skill` (여섯 속성)
   - Principle over rail: 수정 (사실 vs 처방, 사람이 정한 것, 처방의 자격)
   - Interface over document: 수정 ("when to use which" 삭제, 호출 전 알아야 할 사실)
   - **Model over machinery**: 신규
   - For the model, not the maintainer / Dense: 그대로
   - Colleague over autopilot: 수정 (항목을 사실로, 틀 복사 금지)
   - 기본값 교정 줄 문단, 스킬이 필요 없는 경우 문단: 그대로
   - 이웃 스킬 문단: 한 문장으로 교체
3. `## Drawing out what the user knows`
   - 첫 단락, Read before you ask, Keep kinds of authority apart, When the user knows the goal, Probe with specifics, Starting from an existing skill: 그대로
   - Contrast with the default: "Give the run…" 단락 끝에 두 문장 추가
   - Your phrasing: 한 문장 추가
   - Agree on what you cannot establish: 한 문장 추가, 마지막 문장 교체
4. `## Writing judgment down`: 첫 문단 수정, 완료 기준 문단 수정, Put knowledge 문단 축약. 예시·파일 분할 문단은 그대로.
5. `## Code the skill bundles`: 맨 앞 한 문장 추가, 불릿 네 곳 수정. 트리와 나머지는 그대로.
6. `## When the skill is done`: 항목 세 곳 수정.

## 개정 문구 (Codex·Claude 리뷰 반영 최종판)

표시가 없는 문장은 v0.5.0 그대로다. 아래 수정을 적용한 결과의 SHA-256은 `7baf7544bb22603c912dd5bc2f8397cc0df3f1902b393a245a7b89e956ae9c63`이어야 한다. 검증에 쓴 파일 `bench/SKILL.cand7.md`가 남아 있으면 그것과 같아야 한다.

**1절 P2 마지막 문장**
> Where a skill is known to run with nobody there to redirect the model, as in a scheduled job, it has to settle more in advance.

**2절 첫 문단:** "Five properties" → "Six properties"

**Principle over rail** (뒤 문장 'Write a fixed sequence…'를 아래로 교체)
> A rail with a condition is still a rail: "before a restore, confirm unless the user already said to proceed" decides the cases its author imagined, while "a restore overwrites the live database and takes an hour to reverse" lets the model tell a rehearsal from an outage. So write what the model cannot see, however specific, including a remedy it could not find, such as a parameter's name, and leave out what it would do once it sees it; modes, fallback lists and per-case checklists fall to the same test. What the people the skill serves have settled, such as an output format or how they work, is a fact of this kind. Outside what a known scheduled run settles in advance, write an action, a fixed sequence or a prohibition only when a model given the fact has been seen going wrong where the person would not notice or could not undo it, with the reason, and never stronger than that reason requires.

**Interface over document 전문**
> - **Interface over document.** What a tool owns (its inputs, outputs and failure modes) lives in the tool: help text, closed choices, schema, error messages. A prose copy is a second thing that can be wrong, while what the interface of a tool someone else owns does not say stays in the skill. The skill keeps what no single interface can say: how the options differ and where each stops being reliable, choosing among them being the model's, and what must shape the plan before the call, such as an effect that cannot be undone or a cost that grows with the input, because `--help` is read at the call.

**Model over machinery** (Interface over document 다음에 신규)
> - **Model over machinery.** Reading, writing code and running a shell are what the model does best, and a command replaces them with what its author foresaw: a search command over a database the model could query offers only its author's filters and the skill then teaches workarounds, where a schema it can read lets it write the query itself. A command earns its place by reach the model's own tools lack, such as a logged-in session or a format they silently misread, or by hiding work that goes the same way every time and that those tools do clearly slower, too large to read or wrong, such as measuring text in a font or a write that must keep several things consistent. Doing something repeatedly is not that: scripts that each came out right, in shapes that differed, are the model working. Reach, once earned, covers the system purpose-neutrally through as few general commands as it takes, not one task's needs; each thing done with the result, such as a conversion or a check, passes this test on its own. A silent failure alone earns no wrapper: where the result shows it once the model knows where to look, a line does it; otherwise the tests above decide.

**Colleague over autopilot** ('So the skill names…'부터 'and decides the rest.'까지 교체, 앞뒤는 그대로)
> So the skill names whichever of these the domain has, as facts the model can weigh: where the model has been seen to misjudge and the evidence to check; which choices are the person's; what is costly to undo and why; and what the result does not show. Knowing them, the model acts on its call, says why in a line and follows theirs, gets agreement before what the person would not want done without it, says at handover what the result hid, and otherwise asks only for what the work cannot show and the person alone can supply, when a wrong guess would cost more than the question. A skill used only in conversation does not restate this: "ask first unless they have decided", "if nobody can be asked" and "say so at handover" are this paragraph copied in.

**이웃 스킬 문단** ('A new skill also costs…' 문단 전체 교체)
> A skill's description says what it covers and what it does not without naming another skill, because which skill goes first is set in the user's own instructions; a skill that cannot do its job without another's body should be one with it.

**Contrast** ('…already knows to raise.' 뒤에 추가)
> A difference is not yet a finding: a run the user accepted, though it did the work another way, shows nothing the skill needs, and a one-off the tool's error corrects costs a turn, not a line, while a rule of the tool that every session would trip over earns its line from one run. To see whether a fact is enough, run the contrast again with the fact given, simulating the response where a failure would land on something real.

**Your phrasing** ('…boundary conditions come from.' 뒤에 추가)
> What the case yields is the boundary and the fact that moved the decision, not its answer as a rule.

**Agree** ('…into every session.' 뒤에 추가)
> So a problem the contrast shows goes to the user as the result, asking what they make of it and why, not how the model should handle it.

**Agree 마지막 문장 교체**
> Who will use the skill and whether anyone will be there when it runs decide how much it can settle, and where it will live (project, user or plugin) decides whether it must be self-contained; these and its language are usually plain from the request and the project, so read them, say your assumption in a line (by default the author, in conversation) and ask only when a sign points elsewhere.

**Writing judgment down 첫 문단 전문**
> A principle carries its reason. The conditions under which it holds, what overturns it and a case where it decides between two similar-looking options join it only where the model would otherwise get it wrong. It need not decide every close call when someone can redirect the model: at an unclear edge the model chooses and says why. Redirecting is cheap only while undoing is, so where a choice in the domain commits much work or is hard to undo in a way the model would not see, the skill says what it commits. In runs a skill is known to make on a schedule, with nobody there, no one can catch a wrong call, so it settles in advance what the model does when a case could break a requirement or do what cannot be undone, such as stopping or flagging it, without waiting for a first failure. A slogan such as "use judgment" or "keep it user-centred" justifies opposite actions and is not a principle.

**완료 기준 문단 앞 두 문장 교체**
> Where a result can look finished while failing it, say what must hold before the model hands work over, in terms it can check; whether the work is finished is often the person's call. Name, as a fact, what a result in this domain does not show the person, such as a check no tool can make or the kind of thing that gets left out.

**Put knowledge 문단 전문** (v0.5.0의 데이터베이스 예시는 Model over machinery로 옮겼다)
> Put knowledge where it can act. Settled calculations and verification contracts belong in an interface such as a rubric, template or schema, or a script once it passes Model over machinery; judgment that turns on context does not become a fixed branch. A command that only passes a query through adds code and a second copy of the schema, and nothing the model lacks.

**Code the skill bundles**
- 첫 문단 맨 앞에 추가: "Code is here only once it passes Model over machinery; a skill with none has no `scripts/`."
- "Every skill is called as" → "Every skill with code is called as"
- 불릿 "The model is the caller of every command…" 삭제 (내용은 Model over machinery로 옮겼다)
- cli.py 불릿 끝: "any selection, fallback, retry or follow-up call a command has earned is domain logic and belongs to the unit it calls."
- "A tool the model itself must run becomes a subcommand." → "A tool the model itself must run, once it has earned its place, is a subcommand."

**When the skill is done**
- "The judgments the skill carries are written, with the boundaries and conflicts the model would otherwise misjudge resolved, and what remains uncertain is stated."
- "Every objection the user held to and every mistake the model would not have caught has a place in the skill or a recorded reason for leaving it out. Each action line passes the test in Principle over rail or is what a known scheduled run settles in advance, and each command passes the test in Model over machinery."
- "five properties" → "six properties"

### 보존 대조표 (v0.5.0에서 빠지거나 약해진 것)

| v0.5.0 | 처리 | 이유 |
|---|---|---|
| "when to use which" | 삭제 | H1, H12(C) |
| `skill-doctor`, 양쪽 description 경계, 리스팅 경쟁 | 삭제 | H4 |
| "gets agreement first unless …", "so the handover says it" (항목에 붙은 행동) | 항목에서 행동을 떼어 모델의 행동 서술로 옮김 | H7 |
| "the model gets agreement on that choice before committing unless …" | 삭제, "the skill says what it commits" | H7 |
| "where nobody will be there, the skill settles …" | "In runs a skill is known to make on a schedule …"로 좁히고 넓힘(되돌릴 수 없는 일 포함) | H10 |
| 원칙의 네 구성요소 의무 | 이유만 의무, 나머지는 모델이 틀릴 곳에만 | H1 |
| "Every difference the contrast surfaced" | "Every objection the user held to and every mistake the model would not have caught" | 리뷰. 작동하지만 전문가가 거부하는 지식을 잃지 않게 |
| "major conflicts between principles are resolved" | "the boundaries and conflicts the model would otherwise misjudge resolved" | 끝에서 경우의 수를 되살리지 않게 |
| 불릿 "The model is the caller of every command" | Model over machinery로 이동 | H6 |
| 누가 쓰나·위치·언어의 필수 질문 | 읽고 가정을 말함, 단서가 있을 때만 질문 | H9 |

### 리뷰 처리

- **1라운드(Codex 8건, Claude 12건 execution):** 반영. 주요 항목: `--help`로 밀려난 함정(C6 회귀), 과소 구축('stays with the model'), 사용자 이의 장부 복구, 인계 틀, 코드 절 자격 조건, 110·111줄 표류, H4 병합 문장 좁힘. 기각: "사실 충분성이 검증되지 않았다"(런타임 시험을 리뷰어가 못 봤다. 2라운드에서 리뷰어가 파일을 읽고 철회했다).
- **2라운드(Codex 1건, Claude 3건):** 반영. 목적 중립 복원, 대조 결과를 결과로 묻기, H10 연결.
- **3라운드(Codex 0건, Claude 1건):** 반영. 도구 규칙의 사실 줄과 남의 도구.
- **4라운드(두 리뷰어 0건).** 3라운드 반영 후 cand6 확인.
- **5라운드(두 리뷰어 0건).** 'What the people the skill serves have settled…' 한 문장을 더한 최종판 확인. 공개 스킬에 취향이 들어가는 경로와 Keep kinds of authority apart 문단과의 충돌을 봤다. '우리 팀'이라 말하지만 실제로는 한 사람의 취향인 중간 사례는 측정하지 않았다.

## 구현 단계 (다음 세션)

파일을 바꾸는 단계가 3개 이상이므로 시작할 때 `TaskCreate` 트래커를 열고, 아래 완료 판정을 description에 적는다. 바꾸는 것은 SKILL.md 한 파일이고 코드가 없어 TDD 단계는 없다.

0. **준비**
   - 이 파일이 아직 `skill-maker-nested-globe.md`이면 `.claude/plans/skill-maker-model-first.md`로 바꾼다.
   - `main`에서 `feat/skill-maker-model-first` 브랜치를 판다. 첫 커밋에는 계획 파일만 넣는다.
   - 관련 없는 작업 트리 변경은 커밋하지 않는다(`.claude/plans/user-inputs/…` 삭제, `.claude/histories`). `git add`에는 경로를 지정한다.
   - 실측 자료가 남아 있으면 보존한다: `/private/tmp/claude-501/-Users-seongjin-Coding-Skill-Maker/b648863d-6e40-44fb-b0b2-f77f6612149c/scratchpad/bench`를 `.tmp/measure-v0.6/`으로 복사한다(`.tmp/`는 gitignore다). 사라졌으면 그 사실을 PR 본문에 적는다. 문구는 이 계획에 다 있으므로 구현을 막지 않는다.
   - 완료 판정: 브랜치가 있고, 첫 커밋에 계획 파일만 있다.
1. **SKILL.md 개정**
   - 위 "개정 문구"를 그대로 적용한다. 문구를 다듬지 않는다(H13).
   - 완료 판정:
     - `shasum -a 256 "/Users/seongjin/Coding/Skill Maker/.claude/skills/skill-maker/SKILL.md"`가 위 해시와 같다.
     - `git diff`가 개정 문구와 보존 대조표에 있는 곳만 바꾼다.
     - `grep -c '\${' <SKILL.md>`가 `0`을 출력한다(매치가 없으면 grep 종료코드는 1이고 이것이 기대값이다).
     - `claude plugin validate --strict "/Users/seongjin/Coding/Skill Maker/.claude/skills"`가 exit 0이다.
2. **전문 승인**
   - 성진에게 보여 줄 것: 개정 후 SKILL.md 전문, v0.5.0 대비 diff, 바뀐 문장의 한국어 요약, 실측 표와 한계.
   - AskUserQuestion으로 승인받는다. 부분 승인이나 침묵은 승인이 아니다.
   - 성진이 문구를 고치면 고친 문장에 대해 실측 틀과 Codex·Claude 리뷰를 다시 돌린다. 틀이 사라졌으면 Codex·Claude 리뷰만 돌리고 실측이 없다고 말한다.
   - 완료 판정: 성진이 전문을 명시적으로 승인했다.
3. **PR과 머지**
   - 논리 단위로 커밋하고 푸시한 뒤 PR을 연다. `gh pr merge --squash`로 머지한다.
   - 제목: `feat: skill-maker가 모델의 판단을 좁히지 않게 한다`
   - 레포에 PR 템플릿이 없으므로 본문은 `## 무엇을 바꿨나` / `## 왜` / `## 영향` / `## 검증` 순서다.
   - `## 왜`에 담을 것(개발 기록이므로 스킬 밖에 둔다): 성진의 이의 원문, 생성 문장 진단, 측정 모델(Opus 5.5)과 조건, 기각한 대안(전면 재작성, 코드 절 레퍼런스 분리, 사실만 절대 규칙, 문단 전체 삭제), 리뷰 처리와 기각 사유.
   - `## 검증`에는 실제로 돌린 명령과 수치를 그대로 적는다.
   - 완료 판정: PR이 머지됐고, 심링크인 `~/.claude/skills/skill-maker/SKILL.md`가 `main`의 파일과 `diff` 없이 같다.
4. **GitHub 릴리스**
   - 머지 커밋에 `v0.6.0` 태그를 붙인다. 제목: `v0.6.0: 모델의 판단을 좁히지 않는 스킬`
   - 본문 섹션은 v0.5.0과 같은 이름·순서다: 첫 줄 요약, `## 무엇이 들어 있나`(바뀐 판단마다 한 항목), `## 설치`(v0.5.0과 같다), `## 알려진 한계`(위 '검증되지 않는 것', 기존 스킬은 아직 새 기준이 아니라는 것, 본문이 14% 길어진 것).
   - 명령: 본문을 스크래치패드 파일로 쓰고 `gh release create v0.6.0 --target main --title "<제목>" --notes-file <파일>`.
   - 완료 판정: `gh release view v0.6.0`이 Latest이고, `git rev-parse v0.6.0^{commit}`이 `origin/main`과 같다.

## Verification

- **문구 동일성:** SKILL.md SHA-256이 위 해시와 같다. 그 파일이 실측과 리뷰를 거친 문구다.
- **구조:** `claude plugin validate --strict "/Users/seongjin/Coding/Skill Maker/.claude/skills"`가 exit 0, `${` grep 출력 0.
- **보존:** diff의 삭제·약화가 모두 보존 대조표에 있다.
- **이미 끝난 실측과 리뷰:** 이 계획 세션에서 끝났다. 위 수치를 그대로 인용한다.
- **승인:** 성진이 전문을 승인했다.
- **배포:** `v0.6.0`이 Latest이고 머지 커밋을 가리킨다.
- **실사용에서 볼 것:** 다음 skill-maker 세션에서 성진이 "이건 레일 아닌가?"를 몇 번 말하는지, 사실 줄만으로 긴 대화에서 모델이 멈춰야 할 곳에서 멈추는지, 필요한 도구까지 빠지지 않는지.
