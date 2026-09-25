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

A new skill also costs the ones already there, because the skills Claude may pick on its own compete for one listing. Before adding one, run `claude -p "/skill-doctor"` in the project it will serve to see its neighbors; where one shares a trigger with the new skill, draw the boundary in both descriptions, or merge the two when one job needs both bodies together.

## Drawing out what the user knows

You are after the working parts of judgment rather than a list of beliefs: what the expert notices first, what they tell apart that a newcomer lumps together, and what would make them decide the other way. Where two of their principles pull against each other, the condition that settles it is part of the knowledge.

Read before you ask. The user's past outputs, documents, session transcripts, an existing skill and its failure reports answer much of what you would otherwise ask, and making the user recite what you could read spends their attention twice.

**Contrast with the default.** Experts rarely articulate tacit knowledge on request, but they recognize a wrong answer at once. Take representative tasks from the user's real work, have a Claude with no skills or instructions attempt them, show the user the result, and follow each objection down to its reason. Run it in a fresh empty directory holding copies of only the inputs the task needs:

```bash
claude -p --safe-mode --restricted --permission-mode acceptEdits --model <model the skill will serve> --output-format json "<task>"
```

A temporary directory alone is not isolation: without these flags the child runs under the user's permission mode and can write anywhere. If the task needs a shell, naming it back with `--tools` leaves that shell unconfined.

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

Put knowledge where it can act. Settled calculations and verification contracts belong in an interface such as a rubric, template, schema or script; judgment that turns on context does not become a fixed branch.

Split files by when they are needed, not by length. What every path through the skill needs stays in SKILL.md; what only one branch or step needs can move to a reference, with a pointer at that branch saying when to read it ("for a crash report, read references/crash.md"). A reference nothing tells the model to open is skipped without a trace. A skill that will be packaged must carry everything it points to.

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

## When the skill is done

- The user approved the whole skill text, not a section and not by silence, or agreed on the evidence that nothing needed adding.
- The central judgments and their boundaries are written, major conflicts between principles are resolved, and what remains uncertain is stated. Approval of the text is not evidence of how the skill performs across many real sessions, so do not present it as such.
- Every difference the contrast surfaced has a place in the skill or a recorded reason for leaving it out.
- `claude plugin validate --strict` exits 0 on the directory that actually contains the skill's folder: resolve a symlinked skill to its target first and pass that target's parent (for a project skill, `.claude/skills`), never the skill's own folder.
- Code you created or changed was checked against its own contract (inputs, outputs, failures) by the means that code is verified with, and its `--help` matches what it does; new code, and code migrated with the user's agreement, also passes the structure test.
- Before you showed the draft, a reader from a different model family, where one is available, checked it against the four properties above; you weighed that review rather than adopting it wholesale, and if no such review was possible you said so and named the risk.
- Development records (sources, interview notes, rejected alternatives, the model a default-correcting line was written against) are outside the skill, in the commit body or in a handoff returned to the user. If there is no established place for them, agree on one with the user before creating a file.
