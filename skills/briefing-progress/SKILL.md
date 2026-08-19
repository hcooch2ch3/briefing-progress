---
name: briefing-progress
description: Use when someone asks where a multi-step effort stands — what is finished, what is left — especially a non-specialist stakeholder. Fires on "브리핑", "지금까지 한 일", "어디까지 됐어", "남은 일", "진행 상황", "전체 진행", "status", "progress", "where are we". Not for a technical question about one file or one bug.
license: MIT
---

# Briefing Progress

## Overview

A status briefing for a person, not a changelog for a machine. Two failure modes to beat at once: unreadable (jargon, technical titles, buried point) and untrue (numbers from memory, "다 됐습니다" for something nobody can use).

**Not for:** a specific technical question (answer that question), a single small task, or a judgment about whether the work is any good.

## Count Before Any Status Claim — Required

**The number.** `N` = in-scope milestones in one named plan revision; `M` = those whose completion criteria are verified. Keep a unit word the reader can picture — stages, milestones, 단계 — not a bare fraction. `M` counts milestones, not a prefix: it never claims the first `M` are the finished ones. **Find the plan first, then script the tally against it** — `PLAN` below is the file you found:

- `N`, every top-level box — `grep -cE '^[-*+] \[.\] ' "$PLAN"`
- candidates for done — `grep -cE '^[-*+] \[[xX]\] ' "$PLAN"`

`\[.\]` takes whatever marker the plan uses, and a marker other than `[x]`, `[X]` or `[ ]` means
what the plan says it means: in scope and not done where it marks progress, outside `N` where it
marks something dropped. Where the plan never says, the number is not settled — say which reading
you took, in the same breath as the number. Only `[x]` or `[X]` is even a candidate for done.

**Read `grep`'s exit code; never write `|| true`** — it reports every case below as the same
`0`. From the done count, 1 means `M = 0`, and part 3 says what to do with that. From the box
count, 1 means the file carries no boxes and 2 means you named the wrong file; both send you to
*When you cannot count*, because a project having no plan anywhere is something you establish by
looking, never by a failed `grep`.

**Both counts are candidates, not answers.** The pattern counts lines; a plan is not a line
format. Open it and reconcile. Brackets that are not milestones come out — a citation `[1]`, a
checklist written at column 0 under a heading, anything in a fenced block where the plan
documented its own format. A milestone the pattern missed goes in only when it carries a box of
its own, `1. [x]` rather than `- [x]`; anything with no box at all stays out, because supplying
it yourself is inventing the denominator. Keep the `^` anchor while you reconcile: an indented
sub-item is not a milestone, and counting one as such is how a half-done milestone reads as
finished. If the two readings will not settle, go to *When you cannot count*.

**`M` is narrower still**: the done candidates whose completion criteria you then confirmed, and
confirming them means looking at the evidence — that is required here, not the estimating this
rule forbids. What is forbidden is a count from memory or from what you recall of the
conversation.

**One snapshot.** One plan revision, one branch, one verification time. Mixed evidence yields a number true of nothing.

**Evidence has levels.** Each milestone's own level is the highest rung verified *for that
milestone*, and no rung is claimed for a milestone that has not reached it:

`written → automated checks pass → wired into the product → a person can use it`
`구현됨 → 자동 검증 통과 → 앱에 배선됨 → 사용자가 쓸 수 있음`

A checked box, a commit, and a passing gate are three different claims. `git log` proves a commit exists, not that it is integrated, enabled, or usable.

**Each milestone gets a state:** done / in progress / blocked / not started / unverified — 완료 / 진행 중 / 막힘 / 시작 안 함 / 미검증. Only *done* counts toward `M`. Never promote partial work to finished to tidy a table.

**Where the bar sits.** If the plan states its own completion criteria, those are the bar — use them even when they are stricter than the ladder. If it states none, the bar is `wired into the product`: the change is reachable in the thing people actually run. Name which of the two you applied, in one clause. A milestone resting at `written` or `automated checks pass` is 미검증 no matter how many commits it took, and the rung above the bar belongs in part 4, not in `M`.

**When you cannot count** — no authoritative plan, artifacts disagreeing, no gate, failing gate, dirty tree, work split across branches — say so. "There is no basis here for an exact milestone count" beats a confident wrong number.

**When nothing names the milestones** — no plan file, no tracker, nothing that says what the whole job is — you still owe the reader everything except the number. Part 1 becomes one sentence on why no number is available. In parts 3 and 5 the first column is the work area in plain words, not a milestone number. Part 6(a) becomes what writing the plan down would buy them. The rest of the contract is unchanged, and a denominator inferred from commits or from the shape of the code is still invented — don't.

## The Output Contract

Produce these seven parts, in this order, **and nothing else** — no appendix, no technical-detail section. Parts 5 and 6 drop out when they would be empty, on the terms stated there; the rest always appear, except under *If the reader asked for less* below.

1. **Position line.** `M` of `N` milestones complete — "전체 N단계 중 M단계 완료." Its own sentence, and the first thing in the reply: no subject line, no greeting, no preamble, and no other number ahead of it.
2. **One paragraph: what this work is for.** Reuse only words from this conversation or the product's own user-visible text.
3. **Finished table.** One row per milestone, one sentence, ≤15 words, stating **what that milestone made true**. Group consecutive milestones sharing one outcome; past ~8 rows group by phase and offer the detail only if asked. When `M` is 0 the table still appears, as exactly one row: first cell `—`, second cell saying no milestone has met its completion criteria. Not a paragraph instead, not omitted.
4. **Reality line.** The evidence level **every** in-scope milestone has reached — the floor across the set, not the ceiling — and what the reader can use right now. One milestone standing a rung higher does not lift this line; name that one separately if it matters. When nothing is usable yet, say exactly that.
5. **Remaining table.** Same shape, plus each item's state. Mark where it first becomes visible to a user. When nothing remains, drop this part — part 1 already said so, and a table of nothing reads as an omission.
6. **Caveats.** Include (a) every milestone whose recorded plan no longer matches what was built, and (b) every milestone you cannot execute yourself — real-device testing, store submission, anything needing credentials. Write it as a requirement, not an assignment ("사람이 기기를 직접 만져야 합니다"); name an owner only where one is recorded. Never turn "I cannot do this" into "you must do this". When neither (a) nor (b) has anything in it, drop this part. A risk invented to fill the space is the exact failure this contract exists to prevent.
7. **Next step.** One recommendation — when the work is finished, that is what to do with it (hand it over, close it out), never an invented next task. Ask a decision only when work is truly blocked without it — then give the consequence in the reader's terms, a default, and what proceeds regardless. No blocker, no question.

**If the reader asked for less** — a line, a sentence, no tables, or they said they are out of time — the briefing is part 1 followed by part 4, then one clause offering the rest. Parts 2, 3, 5, 6, 7 are dropped, not compressed into the same reply. The position line still comes first and `M` is still the verified count: a short answer is shorter, never rounder.

## Language

Match the conversation's language, headings included — every label here is semantic, not literal text to copy.

**The inclusion rule:** every noun in the briefing is either a word the reader has already used or a plain description of an effect. That covers code names, file paths, and type names — and equally internal process vocabulary: review tiers, severity labels, layer names, milestone codes, invariants. Term not in the reader's own words? Translate it or drop it.

## When Asked to Explain Again

A repeated request to explain means **make the explanation simpler. It does not transfer the decision.**

Re-explain in plainer words, as many times as it takes. Not understanding is not delegation: leave the decision with the reader, and do not execute the specific change that decision governs until they decide it. Decide on their behalf only when they say to.

## Examples

Shape transfers; the wording is each project's own. Write the briefing in the reader's
language — these two differ only in that.

### English

````
4 of 7 milestones complete.

Making the export button produce a file the finance team can open in their own
spreadsheet tool, instead of the raw dump it produces today.

## Done
| Milestone | What it made true |
|---|---|
| 1–2 ✅ | Export now writes real column headers and dates finance can read |
| 3 ✅ | Rows over the size limit split into separate files instead of failing |
| 4 ✅ | A failed export tells you which row broke it |

The plan sets no finish line of its own, so the bar here is the default one — the change
has to be reachable in what people actually run. Taken as a whole this is not usable yet:
milestones 6 and 7 have not been started, so the export finance actually runs is still the
old one. Milestones 1–4 are wired into
the new export, but nobody has opened one of those files in the finance team's own
tool.

## Remaining
| Milestone | State | What is left |
|---|---|---|
| 5 | In progress | Currency columns still export as plain text — this is the first thing finance would notice |
| 6 | Not started | Scheduled exports run on the old path |
| 7 | Not started | Turning it on for everyone |

## Worth knowing
Milestone 5 needs someone with a finance account to open a real export and confirm
the numbers. I cannot do that part. No owner is named in the plan.

## Next step
I would fix the currency columns next. Nothing for you to decide.
````

### 한국어

````
전체 6단계 중 5단계 완료.

앱 메뉴에서 언어를 바꾸면, 앱 안에 들어 있는 블록 편집기 화면의 언어도 같이
바뀌게 하는 작업입니다.

## 끝난 것
| 단계 | 무엇이 되었나 |
|---|---|
| 1–2 ✅ | 어떤 언어로 맞출지 계산하고, 편집기가 열릴 때 그 언어를 심어줌 |
| 3–4 ✅ | 로봇 연결 중 언어를 바꾸면 먼저 물어보는 확인 창 |
| 5 ✅ | 쓰는 도중 언어를 바꿔도 편집기가 같은 언어로 다시 열림 |

계획서에 따로 완료 기준이 없어서 기본 기준인 "앱에 배선돼 실제로 돌아간다"를 적용했습니다.
다섯 단계는 앱에 들어가 자동 테스트까지 통과했습니다. 마지막 6단계는 실제 기기 확인이
8개 중 3개만 끝나서, "사용자가 실제로 쓸 수 있다"까지는 아직 확정이 아닙니다.

## 남은 것
| 단계 | 상태 | 남은 일 |
|---|---|---|
| 6 | 진행 중 (8개 중 3개) | 로봇 연결 중 가드 · 제3언어 기기 · 편집 내용 보존 · 다시 열기 실패 시 탈출 |

## 알아두실 것
남은 확인 5가지는 사람이 기기를 직접 만져야 하는 일이라 제가 못 합니다. 담당은
계획서에 지정돼 있지 않습니다.

## 다음 걸음
확인 5가지를 체크리스트로 뽑아드리는 게 좋겠습니다. 결정하실 건 없습니다.
````

## Common Mistakes

| Mistake | Fix |
|---|---|
| Giving a progress figure from memory | Script the tally, then check each box against the plan's criteria |
| Reading a commit as a finished milestone | Claim only the evidence level you verified |
| Forcing partial work into done or not-done | Mark it in progress or unverified |
| Ending on "it's all finished" | Say separately what can be used right now |
| Reading "explain it again" as "you decide" | Leave the decision; re-explain more plainly |
| Turning what you cannot do into the reader's job | State it as a requirement; name an owner only if one is recorded |
