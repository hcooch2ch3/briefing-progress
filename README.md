# briefing-progress

An Agent Skill that makes a coding agent report progress in a fixed, checkable shape —
for a person who has to repeat it to someone else, not for a machine reading a changelog.

## What it actually does

Ask an agent "어디까지 됐어?" / "where are we?" and you usually get one of two answers:
a wall of commit titles, or a confident number nobody counted. This skill replaces both
with a seven-part contract that opens on a counted line — or, where there is nothing to
count, on one sentence saying why:

```
0 of 6 milestones complete.
전체 6단계 중 0단계 완료.
```

The briefing comes out in whatever language the conversation is in; the two lines above are
the same opening. After it: what the work is for, what is finished, **what you can actually
use right now**, what remains with a state per item, what nobody can do without a human, and
one next step.

The rules that do the work:

- **Count, don't estimate.** Where a plan exists, the number starts from a scripted tally and is
  reconciled against the plan itself; each box is then checked against the plan's own completion
  criteria, or against "wired into the product" where the plan states none. What is ruled out is
  a figure from memory. Where no plan exists, there is no number, and it says so.
- **Evidence has levels** — written → automated checks pass → wired into the product → a person
  can use it. A commit proves a commit exists. It does not prove the feature is wired, enabled,
  or usable.
- **Only verified milestones count.** Partial work is *in progress* or *unverified*, never
  rounded up.
- **When you cannot count, say so.** "There is no basis here for an exact milestone count"
  beats a wrong number.
- **A repeated "explain it again" means explain it simpler — it does not transfer the decision.**
- **Never turn "I cannot do this" into "you must do this."**

## With no plan document

Most repos have no milestone list to tally. That case is handled rather than refused — three
things change:

| | with a plan | without |
|---|---|---|
| Opening line | `0 of 6 milestones complete.` | one sentence on why no number is available |
| First table column | milestone numbers | work areas — "reading amounts", "finding the total" |
| Caveats | where the plan and the build disagree | that a plan would buy you the number |

Everything else stays: what is finished, what is usable now, what remains with a state each,
and one next step. Remaining items come out named rather than numbered, and they are read off
the code, so an item no plan ever listed can still show up.

The trade is real: with no denominator there is no week-over-week figure. If a recurring
report needs one, the briefing will tell you to write the plan down first.

This path was exercised by hand on a repo with no plan file and none in its history, including
a reader who pushed for a percentage anyway. No run invented a denominator. Same caveat as
below — development notes, not measurement.

## What it is not

- Not a code review — it reports where things stand, not whether they are any good.
- Not for a single technical question about one file or one bug.
- Not an agent-to-agent handoff document. The reader is a human, often a non-specialist.

## What was tried, and what it does not prove

Developed against fixture repos built to look further along than they were — a plan whose
top-level checkboxes said 4 of 6 done while a nested sub-item was open, two tests failed, and
a finished feature sat behind a disabled flag. Runs were done by hand, in Korean and English,
including readers who asked for one line and readers with no plan file at all. Findings drove
several rewrites: a full briefing was being returned to someone who had asked for one line,
the finished table took two different shapes when nothing was finished, and one attempt at
tightening the opening line loosened it enough that a run led with a `Subject:` header.

What held up across those runs: the briefing opened on the position line, said what was usable
right now, and did not read a checked box as a finished milestone.

**What this does not establish.** The control arm was two runs, so it cannot separate "the
skill did this" from "a capable model does this anyway" — and the one thing it did show is
that the baseline did not overclaim either. This skill is not sold as a fix for overclaiming.
Runs were scored by the author against the author's own output, unblinded, with no rubric
published here. No transcripts, fixtures, or model versions are committed to this repo, so
none of it is reproducible by you. Treat the above as development notes, not measurement.

Two things vary between runs and are not claimed as fixed: whether the reality line gets its
own heading, and which milestone gets marked as the first one a user could see.

## Install

```bash
rm -rf /tmp/briefing-progress-tmp \
  && git clone https://github.com/hcooch2ch3/briefing-progress.git /tmp/briefing-progress-tmp \
  && mkdir -p ~/.claude/skills \
  && rm -rf ~/.claude/skills/briefing-progress \
  && mv /tmp/briefing-progress-tmp/skills/briefing-progress ~/.claude/skills/ \
  && rm -rf /tmp/briefing-progress-tmp
```

The steps are chained, and the old copy is removed before the new one moves in. Run the
same block to upgrade: without the removal, `mv` fails on the existing directory and leaves
you on the old version.

Or place `skills/briefing-progress/SKILL.md` at `~/.claude/skills/briefing-progress/SKILL.md`
manually. Runtimes other than Claude Code read `~/.agents/skills/briefing-progress/` instead.

Nothing else to configure — the agent loads it on its own when someone asks where things stand.

## Language

The briefing is written in whatever language the conversation is in. Section labels in the
skill are semantic, not literal strings to copy. The reference vocabulary is written
English-first with a Korean equivalent beside it. The evidence ladder is
`written → automated checks pass → wired into the product → a person can use it`
(`구현됨 → 자동 검증 통과 → 앱에 배선됨 → 사용자가 쓸 수 있음`), and the five milestone
states are `done / in progress / blocked / not started / unverified`
(`완료 / 진행 중 / 막힘 / 시작 안 함 / 미검증`).

## License

MIT. See [LICENSE](LICENSE).
