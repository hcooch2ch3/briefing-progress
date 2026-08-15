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

- **Count, don't estimate.** The number comes from a scripted tally of the plan, never from reading.
- **Evidence has levels** — `구현됨 → 자동 검증 통과 → 앱에 배선됨 → 사용자가 쓸 수 있음`.
  A commit proves a commit exists. It does not prove the feature is wired, enabled, or usable.
- **Only verified milestones count.** Partial work is 진행 중 or 미검증, never rounded up.
- **When you cannot count, say so.** "정확한 단계 수를 확인할 기준이 없습니다" beats a wrong number.
- **A repeated "explain it again" means explain it simpler — it does not transfer the decision.**
- **Never turn "I cannot do this" into "you must do this."**

## What it is not

- Not a code review — it reports where things stand, not whether they are any good.
- Not for a single technical question about one file or one bug.
- Not an agent-to-agent handoff document. The reader is a human, often a non-specialist.

## Measured behavior

Tested against a fixture repo designed to look further along than it was: a plan whose
top-level checkboxes said 4 of 6 done, while a nested sub-item was open, two tests failed,
and a finished feature sat behind a disabled flag. Ground truth: 0 of 6 complete.

| | without the skill | with the skill |
|---|---|---|
| Opens with the position line | 0 / 2 | 10 / 10 |
| Honors "one line, no tables, I'm late" | 0 / 1 | 5 / 5 |
| Same output shape across repeats | 2 shapes / 2 runs | 1 shape / 5 runs |
| Avoids all three traps | 3 / 3 | 10 / 10 |

Both arms avoided overclaiming — a capable model already resists that. What the skill
reliably adds is **shape**: the denominator is always stated, the usable-right-now line is
always there, and the answer looks the same every time you ask.

## Install

Copy the directory into your skills folder:

```
~/.claude/skills/briefing-progress/     # Claude Code
~/.agents/skills/briefing-progress/     # cross-runtime alias
```

The agent loads it on its own when someone asks where things stand.

## Language

The briefing is written in whatever language the conversation is in. Section labels in the
skill are semantic, not literal strings to copy. The reference vocabulary — the evidence
ladder and the five milestone states — is written in Korean; English equivalents are
`implemented → automated checks pass → wired into the app → a user can use it` and
`done / in progress / blocked / not started / unverified`.

## License

MIT. See [LICENSE](LICENSE).
