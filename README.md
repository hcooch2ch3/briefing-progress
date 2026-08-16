# briefing-progress

An Agent Skill that makes a coding agent report progress in a fixed, checkable shape —
for a person who has to repeat it to someone else, not for a machine reading a changelog.

## What it actually does

Ask an agent "어디까지 됐어?" / "where are we?" and you usually get one of two answers:
a wall of commit titles, or a confident number nobody counted. This skill replaces both
with a seven-part contract that always opens on the same line:

```
전체 6단계 중 0단계 완료.
```

…followed by what the work is for, what is finished, **what you can actually use right now**,
what remains with a state per item, what nobody can do without a human, and one next step.

The rules that do the work:

- **Count, don't estimate.** Where a plan exists, the number comes from a scripted tally of it —
  never from reading and guessing. Where none exists, there is no number, and it says so.
- **Evidence has levels** — `구현됨 → 자동 검증 통과 → 앱에 배선됨 → 사용자가 쓸 수 있음`.
  A commit proves a commit exists. It does not prove the feature is wired, enabled, or usable.
- **Only verified milestones count.** Partial work is 진행 중 or 미검증, never rounded up.
- **When you cannot count, say so.** "정확한 단계 수를 확인할 기준이 없습니다" beats a wrong number.
- **A repeated "explain it again" means explain it simpler — it does not transfer the decision.**
- **Never turn "I cannot do this" into "you must do this."**

## With no plan document

Most repos have no milestone list to tally. That case is supported, and it is not a
degraded one — only three things change:

| | with a plan | without |
|---|---|---|
| Opening line | `전체 6단계 중 0단계 완료.` | one sentence on why no number is available |
| First table column | milestone numbers | work areas — "reading amounts", "finding the total" |
| Caveats | where the plan and the build disagree | that a plan would buy you the number |

Everything else is unchanged: what is finished, what is usable now, what remains with a
state each, and one next step. Two side effects are arguably improvements — remaining items
come out named rather than numbered, which reads better to a non-specialist, and they are
derived from the code, so gaps no plan ever listed still surface.

The trade is real, though: with no denominator there is no week-over-week figure. If a
recurring report needs one, the briefing will tell you to write the plan down first.

Measured over six runs on a repo with no plan file and no plan in its history, including one
where the reader pushed for a percentage anyway: zero invented denominators.

## What it is not

- Not a code review — it reports where things stand, not whether they are any good.
- Not for a single technical question about one file or one bug.
- Not an agent-to-agent handoff document. The reader is a human, often a non-specialist.

## Measured behavior

Tested against a fixture repo designed to look further along than it was: a plan whose
top-level checkboxes said 4 of 6 done, while a nested sub-item was open, two tests failed,
and a finished feature sat behind a disabled flag. Ground truth: 0 of 6 complete.

Numbers below are from six runs against the version in this repo — three in English, three
in Korean, two of them told "I'm pasting this straight into an email" — plus two baseline
runs with no skill loaded.

| | without | with |
|---|---|---|
| Overclaimed — called a trapped milestone done | 0 / 2 | 0 / 6 |
| Stated the denominator at all | 0 / 2 | 6 / 6 |
| Led with the position line, nothing ahead of it | 0 / 2 | 6 / 6 |
| Kept jargon and file names out | not measured | 6 / 6 |
| Honored "one line, no tables, I'm late" | 0 / 1 | 2 / 2 |
| Empty-state row when nothing is finished | — | 4 / 4 |

Neither arm overclaimed — a capable model already resists that on its own, and this skill
is not sold as a fix for it. What it reliably adds is **shape**: the denominator gets stated,
the usable-right-now line is always there, and the answer arrives in the same form each time.

Those six runs are the tail of a longer loop. Seventeen earlier runs against earlier drafts
drove three fixes that the current wording carries: a full briefing was being returned to
someone who had asked for one line; the finished table took two different forms when nothing
was finished; and a first attempt at tightening the position line loosened it enough that one
English run opened with a `Subject:` header instead. Each fix was re-measured before the next
change. Two things still vary between runs and are not claimed as fixed: whether the reality
line gets its own heading, and which milestone gets marked as the first one a user could see.

A separate regression on a healthier repo — real milestones genuinely finished — confirmed the
empty-state row does not misfire when `M` is greater than zero (4 / 4).

## Install

```bash
git clone https://github.com/hcooch2ch3/briefing-progress.git ~/.claude/skills/briefing-progress-tmp
mv ~/.claude/skills/briefing-progress-tmp/skills/briefing-progress ~/.claude/skills/
rm -rf ~/.claude/skills/briefing-progress-tmp
```

Or place `skills/briefing-progress/SKILL.md` at `~/.claude/skills/briefing-progress/SKILL.md`
manually. Runtimes other than Claude Code read `~/.agents/skills/briefing-progress/` instead.

Nothing else to configure — the agent loads it on its own when someone asks where things stand.

## Language

The briefing is written in whatever language the conversation is in. Section labels in the
skill are semantic, not literal strings to copy. The reference vocabulary — the evidence
ladder and the five milestone states — is written in Korean; English equivalents are
`implemented → automated checks pass → wired into the app → a user can use it` and
`done / in progress / blocked / not started / unverified`.

## License

MIT. See [LICENSE](LICENSE).
