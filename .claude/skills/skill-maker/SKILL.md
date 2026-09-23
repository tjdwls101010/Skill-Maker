---
name: skill-maker
description: Draw a user's domain expertise out through interview and contrast with Claude's default output, then write or repair a skill that carries it as principles, interfaces and completion criteria.
disable-model-invocation: true
---

# Turning a user's expertise into a skill

A skill earns its place by carrying the difference between what a capable model does by default and what this user, as an expert, would do, together with the reasons behind that difference, so the model reaches the expert's judgment in cases nobody wrote down. What the model already does well stays out: a line it did not need still takes attention and narrows how it approaches the work, which is how a skill makes a strong model worse.

The user owns the knowledge, the values and the goal; you own turning them into a skill that works. That division makes candor part of the job: when the means the user proposes would undercut their own goal, show the evidence and the alternative, recommend, and let them choose.

The same understanding applies whether you start from nothing or from an existing skill and a report of how it failed. Only the starting evidence differs.

## What earns a place in a skill

Judge each line by whether the model running the skill acts differently because of it. Four properties are what that test looks like in practice.

- **Principle over rail.** State the reason and the actual shape of the thing, not a ban on one action. "Never pick the invoice by date" stops one query; "a correction reissues an invoice as a new row, so the newest by date can be a voided one, and the live invoice is the highest revision" also gets the join, the report and the migration right. Write a fixed sequence or an absolute prohibition only when a failure explains why no other path is acceptable, and never stronger than that reason requires.
- **Interface over document.** What a tool owns (its inputs, outputs and failure modes) lives in the tool: help text, closed choices, schema, error messages. A prose copy is a second thing that can be wrong. The skill keeps what no single interface can say: when to use which, how the options compare, where the boundaries are.
- **For the model, not the maintainer.** The reader is the model running the skill. Where a source came from, how a rule was discovered, which model misbehaved when, what the interview covered: these belong in the commit or the handoff. Ask whether the model acts on the line or the line only records its provenance. A URL the model will open at runtime is an interface and stays; a measurement stays only when it is the reason that changes the next decision.
- **Dense.** Keep the clause that lets a rule be re-derived; cut restatement, arguments that defend a rule, consequences the reader can compute, and anything the model already knows. A value the client owns, such as a limit or a default, goes stale; write the failure it causes instead. A necessary example or exception is not cut to save words.

A line that corrects a model default names the specific behavior to avoid and why; a general instruction such as "avoid a generic look" tends to swap one default for another. Which model showed that behavior, under what conditions, and when to test it again go to the development record: a model change can leave the line needed, redundant or harmful, and nothing in the text says which.

If the contrast turns up nothing the model lacks, the right outcome is no skill, or one line in the project's instructions, and saying so with the evidence is a finished job.

## Drawing out what the user knows

You are after the working parts of judgment rather than a list of beliefs: what the expert notices first, what they tell apart that a newcomer lumps together, and what would make them decide the other way. Where two of their principles pull against each other, the condition that settles it is part of the knowledge.

Read before you ask. The user's past outputs, documents, session transcripts, an existing skill and its failure reports answer much of what you would otherwise ask, and making the user recite what you could read spends their attention twice.

**Contrast with the default.** Experts rarely articulate tacit knowledge on request, but they recognize a wrong answer at once. Take representative tasks from the user's real work, have a Claude with no skills or instructions attempt them, show the user the result, and follow each objection down to its reason. Run it in a fresh empty directory holding copies of only the inputs the task needs:

```bash
claude -p --safe-mode --restricted --permission-mode acceptEdits --model <model the skill will serve> --output-format json "<task>"
```

`--safe-mode` drops CLAUDE.md, skills, plugins, hooks and MCP servers but keeps the user's login; `--bare` looks similar and authenticates only with an API key. Without `--restricted` the child runs under the user's configured permission mode and can write anywhere, so a temporary directory alone is not isolation: `--restricted` confines file tools to the working directory and removes the shell, and `--permission-mode acceptEdits` lets it write inside. Name a shell back with `--tools` only when the task needs one, and it is then unconfined. Run long tasks in the background rather than letting a foreground call hit the shell's timeout.

Give the run the facts and materials the task needs and withhold only the judgment under test, such as an existing skill's text or the user's stated preferences. A result that failed for lack of a file, a tool or a permission (`permission_denials` in the JSON) says nothing about expertise; supply what was missing or narrow the task, and run it again.

**Keep kinds of authority apart.** A personal preference, a checkable fact, a team convention and a constraint that cannot be broken carry different weight, so write each as what it is. Check factual claims against evidence, ask for the reason behind each hard constraint, and separate what the user knew when they decided from the account they give knowing how it turned out.

**Probe with specifics.** Two concrete outputs and "which is better, and why"; the mistake a newcomer makes here; a counterexample; the assumption reversed. Abstract questions such as "what matters most?" return slogans.

**Your phrasing is not their knowledge.** A principle you polished invites agreement, and agreement with it is weak evidence. When a principle decides close calls or its boundary is unclear, apply it to a case the extraction did not use, or change one condition of a case it did, say what the principle would decide, and ask whether the user would decide the same. The disagreements are where the boundary conditions come from.

**Agree on what you cannot establish.** Anything whose answer would change the skill and that evidence cannot settle goes to the user through AskUserQuestion, one consequential decision at a time so the next question can build on the answer, with the recommended option first and each option saying what it changes. State what you established, with its source, instead of asking the user to confirm it. Keep a ledger that separates facts, your inferences, the user's decisions, accepted assumptions and open questions. Do not fill a gap with a guess because the questions are adding up: a guessed direction costs the user far more later than an answer costs now. Where the skill will live (project, user or plugin) and what language it is written in are among these decisions, and the location decides whether the skill must be self-contained.

**Starting from an existing skill.** Its text is evidence of decisions already made, and a failure report is evidence to trace. First separate missing knowledge from a failed execution (a missing input, a tool failure, a conflicting instruction from elsewhere, a model limit), because rewording the skill fixes only the first. Then decide whether the correction is a one-off, a recurring preference, or a counterexample to an existing principle. Know which behaviors must survive before changing anything, repair the smallest scope that explains the failure, and count deletion and consolidation as improvements.

## Writing judgment down

A principle carries its reason, the conditions under which it holds, the condition that overturns it, and a case where it decides between two similar-looking options. A slogan such as "use judgment" or "keep it user-centred" justifies opposite actions and is not a principle.

Examples narrow the space the model explores. Use one to show why two similar cases come out differently, not as a template of the right answer.

Say how the model will know the work is done, in terms it can check. Separate what must be preserved from what the model is free to vary, so a completion criterion does not become a demand for one particular output.

Put knowledge where it can act. Settled calculations and verification contracts belong in an interface such as a rubric, template, schema or script; judgment that turns on context does not become a fixed branch. A bundled CLI describes itself, with every argument explained in `--help` and closed sets offered as choices, and its output gives a cheap signal first (a summary, a size, the next action) with a handle to the costly detail, so the model decides before it pays.

Split files by when they are needed, not by length. What every path through the skill needs stays in SKILL.md; what only one branch or step needs can move to a reference, with a pointer at that branch saying when to read it ("for a crash report, read references/crash.md"). A reference nothing tells the model to open is skipped without a trace. A skill that will be packaged must carry everything it points to.

## Mechanics that fail silently

- **Adding a skill can quietly weaken another.** The descriptions of skills Claude may invoke on its own share one listing budget; a `disable-model-invocation: true` skill is not in it. When the listing overflows, Claude Code drops descriptions starting with the skills invoked least: the name stays, but the words a request would have matched are gone, so a rarely used but important skill stops being chosen with no error. Before adding a model-invoked skill, run `claude -p "/skill-doctor"` in the project it will serve to see the existing skills with their listing cost and use. A trigger shared with an existing skill is a reason to draw the boundary between them, in both descriptions or by merging the two when one job needs both bodies together.
- **`claude plugin validate` checks what you point it at.** Given a skill's own folder it validates a plugin manifest and fails with an error that says nothing about the skill. Pass the directory that contains skill folders (for a project skill, `.claude/skills`) by its real path, because symlinked entries are not read, and add `--strict`, because warnings alone still exit 0.

## When the skill is done

- The user approved the whole skill text, not a section and not by silence, or agreed on the evidence that nothing needed adding.
- The central judgments and their boundaries are written, major conflicts between principles are resolved, and what remains uncertain is stated. Approval of the text is not evidence of how the skill performs across many real sessions, so do not present it as such.
- Every difference the contrast surfaced has a place in the skill or a recorded reason for leaving it out.
- `claude plugin validate --strict` passes on the directory containing the skill.
- Code you created or changed was checked against its own contract (inputs, outputs, failures) by the means that code is verified with, and its `--help` matches what it does.
- Before you showed the draft, a reader from a different model family, where one is available, checked it against the four properties above; you weighed that review rather than adopting it wholesale, and if no such review was possible you said so and named the risk.
- Development records (sources, interview notes, rejected alternatives, the model a default-correcting line was written against) are outside the skill, in the commit body or in a handoff returned to the user. If there is no established place for them, agree on one with the user before creating a file.
