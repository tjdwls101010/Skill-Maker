# skill-maker: 사용자와 함께 일하는 스킬을 만들게 하는 개선 계획

> 계획 세션(2026-10-15)에서 작성했다. 계획 모드에서는 지정 경로에만 쓸 수 있어서, 승인 직후 `.claude/plans/skill-maker-collaboration.md`로 이름을 바꾼다. 이전 세 계획과 같은 방식이다.

## Context

- skill-maker(v0.3.0, SKILL.md 한 파일)는 사용자의 전문성을 스킬로 옮긴다. 성진은 이 스킬을 잘 쓰고 있다.
- 문제: skill-maker가 만든 스킬이 "한 번의 입력을 받아 혼자 끝까지 수행한다"를 전제로 상황마다 대처법까지 담는다. 이유와 뒤집는 조건을 붙여 원칙 형식을 갖춰도, 스킬을 쓰는 사람이 그 자리에서 정할 일을 미리 정해 버리므로 기능은 레일이다.
- 실제 런타임: 스킬을 쓰는 Claude는 사용자와 여러 턴 의논하며 일한다. 사용자는 여러 명일 수 있고 방법론·취향이 저마다 다르다.
- 목표: skill-maker가 만드는 스킬에서 Claude는 자기 지능·창의성·미감을 보존하면서, 사용자를 존중하고 소통하며 함께 일하는 동료로 행동한다.
- 성공 기준은 품질 하나. 부분 수정으로 안 되면 재작성까지 고려한다(성진).
- 구현 후 GitHub 릴리스를 한다(성진). 직전 릴리스는 `v0.3.0`.

## 증거 (filmcraft → video 스킬 제작 기록, `~/Coding/video`)

### 사실
- 성진의 영상 편집 경험은 "거의 없음"이다. 첫 계획 인터뷰에서 성진이 직접 답했다.
- 첫 초안(`video/.claude/plans/filmcraft-skill.md` 7절 "Editorial judgment")에 담긴 원칙: 직컷이 기본이고 디졸브는 시간·장면 전환, 자막은 하단 중앙이고 가리면 위로, 의미 있는 필러는 남긴다, 박힌 슈퍼에 얹지 않는다. 각 원칙에 이유와 뒤집는 조건이 붙어 있었다.
- skill-maker의 "Your phrasing is not their knowledge" 문단을 따른 단계 14a(원칙 경계 확인)에서 위 원칙의 경계를 성진에게 물었다. 질문은 필러·전환·방송 슈퍼·자막 위치 넷이었고, 성진은 dismiss한 뒤 이렇게 지적했다(10/14 01:53): "사실 이런 건 원칙인 것 같아 보이지만 레일에 가깝지 않을까? …"
- 지적 후 바뀐 것:
  - 취향 규칙을 빼고 "What does not depend on taste"(사람마다 갈리지 않는 것)와 "Working with the user"로 바꿨다.
  - 협업 리듬은 성진 결정 "싼 건 먼저 해 보이고, 비싼 건 묻기"를 따랐다.
  - 14a는 폐기했다.
- video 개선(10/14) 때 성진이 한 교정은 모두 "덜 담아라" 방향이었다:
  - 일반 그래픽 원칙: "이거 다 이미 클로드가 충분히 이해하는 규칙인거 같은데?"
  - ffmpeg 예외: "어차피 사용자와 클로드가 대화하며 문제가 있으면 클로드가 충분히 대처할 수 있을거 같은데"
  - 새 사례 경계 확인: "이 스킬은 다양한 영상을 대상으로 활용될 수 있어야 하는거 알지?" → 장르·사례별 규칙은 레일, 애매한 경우는 대화로
  - 작품 폴더: 4일차 폴더 구조를 관례로 삼지 않는다("너무 해당 프로젝트에 적합하게 한거")
  - 취향 기록: 메모 없이 과거 결과물(`data/`)만
- 같은 기록에 따르면 성진은 skill-maker 절차가 빠진 것을 먼저 지적했다(`video-graphics.md` "skill-maker 원문 대비 감사"). 그 절차를 그대로 따른 결과(새 사례 경계 확인)는 다시 기각했다. 즉 원인은 실행 누락이 아니라 skill-maker 본문이다.
- 외부 근거(`.tmp/references/The new rules of context engineering…`):
  - 규칙이 틀리는 이유의 예로 "the user may have their own preferences"를 든다.
  - 스킬에 대해서는 "Avoid making them overconstrained, except in highly important areas", "particular to you, your team, or product"라고 쓴다.

### 진단 (Claude 추론, 성진이 전부 확인)
- **레일 판정의 기준.** 네 원칙은 맞다. 빠진 것은 스킬이 쓰이는 상황의 모델이다. 지금은 레일인지를 형식(이유가 붙었나)으로 가린다. 앞으로는 기능(그 결정이 누구 몫인가)으로 가린다.
- **원인 셋 (skill-maker 본문).**
  - 도입부 "what this user, as an expert, would do"가 저자와 사용자를 한 사람으로 본다.
  - 아직 안 정해진 질문을 보낼 곳이 본문, 인터페이스, 저자에게 묻기 셋뿐이다. "사용 중의 대화에 남긴다"는 갈래가 없다. 여기에 "Do not fill a gap with a guess"가 겹쳐, 모든 것을 만들 때 확정하려 든다.
  - 원칙 문장의 요건(이유·조건·뒤집는 조건·판정 사례)과 "Your phrasing is not their knowledge"의 경계 확인이 모든 원칙을 끝까지 명세하라고 민다.
- **비용의 비대칭.** 사용자가 곁에 있으면 빠진 줄의 비용은 대화 한 턴이다. 틀렸거나 좁은 줄의 비용은 모든 사용자와 모든 세션에 조용히 쌓인다.
- **보정 셋.**
  - 개인 스킬에서는 저자의 취향도 정당한 내용이다. 문제는 취향이 아니라 누가 쓰는 스킬인지와 맞지 않는 권위다.
  - 모든 스킬에 "끊임없이 논의"나 일반적인 협업 절을 넣는 것도 레일이고 군더더기다. 그 도메인에서 무엇이 비싸고 무엇이 취향인지만 담는다.
  - 저자가 그 분야 전문가가 아닐 때를 skill-maker가 다루지 못한다. filmcraft에서는 그 빈자리를 Claude의 취향이 규칙으로 메웠다.

## 결정 장부

| # | 결정 | 소유자 | 근거·영향 |
|---|---|---|---|
| K1 | 위 진단과 보정 셋을 계획의 전제로 삼는다 | 성진 | 질문 1 "모두 맞음" |
| K2 | 구현 후 GitHub 릴리스(직전 `v0.3.0`) | 성진 | 마지막 단계 |
| K3 | 계획 파일명은 `skill-maker-collaboration.md`. 승인 직후 바꾼다 | Claude (계획 모드 제약) | 이전 세 계획과 같은 방식 |
| K4 | "누구 몫인가"는 다섯 번째 원칙이 아니라 principle over rail 안의 한 문장이다. 레일의 정의를 형식에서 기능으로 넓힌다 | 성진 | 원칙은 넷 그대로. 리뷰 기준도 그대로 |
| K5 | 제자리 개정 + 보존 대조표. 5절(코드)은 손대지 않는다 | 성진 | v0.3.0 통째 교체 초안이 이유 다섯 가지를 놓쳤던 실패. 기존 문장은 모두 유지·수정·이동·삭제(사유) 중 하나 |
| K6 | 헤드리스 실측(기준선·재측) 없음. 검증은 codex 문서 리뷰 + `validate --strict` + 전문 승인 | 성진 | 원샷 실행은 협업 품질을 재지 못한다. 효과는 실사용에서 본다 |
| K7 | 모든 생성 스킬에 일반 협업 절을 넣으라고 하지 않는다 | 성진(질문 1 보정 확인) | 기본 동작을 되풀이하는 군더더기이자 레일. 도메인의 사실(무엇이 비싸고 되돌리기 어려운가)은 기존 '사실' 범주로 들어간다 |
| K8 | 독립 검토(계획 리뷰·구현 리뷰 루프)는 codex `gpt-6.1-sol`, effort `medium`, read-only | 성진 | 처음 요청의 gpt-6-astra high를 대체한다 |
| K9 | 릴리스 번호는 `v0.4.0`이다. 기능 추가이므로 0.x의 minor를 올린다. 제목과 본문은 v0.3.0 형식을 따른다 | Claude (v0.2.0·v0.3.0 관례) | — |
| K10 | 4절에 "Redirecting is cheap only while undoing is…"를 넣는다. 일반 협업 절이 아니라, 모델이 못 보는 되돌리기 비용만 도메인 사실로 담는다 | 성진 (codex 1차 사례 적용의 유일한 불일치) | K7과 양립한다. video의 "Costly to undo… ask first" 같은 줄이 저절로 나오게 한다 |
| K11 | 공유 스킬의 개인 취향은 각자 대화에서 고르고, 그 사람의 결과물이나 지시가 다음 세션으로 나른다. 공유 스킬 본문은 나르지 않는다(일반 원칙) | 성진 (video `data/` 결정의 일반화를 확인) | 3절 authority 추가 문장 |

## SKILL.md 변경 후 섹션 구조

frontmatter는 바꾸지 않는다. 본문은 영어로 쓰고, 한 덩어리를 한 줄로 쓴다(하드랩 없음). `${` 형태는 쓰지 않는다(skill-maker를 로드할 때 치환된다).

1. `# Turning a user's expertise into a skill`: P1 수정, P2 신설, P3 수정, P4 그대로
2. `## What earns a place in a skill`: Principle over rail에 한 문장. 나머지 그대로
3. `## Drawing out what the user knows`
   - 그대로인 단락: 첫 단락, Contrast with the default, Probe with specifics, Starting from an existing skill
   - 끝에 한 문장을 더하는 단락: Read before you ask, Keep kinds of authority apart
   - **When the user knows the goal but not the domain**: 새 단락, authority 다음에 둔다
   - Your phrasing is not their knowledge: 범위를 좁히고 한 문장을 더한다
   - Agree on what you cannot establish: 첫머리와 마지막 문장을 수정한다
4. `## Writing judgment down`: 원칙 구성 문단에 한 문장, 완료 기준 문단 수정. 나머지 그대로
5. `## Code the skill bundles`: 변경 없음(K5)
6. `## When the skill is done`: 둘째 항목 첫 절 수정. 나머지 그대로

## 개정 초안 (codex 계획 리뷰 3차까지 반영)

용어:
- "the user"는 skill-maker 세션에서 함께 스킬을 만드는 사람, 곧 저자다.
- "the person working with the model"은 만들어진 스킬을 쓰는 사람이다.

표시: `+`는 추가, `~`는 수정이다. 표시가 없는 것은 원문 그대로다.

**1. 도입부**
- P1 `~`: "what this user, as an expert, would do"를 "what an expert in the work would do"로 바꾼다. 나머지는 그대로다.
- P2 `+` (새 문단)
  > The model running a skill seldom works alone: someone usually works with it over many turns, and a skill several people use serves people with their own methods and taste. So a skill is what a capable colleague brings to that work, not a script for finishing it alone. It settles what holds across the people and the work it serves and what the model would otherwise get wrong, and leaves to the person there what is theirs to choose: with someone there, a line left out costs a turn of conversation, while a line that settles their choice misleads every session it is wrong for. Where nobody is there to redirect the model, as in a scheduled job, the skill has to settle more in advance.
- P3 `~`: "The user owns the knowledge, the values and the goal"를 "The user you are building the skill with owns the knowledge, the values and the goal"로 바꾼다. 나머지는 그대로다.
- P4(출발점 문단): 그대로다.

**2. What earns a place in a skill**
- Principle over rail `+`: invoice 예시 뒤, "Write a fixed sequence…" 앞에 넣는다.
  > A line is a rail by what it does, not by how it is phrased: one that settles a matter of taste or method for the person working with the model, when neither they nor a convention they work under has settled it, narrows the work however well its reason is stated.

**3. Drawing out what the user knows**
- Read before you ask `+`: 끝에 붙인다.
  > A past output shows what one piece of work needed; carry a lesson from it only as far as the evidence reaches, with the conditions under which it applies, rather than making that piece's choices the rule for every piece.
- Keep kinds of authority apart `+`: "…so write each as what it is." 뒤에 넣는다.
  > A preference binds the people who hold it and a team convention the people who work under it: in a skill that serves only its author, the author's preference is knowledge, and in a skill others use it is a choice each person makes in the conversation, carried between sessions by their own work or instructions rather than by the skill they share.
- `+` 새 단락 (authority 다음)
  > **When the user knows the goal but not the domain.** They still own the goal, their own preferences and the requirements the work answers to, but the domain judgment the skill carries then comes from what can be checked, such as how a tool behaves or what a platform requires, and from what the contrast shows the model getting wrong, with the conditions under which it does. Your own taste in the domain is not something the model running the skill lacks, so it does not become the skill's rules, and the user is not the one to settle its boundaries.
- Your phrasing is not their knowledge
  - `~`: "When a principle decides close calls"를 "When a principle the skill will carry decides close calls"로 바꾼다.
  - `+`: 끝에 붙인다.
    > When the boundary turns on taste, it is the user's to settle only for their own preference in a skill that serves only them, or for a convention they set for the people it serves; otherwise it belongs to the person using the skill.
- Agree on what you cannot establish
  - 첫머리 `~`: 기존 첫 문장 "Anything whose answer would change the skill and that evidence cannot settle goes to the user…"의 앞부분을 이렇게 바꾼다. 뒤의 "…one consequential decision at a time…"부터는 그대로다.
    > First tell apart what the skill must carry from what each person using it will choose in the moment: the second is settled in that session, and settling it now writes one person's answer into every session. Anything the skill must carry whose answer would change it and that evidence cannot settle goes to the user through AskUserQuestion, …
  - 마지막 문장 `~`
    > Who will use the skill and whether anyone will be there when it runs, where it will live (project, user or plugin) and what language it is written in are among these decisions: the first two decide how much the skill can settle for the people using it, and the location decides whether the skill must be self-contained.

**4. Writing judgment down**
- 원칙의 구성 문단 `+`: 첫 문장 뒤에 넣는다.
  > It need not decide every close call when someone can redirect the model: at an unclear edge the model chooses and says why. Redirecting is cheap only while undoing is, so where a choice in the domain commits much work or is hard to undo in a way the model would not see, the skill says so, and the model gets agreement on that choice before committing unless the person has already made it or left it to the model; where nobody will be there, the skill settles what the model does when an unclear case could break a requirement, such as stopping or flagging it.
- 완료 기준 문단 `~`
  > Say what must hold before the model hands work over, in terms it can check; whether the work is finished is often the person's call. Separate what must be preserved from what is free to vary, which the model and the person settle between them, so a completion criterion does not become a demand for one particular output.

**6. When the skill is done**
- 둘째 항목 `~`: "The central judgments and their boundaries are written"를 "The judgments the skill carries and their boundaries are written"로 바꾼다. 나머지는 그대로다.

### 보존 대조표 (v0.3.0 → 개정 후)

v0.3.0 문장 가운데 아래 표에 없는 것은 모두 그대로 둔다. 삭제는 없다.

| v0.3.0 문장 | 처리 |
|---|---|
| 도입 P1 "…what this user, as an expert, would do…" | 수정: "an expert in the work". 나머지 그대로 |
| (없음) | 추가: 도입 P2 |
| 도입 P2 "The user owns the knowledge, the values and the goal; …" | 수정: "The user you are building the skill with owns…". 나머지 그대로 |
| Principle over rail 항목 | 유지 + 한 문장 추가(invoice 예시 뒤) |
| Read before you ask 단락 | 유지 + 끝에 한 문장 |
| Keep kinds of authority apart 단락 | 유지 + 둘째 자리에 한 문장 |
| (없음) | 추가: When the user knows the goal but not the domain 단락 |
| Your phrasing… "When a principle decides close calls…" | 수정: "a principle the skill will carry" + 끝에 한 문장 |
| Agree… "Anything whose answer would change the skill and that evidence cannot settle goes to the user through AskUserQuestion, …" | 수정: 앞에 구분 문장을 두고 "Anything the skill must carry whose answer would change it…"로 바꾼다. 뒤 절은 그대로 |
| Agree… "Where the skill will live (project, user or plugin) and what language… self-contained." | 수정: 누가 쓰는지와 실행 때 사람이 있는지를 더한다. 위치와 자기완결의 이유는 유지 |
| 4절 "A principle carries its reason, … two similar-looking options." | 유지 + 바로 뒤에 두 문장 |
| 4절 "Say how the model will know the work is done, … one particular output." | 수정: "넘기기 전에 성립할 것"과 "모델과 사람이 함께 정한다". "a completion criterion does not become a demand for one particular output"는 유지 |
| 6절 "The central judgments and their boundaries are written, …" | 수정: "The judgments the skill carries". 나머지 그대로 |

### codex 계획 리뷰 (1차: gpt-6.1-sol medium, read-only)

- run `20261015-101805-draft-review-f0d1`, thread `01a13d23-4cc0-7d21-8e32-91718b5aa2a9`

| 지적 | 등급 | 처리 |
|---|---|---|
| PoR 문장이 개인 스킬에서 저자가 스스로 정한 취향까지 레일로 읽힌다 | execution | 반영: "when neither they nor a convention they work under has settled it" |
| "A preference binds only the people who hold it"가 팀 관례를 각자의 대화로 넘긴다 | execution | 반영: 팀 관례는 그 아래서 일하는 사람을 묶는다 |
| Agree 첫 문장이 여전히 모든 질문을 저자에게 보낸다. 사용 중 대화로 넘길 질문을 먼저 가르는 단계가 없다 | execution | 반영: 구분 문장을 첫머리에 두고, 저자에게 묻는 대상을 "the skill must carry"로 좁힌다 |
| 비전문가 저자 단락이 저자의 선호·요구사항·팀 관례까지 배제한다. "for anyone"은 조건부 실패(예: 한글 폰트)를 버린다 | execution | 반영: 저자가 여전히 소유하는 것을 명시하고, "with the conditions under which it does"로 바꾼다 |
| "close call을 다 정하지 않아도 된다"가 무인 실행을 고려하지 않는다 | execution | 반영: 무인이면 멈춤·표시 같은 처리를 정한다 |
| 되돌리기 비싼 선택과 싼 선택을 가를 도메인 사실을 끌어내지 않는다. 사례 적용 13개 중 유일한 불일치 | execution | 반영: "Redirecting is cheap only while undoing is…". K7의 '사실 범주'를 명시적으로 만든 것이다. 성진 확인(K10) |
| 6절 "The central judgments and their boundaries are written"이 대화로 넘긴 선택에 대해 경계 추출을 다시 시킨다 | execution | 반영: "The judgments the skill carries" |
| "it becomes a rule only where it would hold for the other work"가 조건부 교훈을 버린다 | execution | 반영: 증거가 닿는 만큼, 조건과 함께 |
| 삭제 제안: P2 앞 두 문장 | — | 기각: P2는 진단 원인 1(도입부가 저자와 사용자를 한 사람으로 봄)의 수정이다. 지역 문장들이 공유하는 이유(여러 턴, 여러 사람, 비용 비대칭)를 담아 재도출 근거가 된다. 두 문장은 한 문장으로 합쳤다 |
| 삭제 제안: "A boundary that falls on someone's taste…" | — | 기각하되 고쳐 씀: 14a에서 취향 원칙을 원칙으로 오인한 채 경계를 물었다. 탐침 중에 멈출 신호가 필요하다. 개인 스킬 예외를 넣어 다시 썼다 |

사례 적용: 피드백 전 filmcraft 초안의 13개 줄 가운데 12개에서 개정 skill-maker의 판정이 성진의 실제 결과와 같았다. 다른 1개는 "Yours to choose…"(싼 선택과 비싼 선택의 구분)였고, 위 반영으로 다룬다.

### codex 계획 리뷰 (2차: 같은 thread resume)

- run `20261015-102155-draft-review-cf1f`
- 1차 지적은 모두 고쳐졌다. 특히 Agree 첫머리와 "Do not fill a gap with a guess"의 상호작용이 해소됐다. 기각한 삭제 제안 둘은 다시 열리지 않았다.
- 사례 적용에서 남았던 1개도 이제 일치한다(13/13).

| 지적 | 등급 | 처리 |
|---|---|---|
| Phrasing 마지막 문장의 '개인 스킬만'이 팀 관례 쪽 수정과 충돌한다. 관례를 정할 권한이 있는 저자에게도 묻지 않게 된다 | execution | 반영: 저자가 묻는 대상을 "their own preference in a skill that serves only them, or a convention they set for the people it serves"로 한다 |
| "the skill names the choices…"가 모델이 이미 아는 비용까지 목록으로 쓰게 한다. dense와 K7에 어긋난다 | execution | 반영: "in a way the model would not see" |
| "raises"가 합의를 받는다는 뜻인지 알리기만 한다는 뜻인지 모호하다. 이미 허락받은 선택의 예외가 없다 | execution | 반영: "gets agreement on that choice before committing unless the person has already made it" |

### codex 계획 리뷰 (3차: 같은 thread resume)

- run `20261015-102422-draft-review-a4ec`
- 2차 지적 셋 가운데 둘이 해결됐다. 팀 관례와의 충돌, 그리고 비용 목록이 무조건 들어가던 문제다. 새로 생긴 실패는 없고, K7·K10·K11과 충돌하지 않는다.

| 지적 | 등급 | 처리 |
|---|---|---|
| "unless the person has already made it"이 사람이 모델에게 맡긴 선택("구간은 네가 골라, 묻지 마")은 면제하지 않는다 | execution | 반영: "unless the person has already made it or left it to the model". 이 판에 대한 확인은 구현 2단계 리뷰 루프(같은 thread)가 맡는다 |

### 원인과 대응 문장
| 원인·증거 | 대응 문장 |
|---|---|
| 도입부가 저자와 사용자를 한 사람으로 봄 | P1 수정, P2 신설, P3 "you are building the skill with" |
| 행선지에 '사용 중 대화에 남긴다'가 없음 | P2, Agree 첫머리, Agree 마지막 문장(누가 쓰나·실행 때 사람이 있나) |
| 원칙 요건이 완전 명세를 강요 | 4절 "need not decide every close call", Phrasing 범위 한정 + 마지막 문장, 6절 "judgments the skill carries" |
| filmcraft 'Editorial judgment' (Claude 취향의 규칙화) | 비전문가 저자 단락, PoR 추가 문장 |
| 14a 원칙 경계 확인 | Phrasing 범위 한정 + "When the boundary turns on taste…" |
| filmcraft "Yours to choose, with the reason in the report" (혼자 끝내는 완료) | 4절 완료 기준 수정, "Redirecting is cheap only while undoing is…" |
| 4일차 폴더를 관례로, "다양한 영상" | Read before you ask 추가 문장, P2 "across the people and the work" |
| 보정: 개인 스킬의 저자 취향은 정당 | Authority 추가 문장, PoR "neither they nor a convention…", Phrasing 마지막 문장 |
| 보정: 무인 실행 | P2 마지막 문장, Agree 마지막 문장, 4절 "where nobody will be there…" |
| video: 취향은 과거 결과물로 (`data/`) | Authority 추가 문장 "carried between sessions by their own work or instructions" |

## 구현 단계 (다음 세션)

파일을 바꾸는 단계가 3개 이상이므로 시작할 때 `TaskCreate` 트래커를 연다. 각 단계의 완료 판정은 description에 적는다. 바꾸는 것은 SKILL.md 한 파일이고 코드가 없어 TDD 단계는 없다.

0. **준비**
   - 이 계획 파일의 이름을 `.claude/plans/skill-maker-collaboration.md`로 바꾼다. 승인 직후 이미 바꿨으면 생략한다.
   - `main`에서 `feat/skill-maker-collaboration` 브랜치를 판다. 첫 커밋에는 계획 파일만 넣는다.
   - 관련 없는 작업 트리 변경은 커밋하지 않는다. `.claude/plans/user-inputs/…` 삭제와 `.claude/histories`가 그렇다. `git add`에는 경로를 지정한다.
   - 리뷰 증거 파일을 레포 `.tmp/evidence/`로 복사한다. `.tmp/`는 gitignore돼 있다.
     - 원본: `/private/tmp/claude-501/-Users-seongjin-Coding-Skill-Maker/61b7c618-3552-4ac5-a432-03a5d05f575d/scratchpad/evidence/filmcraft_prefeedback_SKILL.md`
     - 원본이 사라졌다면 이렇게 다시 만든다. `~/.claude/projects/-Users-seongjin-Coding-video/e04736f8-5f88-443b-8251-f997cf8110fa.jsonl`의 2407번째 줄은 assistant의 `Write` 호출이다. 그 `input.content`를 꺼내 저장한다. 첫 줄이 `---`, 둘째 줄이 `name: filmcraft`이면 맞는 파일이다.
   - 완료 판정: 브랜치가 있고, 첫 커밋에 계획 파일만 있으며, `.tmp/evidence/filmcraft_prefeedback_SKILL.md`에 `## Editorial judgment` 절이 들어 있다.
1. **SKILL.md 개정**
   - 위 "개정 초안"과 보존 대조표대로 제자리에서 고친다. 초안의 영어 문장을 그대로 옮긴다.
   - 줄바꿈은 기존대로 한 덩어리를 한 줄로 쓴다.
   - 완료 판정:
     - `git diff`가 보존 대조표의 행에 있는 곳만 바꾼다. 대조표에 없는 삭제는 없다.
     - `grep -c '\${' "/Users/seongjin/Coding/Skill Maker/.claude/skills/skill-maker/SKILL.md"`가 `0`을 출력한다. 매치가 없을 때 grep의 종료코드는 1이고, 이것이 기대값이다.
     - `claude plugin validate --strict "/Users/seongjin/Coding/Skill Maker/.claude/skills"`가 exit 0이다.
2. **codex 문서 리뷰 루프 (K8)**
   - `codex` 스킬로 `gpt-6.1-sol` medium을 read-only로 돌린다.
   - 계획 리뷰 thread(아래 "codex 계획 리뷰")를 resume해 반영·기각 이력을 이어 간다. thread를 쓸 수 없으면 새로 시작하고, 이 계획의 결정 장부와 처리표를 함께 넘긴다.
   - 입력:
     - 개정 후 SKILL.md 전문
     - v0.3.0 원문(`git show main:.claude/skills/skill-maker/SKILL.md`)
     - 이 계획의 결정 장부와 보존 대조표
     - 증거: `.tmp/evidence/filmcraft_prefeedback_SKILL.md`(피드백 전), `/Users/seongjin/Coding/video/.claude/skills/video/SKILL.md`(피드백 후)
   - 질문: 네 원칙 위반, 보존 누락, 두 모델이 다르게 읽을 문장, filmcraft 피드백 전 초안에 적용했을 때 성진의 실제 결과와 어긋나는 판정.
   - 응답 형식은 계획 리뷰와 같은 `--schema`다.
   - execution 등급이 0건이 될 때까지 고친다.
   - 결정 장부와 충돌하는 지적은 AskUserQuestion으로 성진에게 올린다.
   - taste 등급은 반영하거나, 기각 사유를 PR 본문에 적는다.
   - 완료 판정: 최신 라운드의 execution 등급이 0건이고, 그 라운드가 검토한 본문이 승인받을 본문과 같다.
3. **전문 승인**
   - 성진에게 보여 줄 것: 개정 후 SKILL.md 전문, v0.3.0 대비 diff, 바뀐 문장의 한국어 요약.
   - AskUserQuestion으로 승인받는다. 부분 승인이나 침묵은 승인이 아니다.
   - 완료 판정: 성진이 전문을 명시적으로 승인했다.
4. **PR과 머지**
   - 논리 단위로 커밋하고 푸시한 뒤 PR을 연다. `gh pr merge --squash`로 머지한다.
   - 제목 예: `feat: skill-maker가 사용자와 함께 일하는 스킬을 만들게 한다`
   - 레포에 PR 템플릿이 없으므로 본문은 `## 무엇을 바꿨나` / `## 왜` / `## 영향` / `## 검증` 순서로 쓴다.
   - `## 왜`에 담을 것(개발 기록이고 스킬 밖에 둔다):
     - filmcraft 증거 요약
     - 진단
     - 기각한 대안: 다섯 번째 원칙, 전면 재작성, 헤드리스 실측, 일반 협업 절
     - codex 리뷰 처리와 기각 사유
   - 완료 판정: PR이 머지되고 `main`에 반영됐다. 심링크라 `~/.claude/skills/skill-maker/SKILL.md`에 바로 반영되는지 `diff`로 확인한다.
5. **GitHub 릴리스 (K2, K9)**
   - 머지 커밋에 `v0.4.0` 태그를 붙인다. 제목 예: `v0.4.0: 사용자와 함께 일하는 스킬`
   - 본문 섹션은 v0.3.0과 같은 이름·순서다.
     - 첫 줄 요약
     - `## 무엇이 들어 있나`: 바뀐 판단마다 한 항목
     - `## 설치`: v0.3.0과 같다
     - `## 알려진 한계`: 헤드리스 실측 없이 리뷰와 승인만으로 검증했다. 협업 품질과 과교정 여부는 실사용에서 확인한다.
   - 명령: 본문을 스크래치패드 파일로 쓰고 `gh release create v0.4.0 --target main --title "<제목>" --notes-file <파일>`을 실행한다.
   - 완료 판정: `gh release view v0.4.0`이 Latest이고, `git rev-parse v0.4.0^{commit}`이 `origin/main`과 같다.

## Verification

- **구조:** `claude plugin validate --strict "/Users/seongjin/Coding/Skill Maker/.claude/skills"`가 exit 0이다. `${` grep 출력은 0이다.
- **보존:** 보존 대조표의 모든 행이 diff에서 확인되고, 대조표 밖의 삭제는 없다.
- **문서 품질:** codex 문서 리뷰의 최신 라운드에서 execution 등급이 0건이다.
- **사례 적용:** filmcraft 피드백 전 초안에 개정 본문을 적용한 리뷰어의 판정이 성진의 실제 결과(video SKILL.md)와 어긋나지 않는다. 어긋나면 그 이유를 기록한다.
- **승인:** 성진이 전문을 승인했다.
- **배포:** `v0.4.0`이 Latest이고 머지 커밋을 가리킨다.
- **검증되지 않는 것(K6):** 새 본문이 실제 skill-maker 세션에서 협업형 스킬을 낳는지. 개인 스킬에서 저자 취향을 잃는 과교정이 생기는지. 실사용에서 관찰한다.
