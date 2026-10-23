# skill-maker: 동료 프레임(Colleague over autopilot) 계획

> 계획 모드에서는 지정 경로(`claude-skills-skill-maker-tingly-pudding.md`)에만 쓸 수 있다. 승인 즉시 `.claude/plans/skill-maker-colleague-frame.md`로 이름을 바꾼다. 다음 세션이 바뀐 이름으로 시작하지 않았으면 구현 0단계에서 바꾼다. v0.4.0 때는 "승인 직후 바꾼다"고 적었지만 구현 세션이 옛 이름(`…tidy-hummingbird.md`)으로 시작했다.

## Context

- skill-maker(v0.4.0, SKILL.md 한 파일, 공개 레포 `tjdwls101010/Skill-Maker`)는 사용자의 전문성을 스킬로 옮긴다. 성진은 이 스킬을 잘 쓰고 있다.
- 성진의 요청(2026-10-23): 스킬을 쓰는 Claude가 동료처럼 일하게 하는 관점을 PoR 안의 한 문장이 아니라 이름 붙은 프레임으로 세운다. 새로 더한 요소는 셋이다.
  - 사람이 문제를 정하고 실행하고 책임지는 주체라는 것
  - 모델이 스스로 틀릴 수 있다는 메타인지
  - 스스로 판단할 것과 판단하지 말 것의 구분(매 단계 승인은 아님)
- 이 요청은 v0.4.0 계획의 K4("누구 몫인가는 다섯 번째 원칙이 아니라 PoR 안의 한 문장")를 다시 여는 것이다.
- 성공 기준은 품질 하나다.

## 증거

### v0.4.0에 이미 있는 프레임의 재료 (SKILL.md)
- 도입 P2: 모델은 보통 혼자 일하지 않는다. 스킬은 동료가 가져오는 것이지 혼자 끝내는 대본이 아니다. 무인 실행이면 더 정한다.
- PoR: 레일은 형식이 아니라 기능으로 판정한다(사람 몫의 취향·방법을 정해 버리는 줄).
- 3절:
  - 취향의 권위(개인 스킬 vs 공유 스킬)
  - 비전문가 저자 단락
  - Phrasing의 취향 경계
  - Agree 첫머리: 스킬이 담을 것 vs 쓰는 사람이 그 자리에서 정할 것
  - 누가 쓰나, 실행 때 사람이 있나
- 4절:
  - close call을 다 정하지 않아도 된다. 되돌리기 비싸면 먼저 합의하고, 무인이면 멈추거나 표시한다.
  - 완료 기준은 "넘기기 전 성립할 것"이고, 끝났는지는 사람이 판정한다.

### v0.4.0 이후 만든 스킬에서 이미 작동하는 곳
- `claude artist/.claude/skills/design` (10/17, 성진 전용)
  - "Claude는 판단을 끝까지 하고, 판정은 성진의 눈이 한다", "추천은 가설로"
  - 결과 직후 선택지를 띄우지 않는다.
  - 불만을 금지로 옮길 때는 해석을 한 줄로 확인한다.
  - 도메인 맹점: "프롬프트를 쓰는 Claude도 같은 쪽으로 끌려가 … Claude 자신의 초안도 출처 시험에 건다"
- `Agentic Video/.claude/skills/video` (10/15): "Cheap to change … make the call, show it", "Costly to undo … ask first", "what you could not verify. You cannot hear the cut"
- `Agentic SNS/.claude/skills/threads`: 실계정이므로 기본 예산을 넘는 읽기는 비용을 말하고 먼저 합의한다. 아무도 없으면 기본 예산 안에서만 읽는다.
- `Agentic Invest/.claude/skills/invest` (10/19): 결과물이 숨기는 것 — "what the numbers cannot show … carry it into the answer"
- 대비: `Agentic SNS/.claude/skills/naver-blog`(프레임 이전)에는 사람에게 가져갈 지점이 없다.

### 모델 기본값 (`.tmp/references/Prompting Claude Opus 5.5.md`)
- "tends to get to work quickly": 둘러보기 전에 착수한다.
- 원치 않는 멈춤의 예: "a list of decisions for the user when, by your own account, none of them blocks the rest of the work"
- 이 문서에서 "human-in-the-loop applications"는 '응답할 사람이 있는 앱'을 뜻한다.

## 결정 장부

| # | 결정 | 소유자 | 근거·영향 |
|---|---|---|---|
| H1 | 의도: 생성 스킬의 모델은 판단을 끝까지 하되, 사람을 일의 주인·책임자로 대하고, 자기가 틀릴 수 있는 곳을 알고, 스스로 정할 것과 사람에게 가져갈 것을 가른다(매 단계 승인 아님). 이것을 이름 붙은 프레임으로 세운다. K4를 다시 연다 | 성진 | 질문 1 "맞다" |
| H2 | 진단: 재료 대부분은 v0.4.0에 흩어져 있고 이후 스킬에서 작동한다. 빠진 것은 이름과 한 자리(리뷰 기준), 책임(결과물이 숨기는 것), 메타인지(도메인 맹점)다. 스킬의 본업인 역량 전달을 희석하지 않는다. K7(일반 협업 절 금지)을 지켜 양방향(가져갈 것 + 묻지 않고 할 것)으로 쓴다 | 성진 | 질문 1 "맞다" |
| H3 | 메타인지는 도메인 맹점으로 담는다. 틀리는 기제 셋(초기 해석이 닻이 됨, 새 증거 없는 재검토는 재확인일 뿐, 볼 수 없는 것은 확인 불가)은 skill-maker에 이유로 한 절만 둔다. 생성 스킬에는 그 도메인의 맹점, 증거를 주는 확인 수단, 사람만 확인할 수 있는 것을 담는다. skill-maker 인터뷰에 맹점 질문을 더하고, 대조 실험에서 '자신 있게 틀린 곳'을 맹점 후보로 읽는다. 일반 경각심 문장은 넣지 않는다 | 성진 | 질문 2 "도메인 맹점으로" |
| H4 | 프레임은 2절의 다섯 번째 속성(짧은 시험)으로 둔다. 세부는 쓰이는 자리에 둔다(맹점을 끌어내는 법은 3절, 넘길 때 보일 것은 4절). 6절 교차 리뷰 기준은 '다섯 속성'이다. 도입 P2는 프레임 전체의 이유로 그대로 둔다 | 성진 | 질문 3. 다른 넷은 주로 '빼는' 시험이지만 이 속성은 '있어야 할 것'도 요구한다. PoR의 '이유를 쓰라'와 같은 결이다 |
| H5 | 실측(헤드리스 대조, 사례 적용)은 하지 않는다. 검증은 독립 문서 리뷰 + `validate --strict` + 전문 승인이다 | 성진 | 질문 4 "리뷰와 승인만". K6과 같다 |
| H6 | 독립 검토는 Sonnet 서브에이전트(`Agent`, `model: "sonnet"`)가 맡는다. codex는 쓰지 않는다 | 성진 | 질문 5 "sonnet으로 해". skill-maker 완료 기준은 다른 모델 계열의 리뷰를 요구한다. 같은 계열이라 같은 맹점을 공유할 위험을 PR 본문과 릴리스 '알려진 한계'에 적는다 |
| H7 | 개정된 skill-maker 자신도 다섯 속성을 지킨다. 원칙과 그 이유를 한 덩어리로 쓰고, 모델이 행동을 바꾸지 않는 줄은 빼고, 출처(Opus 5.5 가이드, 연구, 스킬 제작 기록)는 본문에 남기지 않는다. 새 속성은 skill-maker 자신에게도 걸리므로 리뷰는 개정 본문 전체를 다섯 속성으로 판정한다 | 성진 | 계획 중 지적 "skill-maker 또한 skill-maker의 프레임을 철저히 반영" |
| H8 | 이름은 **Colleague over autopilot**이다. 'human in the loop'은 쓰지 않는다 | 성진 | 질문 6. 'X over Y' 꼴이고, 도입 P2의 "a capable colleague … not a script for finishing it alone"과 이어진다. HITL은 '승인 관문', '응답할 사람이 있음'으로 읽혀 과잉 확인 쪽으로 기운다 |
| H9 | PoR의 '기능 판정' 문장을 한 글자도 바꾸지 않고 새 속성으로 옮긴다(조건 "when neither they nor a convention they work under has settled it" 포함). PoR은 v0.3.0의 범위로 돌아간다 | 성진 | 질문 7. 옮긴 문장의 "rail"이라는 말이 두 속성을 잇는다. 1차 리뷰 B: 바꿔 쓰면 'method', 'narrows the work'가 사라진다 |
| H10 | 제자리 개정 + 보존 대조표로 고친다. frontmatter는 손대지 않는다. 5절(코드)은 H15의 마지막 문단 수정만 한다 | 성진 | 질문 8. K5와 같다. v0.3.0 통째 교체 초안이 기존 이유 다섯 가지를 놓친 적이 있다 |
| H11 | 머지 후 `v0.5.0`을 릴리스한다. 제목과 본문은 v0.4.0 형식(요약 / `## 무엇이 들어 있나` / `## 설치` / `## 알려진 한계`)을 따른다 | 성진 | 질문 9 |
| H12 | 계획 파일명은 `skill-maker-colleague-frame.md`다. 승인 즉시 바꾼다 | Claude (계획 모드 제약) | 이전 계획들과 같은 `skill-maker-<주제>.md` 꼴 |
| H13 | 경계 시험 3건에서 새 속성의 예측 판정이 성진의 판정과 모두 같았다. 속성 문장은 이 판정을 내도록 유지한다. (A) video "You edit together with the user … outranks anything in this skill" → 줄 전체 삭제: 앞 절은 일반 협업 문장, 뒷절은 모델 기본 동작. (B) video "Say what changed, the calls you made … You cannot hear the cut …" → 앞 문장 삭제(일반 보고는 기본값), 뒷문장 유지(결과물이 숨기는 것). (C) design "결과 직후 선택지를 띄우지 않는다" → 저자 전용 스킬에선 유지, 공유 스킬이라면 삭제(각자의 방식) | 성진 | 질문 10~12 "예측과 같다". 책임 문장은 도메인에서 결과물이 숨기는 것만 담고, 일반 보고는 기본값에 맡긴다. 시험은 리뷰 전 초안으로 했지만, 1·2차 리뷰 반영본에서도 같은 판정이 나온다: (A)는 "A line that tells the model to check in … locates nothing", (B)는 "what the result will not show them, which the handover says"와 4절 책임 문장, (C)는 바뀌지 않은 권위 단락이 낸다 |
| H14 | 이미 만든 다른 스킬(video, design, threads/sns, invest, naver-blog 등)의 재감사는 이 계획의 범위가 아니며 후속 목록도 남기지 않는다 | 성진 | 질문 13 "이건 신경쓰지 마" |
| H15 | 사용자의 프로젝트가 이미 따르는 스택·배치는 그 사람 몫이다. 그래서 이 관례를 별도의 결정으로 제안하고, 합의 전에는 그 사람의 것이 서고 구조 테스트를 적용하지 않는다. 관례가 없으면 지금처럼 이 트리를 쓴다. 자리는 5절 마지막 문단(옛 코드의 마이그레이션 제안)을 넓히는 것이다 | 성진(내용) / Claude(자리·문구, 2차 리뷰) | 질문 14 "한 절 더한다". Sonnet 리뷰 H(잠재 execution)로, 새 속성을 skill-maker 자신에게 적용한 결과다(H7). 질문 때 보인 '첫 문단 끝에 덧붙이기'는 바로 앞 문장 "The tree's value lies in being the same everywhere"와 부딪히고 구조 테스트 공백을 남긴다. 그래서 같은 장치를 가진 마지막 문단을 넓혔다. 성진의 프로젝트는 이미 이 관례를 따르므로 달라지는 것이 없다. 전문 승인(구현 3단계)에서 다시 확인한다 |

| H16 | 새 속성(계획 초안 약 250단어, 다른 넷은 55~100단어)은 구현에서 줄이는 것을 목표로 한다. 고정: PoR에서 옮긴 레일 문장(원문 그대로, H9)과 기제 셋의 내용(H3, 표현은 줄여도 된다). 줄일 수 있는 것: 항목별 행동의 표현, 4절과 겹치는 부분, 일반 줄 문장의 표현. 단, 1·2차 리뷰가 막은 실패(A 항목별 행동 없음, C 존재 요구, N1 사람만 가진 증거, N2 추측)를 다시 열면 안 되고, 필수 사례(threads L13, design L14·L22)의 판정이 유지돼야 한다 | 성진 | 질문 15 "구현에서 줄이는 것을 목표로". 성진의 지적 "밀도 높은 원칙 중심"(H7) |
| H15 확인 | 5절은 마지막 문단을 넓히는 형태로 간다 | 성진 | 질문 16 "마지막 문단을 넓힌다" |

## SKILL.md 개정 후 섹션 구조

본문은 영어로 쓰고, 한 덩어리를 한 줄로 쓴다(하드랩 없음). `${` 형태는 쓰지 않는다(skill-maker를 로드할 때 치환된다). frontmatter는 바꾸지 않는다.

1. `# Turning a user's expertise into a skill`: P1~P4 그대로. P2(동료 문단)가 프레임 전체의 이유다.
2. `## What earns a place in a skill`
   - 첫 문장 "Four properties" → "Five properties"
   - Principle over rail: '기능 판정' 문장이 빠진다(H9). 나머지는 그대로다.
   - Interface over document, For the model not the maintainer, Dense: 그대로
   - **Colleague over autopilot**: 새 항목. Dense 다음에 둔다.
   - 그 뒤 세 문단(기본값 교정 줄, 스킬이 필요 없는 경우, 이웃 스킬): 그대로
3. `## Drawing out what the user knows`
   - 첫 단락: 판단의 작동 부품 목록에 두 항목을 더한다(이 일에서 판단이 틀리는 곳과 그걸 알아채는 법, 일의 주인에게 먼저 가져가는 것).
   - Contrast with the default: "Give the run the facts and materials…" 단락 끝에 맹점 판별 문장 하나를 더한다.
   - 나머지 단락은 그대로다: Read before you ask, Keep kinds of authority apart, When the user knows the goal but not the domain, Probe with specifics, Your phrasing is not their knowledge, Agree on what you cannot establish, Starting from an existing skill.
4. `## Writing judgment down`: 완료 기준 문단에 책임 문장 하나를 더한다. 나머지는 그대로다.
5. `## Code the skill bundles`: 마지막 문단(마이그레이션 제안)을 넓힌다(H15). 첫 문단, 트리, 항목들은 그대로다.
6. `## When the skill is done`: 교차 리뷰 항목의 "four properties" → "five properties". 나머지는 그대로다.

## 개정 초안 (Sonnet 계획 리뷰 1·2차 반영)

표시: `+`는 추가, `~`는 수정, `→`는 이동이다. 표시가 없으면 원문 그대로다.

**2절 첫 문단** `~`
> Judge each line by whether the model running the skill acts differently because of it. Five properties are what that test looks like in practice.

**2절 Principle over rail**
- `→` 다음 문장을 빼서 새 속성으로 옮긴다: "A line is a rail by what it does, not by how it is phrased: one that settles a matter of taste or method for the person working with the model, when neither they nor a convention they work under has settled it, narrows the work however well its reason is stated."
- 개정 후 PoR 전문:
  > - **Principle over rail.** State the reason and the actual shape of the thing, not a ban on one action. "Never pick the invoice by date" stops one query; "a correction reissues an invoice as a new row, so the newest by date can be a voided one, and the live invoice is the highest revision" also gets the join, the report and the migration right. Write a fixed sequence or an absolute prohibition only when a failure explains why no other path is acceptable, and never stronger than that reason requires.

**2절 새 속성** `+` (Dense 다음). PoR에서 옮긴 문장은 원문 그대로다(H9). 아래는 계획 초안 판(약 250단어)이고, 구현에서 H16에 따라 줄인다.
> - **Colleague over autopilot.** The person working with the model owns the goal and answers for the result, and the model brings its full judgment; the skill carries what the model cannot find from inside the work, because an early reading carries into the steps built on it, rereading its own work without new evidence tends to repeat that reading, and what the person intends or knows outside the conversation cannot be checked from it. Where the domain has them, the skill names where the model's judgment has been seen to run wrong and the evidence that shows it, which the model looks for in the work; the choices that are the person's, where the model makes its call, says in a line why and follows theirs; those costly to undo, which it gets the person's agreement on first unless they have already decided or left it to the model; and what the result will not show them, which the handover says. What the work cannot show and only the person can supply, the model asks for when the answer would change the work; the rest it decides without asking. A line is a rail by what it does, not by how it is phrased: one that settles a matter of taste or method for the person working with the model, when neither they nor a convention they work under has settled it, narrows the work however well its reason is stated. A line that tells the model to check in or to doubt itself in general locates nothing, so it comes out as questions and hedges everywhere alike.

**3절 첫 단락** `~`
> You are after the working parts of judgment rather than a list of beliefs: what the expert notices first, what they tell apart that a newcomer lumps together, what would make them decide the other way, where judgment in this kind of work tends to go wrong and what tells them so, and what they put to whoever the work is for before going further. Where two of their principles pull against each other, the condition that settles it is part of the knowledge.

**3절 Contrast with the default** `+`: "Give the run the facts and materials…" 단락 끝, "…and run it again." 뒤에 넣는다.
> A wrong answer the run gave with confidence, with nothing in its own output to warn it, is a blind spot the skill carries with the evidence that exposes it, once the user's reason for rejecting it holds beyond this task; what the run flagged as uncertain it already knows to raise.

**4절 완료 기준 문단** `+`: 첫 문장 뒤에 넣는다.
> Say what must hold before the model hands work over, in terms it can check; whether the work is finished is often the person's call. **Name what a result in this domain does not show the person, such as a check no tool can make or the kind of thing that gets left out, so the handover says it.** Separate what must be preserved from what is free to vary, which the model and the person settle between them, so a completion criterion does not become a demand for one particular output.

**5절 마지막 문단** `~` (H15): 마이그레이션 문단을 넓힌다. 첫 문단의 "The tree's value lies in being the same everywhere…"는 그대로 둔다.
- v0.4.0: "When a change to an existing skill touches the structure of code that predates this layout, offer the migration to the user as a decision of its own; until it is agreed, the old layout stands and the structure test does not apply to it."
- 개정:
  > When a change to an existing skill touches the structure of code that predates this layout, or the user's project already works under another stack or layout, offer this layout to the user as a decision of its own; until it is agreed, theirs stands and the structure test does not apply to it.

**6절 교차 리뷰 항목** `~`: "checked it against the four properties above" → "checked it against the five properties above". 나머지는 그대로다.

### 보존 대조표 (v0.4.0 → 개정 후)

v0.4.0 문장 가운데 아래 표에 없는 것은 모두 그대로 둔다. 삭제는 없다. 이동이 하나 있다.

| v0.4.0 문장 | 처리 |
|---|---|
| 2절 "Four properties are what that test looks like in practice." | 수정: "Five properties" |
| PoR "A line is a rail by what it does, … however well its reason is stated." | 이동: 원문 그대로(한 글자도 바꾸지 않고) 새 속성의 뒤에서 둘째 문장으로 |
| (없음) | 추가: Colleague over autopilot |
| 3절 첫 단락의 판단 부품 목록 | 수정: 두 항목 추가(이 일에서 판단이 틀리는 곳과 알아채는 법, 일의 주인에게 먼저 가져가는 것). 나머지 그대로 |
| 3절 "Give the run the facts and materials…" 단락 | 추가: 끝에 맹점 판별 문장 |
| 4절 완료 기준 문단 | 추가: 책임 문장(결과물이 보여 주지 않는 것) |
| 5절 마지막 문단 "When a change to an existing skill touches the structure of code that predates this layout, offer the migration …" | 수정: 사용자 프로젝트가 이미 다른 스택·배치를 따르는 경우를 더한다(H15). 옛 코드의 경우, 'decision of its own', 합의 전 구조 테스트 비적용은 유지한다 |
| 6절 "…checked it against the four properties above…" | 수정: "five properties" |
| 6절 "…new code, and code migrated with the user's agreement, also passes the structure test." | 수정: "new code under this layout"(구현 리뷰 F2). 5절을 넓힌 뒤, 사용자 배치로 쓴 새 코드에 구조 테스트를 요구하지 않도록 |

### Sonnet 계획 리뷰 (1차, `plan-review`, read-only)

| 지적 | 등급 | 처리 |
|---|---|---|
| A. 새 속성이 꼽는 항목에 행동이 없다. 맹점과 사람 몫의 선택까지 '사람에게 가져간다'로 읽혀, 맹점마다 확인 질문을 하고 싼 취향 선택도 먼저 묻게 된다. 변하지 않은 문장(P2, 4절 "at an unclear edge the model chooses and says why"), video·design과도 어긋난다 | execution | 반영: 항목마다 행동을 붙였다. 맹점은 증거로 확인하고, 사람 몫의 선택은 정하고 이유를 말하되 그 사람의 것을 따르고, 되돌리기 비싼 것은 먼저 가져가고(이미 정했거나 맡겼으면 예외), 결과물이 숨기는 것은 넘길 때 말한다. 나머지는 묻지 않고 정한다. Claude 자체 검토에서도 같은 문제를 찾았다 |
| B. PoR에서 옮긴 문장을 바꿔 쓰면서 'by what it does, not by how it is phrased', 'taste or method', 'narrows the work'를 잃었다. 'the person's choice'는 스킬이 꼽은 목록에 기대는 순환 정의가 된다 | execution | 반영: 원문 그대로 옮긴다(H9) |
| C. 다섯 속성 가운데 유일하게 '있어야 할 내용'을 요구해서, 모든 스킬에 네 항목 절을 밀어 넣는다(K7 위반). threads·invest처럼 일부만 있는 좋은 스킬도 있다 | execution | 반영: "Where the domain has them" |
| H. (잠재) 5절 코드 관례(Python·uv·트리)는 새 속성을 skill-maker 자신에게 적용하면 사람 몫의 방법을 정해 버리는 줄이다. 공개 스킬의 다른 사용자가 이미 쓰는 스택·배치가 기본값에 덮인다 | execution(잠재) | H10과 충돌 → 성진에게 질문 |
| D. 맹점이 관찰과 묶여 있지 않다 → "has been seen to run wrong" | taste | 반영 |
| E. 기제 셋: '닻' 절은 뒤 규칙과 이어지지 않으니 삭제, "mostly confirms"는 과장, "cannot see"는 절대적 | taste | 부분 반영: "cannot find", "tends to repeat that reading"로 고쳤다. '닻' 절은 H3에서 합의한 기제이고 성진이 처음 짚은 '잘못된 방향으로 깊이 빠짐'의 핵심이라 남기되, 과장인 "every"를 빼 "carries into the steps built on it"으로 썼다 |
| F. "the line between the two"는 정의되지 않은 비유이고 뒤 목록이 이미 다 말한다 | taste | 반영: 삭제 |
| G. "gives it nothing to act on"은 틀린 이유다. 모델은 일반 확인 줄을 오히려 잘 따라 과잉 질문을 한다 | taste | 반영: "locates nothing, so it comes out as questions and hedges everywhere alike" |
| I. 3절 "their own judgment"는 추상적 자기 평가 질문(구호)을 부르고, "the person they work for"는 의뢰인을 전제한다 | taste | 반영: "judgment in this kind of work", "whoever the work is for" |
| J. Contrast 추가 문장이 "it"의 지시를 끊고, 한 번의 실행을 곧 맹점으로 승격한다 | taste | 반영: "Give the run…" 단락 끝으로 옮기고 "once the reason the user rejects it holds beyond this task" 조건을 붙였다 |
| K. 4절 책임 문장이 속성의 전제를 되풀이하고, "what was left out"은 작업별이라 상투 경고가 되기 쉽다 | taste | 반영: 전제를 빼고 "the kind of thing that gets left out" |
| L. 2절 첫 문장 "Judge each line…"이 존재 요구인 다섯째 속성을 다 설명하지 못한다 | taste | 수정 없음: C의 "Where the domain has them"으로 충분하다(리뷰어 동의) |

경계 판정 확인(리뷰어 Q7): design L20·L40·L22·L48, video L111·L122·L29, threads L13, invest L18-24는 모두 A·B 반영 후 바르게 판정된다.

### Sonnet 계획 리뷰 (2차, 같은 리뷰어 이어 쓰기)

- B·C는 해소됐다. A는 의도대로 해소됐다.
- E('닻' 절)는 이제 뒤 규칙(되돌리기 비싼 것은 먼저 합의)과 이어져 이의가 없다.
- v0.4.0 대비 보존 누락이 없다. 옮긴 레일 문장은 한 글자도 다르지 않다.
- 개정 본문 전체를 다섯 속성으로 읽은 결과, 아래 N1·N2를 반영하면 execution 등급이 남지 않는다.

| 지적 | 등급 | 처리 |
|---|---|---|
| N1. "which the model checks rather than asks about"는 사람만 증거를 가진 맹점까지 묻지 말라고 읽힌다. 예: design의 '불만을 금지로 옮길 때 해석을 확인받는다' | execution | 반영: "which the model looks for in the work". 사람만 줄 수 있는 것은 N2 문장이 맡는다 |
| N2. "The rest the model decides without asking"은 작업으로는 알 수 없는 사실(design의 용도·고정 문구·비율)까지 추측하게 한다. skill-maker 자신의 Agree 단락과 "Do not fill a gap with a guess"와도 충돌한다(H7) | execution | 반영: "What the work cannot show and only the person can supply, the model asks for when the answer would change the work; the rest it decides without asking." 과잉 질문(막히지 않는 결정 목록)은 '답이 작업을 바꿀 때'로 막는다 |
| "says why"가 "in a line"을 잃었다 | taste | 반영: "says in a line why" |
| "brings to the person first"는 '알리고 진행한다'로도 읽힌다 | taste | 반영: "gets the person's agreement on first"(4절 표현과 맞춤) |
| Contrast 문장 "the reason the user rejects it"이 어색하다 | taste | 반영: "the user's reason for rejecting it" |
| 무인 실행: 속성이 사람이 있는 상황을 전제한다 | taste | 수정 없음: P2 마지막 문장과 4절 "such as stopping or flagging it"이 다룬다(리뷰어 동의) |
| 길이: 다른 속성(55~100단어)보다 길다(약 250단어, 옮긴 레일 문장 43단어 포함) | taste | 수정 없음: 정확성 때문에 줄일 곳은 없다는 리뷰어 판단. 구현 리뷰 루프에서 다시 본다 |
| H15 자리: 첫 문단에 덧붙이면 "the same everywhere"와 부딪히고 구조 테스트 공백이 남는다 → 5절 마지막 문단을 넓힌다 | (H 후속) | 반영 |

### 결정과 대응 문장

| 결정 | 대응 문장 |
|---|---|
| H1 사람은 주인·책임자, 모델은 판단을 끝까지 | 새 속성 첫 절 "owns the goal and answers for the result, and the model brings its full judgment" |
| H3 맹점: 기제는 이유로 한 절 | 새 속성 "the skill carries what the model cannot find from inside the work, because an early reading carries into the steps built on it, …" |
| H3 맹점: 생성 스킬이 담을 것 | 새 속성 "names where the model's judgment has been seen to run wrong and the evidence that shows it, which the model looks for in the work" |
| H3 인터뷰 질문, 대조 실험 읽기 | 3절 첫 단락 두 항목, Contrast 추가 문장 |
| H2 책임 | 새 속성 "what the result will not show them", 4절 책임 문장 |
| H2 양방향(묻지 않고 할 것) | 새 속성의 항목별 행동, "What the work cannot show and only the person can supply, the model asks for when the answer would change the work; the rest it decides without asking.", "A line that tells the model to check in or to doubt itself in general locates nothing…" |
| H9 기능 판정 문장 | 새 속성 뒤에서 둘째 문장(원문 그대로) |
| H15 사용자의 스택·배치 | 5절 마지막 문단 |
| H7 skill-maker 자신 | skill-maker 본문은 이미 자기 맹점("Your phrasing is not their knowledge", Claude 취향, "Do not fill a gap with a guess"), 사람 몫(AskUserQuestion, 장부), 결과가 숨기는 것("Approval of the text is not evidence…")을 담는다. 리뷰에서 다섯 번째 속성으로 다시 판정한다 |

## 구현 단계 (다음 세션)

파일을 바꾸는 단계가 3개 이상이므로 시작할 때 `TaskCreate` 트래커를 연다. 각 단계의 완료 판정은 description에 적는다. 바꾸는 것은 SKILL.md 한 파일이고 코드가 없으므로 TDD 단계는 없다.

0. **준비**
   - 이 계획 파일이 아직 `claude-skills-skill-maker-tingly-pudding.md`이면 `.claude/plans/skill-maker-colleague-frame.md`로 바꾼다.
   - `main`에서 `feat/skill-maker-colleague-frame` 브랜치를 판다. 첫 커밋에는 계획 파일만 넣는다.
   - 관련 없는 작업 트리 변경은 커밋하지 않는다(`.claude/plans/user-inputs/…` 삭제, `.claude/histories`). `git add`에는 경로를 지정한다.
   - 완료 판정: 브랜치가 있고, 첫 커밋에 계획 파일만 있다.
1. **SKILL.md 개정**
   - 위 "개정 초안"(리뷰 반영본)과 보존 대조표대로 제자리에서 고친다. 새 속성을 뺀 나머지는 초안의 영어 문장을 그대로 옮긴다.
   - 새 속성은 H16에 따라 줄인 판을 쓴다. 계획 초안 판(약 250단어)은 비교용으로 남긴다.
   - 완료 판정:
     - 줄인 판의 단어 수와 계획 초안 판의 단어 수를 기록했다.
     - 줄인 판에도 H16의 고정 요소가 있다: 레일 문장 원문, 기제 셋, 항목별 행동, "Where the domain has them", N2 문장의 뜻.
     - `git diff`가 보존 대조표의 행에 있는 곳만 바꾼다. 대조표에 없는 삭제는 없다.
     - `grep -c '\${' "/Users/seongjin/Coding/Skill Maker/.claude/skills/skill-maker/SKILL.md"`가 `0`을 출력한다(매치가 없을 때 grep의 종료코드는 1이고, 이것이 기대값이다).
     - `claude plugin validate --strict "/Users/seongjin/Coding/Skill Maker/.claude/skills"`가 exit 0이다.
2. **독립 리뷰 루프 (H6)**
   - `Agent`(`model: "sonnet"`, general-purpose)를 읽기 전용 지시로 돌린다.
   - 입력:
     - 개정 후 SKILL.md 전문
     - v0.4.0 원문(`git show main:.claude/skills/skill-maker/SKILL.md`)
     - 이 계획의 결정 장부와 보존 대조표
   - 질문:
     - 다섯 속성 위반. 새 속성은 skill-maker 자신에게도 적용한다(H7).
     - 보존 누락
     - 두 모델이 다르게 읽을 문장
     - 레일이나 일반 협업·경각심 줄
     - 줄인 새 속성이 1·2차 리뷰 지적(A·C·N1·N2)을 다시 열었는가. 더 줄일 수 있는 절이 있는가(H16)
     - 필수 사례: 최종 새 속성으로 다음 줄을 판정하고, 기대 판정과 다르면 execution으로 보고한다.
       - `Agentic SNS/.claude/skills/threads/SKILL.md` L13 뒷부분("who does X follow" 같은 질문은 기본 예산 안에서 읽고, 무엇을 다뤘는지 말하고, 더 큰 읽기를 비용과 함께 제안한다). 기대 판정은 '묻지 않고 읽는다'다. N2 문장("asks for when the answer would change the work")이 '몇 페이지 읽을까요'를 먼저 묻게 만들면 실패다.
       - `claude artist/.claude/skills/design/SKILL.md` L14(만들기 전에는 용도·고정 문구·비율처럼 이미지로 확인할 수 없는 사실만 묻는다). 기대 판정은 '묻는다'다.
       - 같은 파일 L22(불만을 금지로 옮길 때 해석을 한 줄로 확인받는다). 기대 판정은 '사람만 줄 수 있는 증거이므로 확인받는다'다.
   - 응답 형식: 지적마다 위치, 등급(`execution`: 모델이 잘못 행동하게 만드는 것 / `taste`: 표현), 근거, 고칠 안.
   - 처리:
     - execution 등급이 0건이 될 때까지 고친다.
     - 첫 라운드에는 이 계획의 리뷰 처리표(1·2차)를 함께 넘겨, 반영하거나 기각한 지적이 다시 열리지 않게 한다. 이후 라운드는 같은 리뷰어를 `SendMessage`로 이어 쓴다.
     - 결정 장부와 충돌하는 지적은 AskUserQuestion으로 성진에게 올린다.
     - taste 등급은 반영하거나, 기각 사유를 PR 본문에 적는다.
   - 완료 판정: 최신 라운드의 execution 등급이 0건이고, 그 라운드가 검토한 본문이 승인받을 본문과 같다.
3. **전문 승인**
   - 성진에게 보여 줄 것:
     - 개정 후 SKILL.md 전문
     - v0.4.0 대비 diff
     - 바뀐 문장의 한국어 요약
     - 새 속성의 계획 초안 판과 줄인 판(단어 수와 무엇을 줄였는지)
   - AskUserQuestion으로 승인받는다. 부분 승인이나 침묵은 승인이 아니다.
   - 완료 판정: 성진이 전문을 명시적으로 승인했다.
4. **PR과 머지**
   - 논리 단위로 커밋하고 푸시한 뒤 PR을 연다. `gh pr merge --squash`로 머지한다.
   - 제목 예: `feat: skill-maker에 동료 프레임을 다섯 번째 속성으로 더한다`
   - 레포에 PR 템플릿이 없으므로 본문은 `## 무엇을 바꿨나` / `## 왜` / `## 영향` / `## 검증` 순서로 쓴다. PR #6과 같은 형식이다.
   - `## 왜`에 담을 것(개발 기록이므로 스킬 밖에 둔다):
     - 증거 요약
     - 진단(H2)
     - 기각한 대안: 독립 절, 일반 경각심 문장, PoR 안에 유지(K4), 'human in the loop' 이름, 실측, 전면 재작성
     - 리뷰 처리와 기각 사유
     - 같은 계열 리뷰어의 위험(H6)
   - 완료 판정: PR이 머지되고 `main`에 반영됐다. 심링크라서 `~/.claude/skills/skill-maker/SKILL.md`에 바로 반영되는지 `diff`로 확인한다.
5. **GitHub 릴리스 (H11)**
   - 머지 커밋에 `v0.5.0` 태그를 붙인다. 제목 예: `v0.5.0: 동료 프레임`
   - 본문 섹션은 v0.4.0과 같은 이름·순서다.
     - 첫 줄 요약
     - `## 무엇이 들어 있나`: 바뀐 판단마다 한 항목
     - `## 설치`: v0.4.0과 같다
     - `## 알려진 한계`
       - 실측 없이 리뷰와 승인만으로 검증했다.
       - 리뷰어가 같은 모델 계열(Sonnet)이다.
       - 맹점·책임 줄이 실제 생성 스킬에 생기는지, 과잉 질문이 줄었는지는 실사용에서 확인한다.
   - 명령: 본문을 스크래치패드 파일로 쓰고 `gh release create v0.5.0 --target main --title "<제목>" --notes-file <파일>`을 실행한다.
   - 완료 판정: `gh release view v0.5.0`이 Latest이고, `git rev-parse v0.5.0^{commit}`이 `origin/main`과 같다.

## Verification

- **구조:** `claude plugin validate --strict "/Users/seongjin/Coding/Skill Maker/.claude/skills"`가 exit 0이다. `${` grep 출력은 0이다.
- **보존:** 보존 대조표의 모든 행이 diff에서 확인되고, 대조표 밖의 삭제는 없다.
- **문서 품질:** Sonnet 독립 리뷰의 최신 라운드에서 execution 등급이 0건이다. 새 속성은 skill-maker 자신에게도 적용해 판정한다.
- **승인:** 성진이 전문을 승인했다.
- **배포:** `v0.5.0`이 Latest이고 머지 커밋을 가리킨다.
- **검증되지 않는 것(H5):**
  - 새 본문이 실제 skill-maker 세션에서 맹점과 책임 줄을 낳는지
  - 일반 협업 줄 없이 양방향으로 쓰이는지
  - 과잉 질문이 생기지 않는지
  - 이것들은 실사용에서 관찰한다.
