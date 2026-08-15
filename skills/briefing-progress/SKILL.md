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

**The number.** `N` = in-scope milestones in one named plan revision; `M` = those whose completion criteria are verified. Keep a unit word the reader can picture — stages, milestones, 단계 — not a bare fraction. `M` counts milestones, not a prefix: it never claims the first `M` are the finished ones. Script the tally (`grep -c '^- \[x\]'` vs `'^- \[ \]'` per section); never state a count you estimated by reading.

**One snapshot.** One plan revision, one branch, one verification time. Mixed evidence yields a number true of nothing.

**Evidence has levels — report the highest one verified, and don't mix them:**

`written → automated checks pass → wired into the product → a person can use it`
`구현됨 → 자동 검증 통과 → 앱에 배선됨 → 사용자가 쓸 수 있음`

A checked box, a commit, and a passing gate are three different claims. `git log` proves a commit exists, not that it is integrated, enabled, or usable.

**Each milestone gets a state:** done / in progress / blocked / not started / unverified — 완료 / 진행 중 / 막힘 / 시작 안 함 / 미검증. Only *done* counts toward `M`. Never promote partial work to finished to tidy a table.

**When you cannot count** — no authoritative plan, artifacts disagreeing, no gate, failing gate, dirty tree, work split across branches — say so. "There is no basis here for an exact milestone count" beats a confident wrong number.

## The Output Contract

Produce these seven parts, in this order, **and nothing else** — no appendix, no technical-detail section.

1. **Position line.** `M` of `N` milestones complete — "전체 N단계 중 M단계 완료." Its own sentence, and the first thing in the reply: no subject line, no greeting, no preamble, and no other number ahead of it.
2. **One paragraph: what this work is for.** Reuse only words from this conversation or the product's own user-visible text.
3. **Finished table.** One row per milestone, one sentence, ≤15 words, stating **what that milestone made true**. Group consecutive milestones sharing one outcome; past ~8 rows group by phase and offer the detail only if asked. When `M` is 0 the table still appears, as exactly one row: first cell `—`, second cell saying no milestone has met its completion criteria. Not a paragraph instead, not omitted.
4. **Reality line.** The highest verified evidence level, and what the reader can use right now. When nothing is usable yet, say exactly that.
5. **Remaining table.** Same shape, plus each item's state. Mark where it first becomes visible to a user.
6. **Caveats.** Include (a) every milestone whose recorded plan no longer matches what was built, and (b) every milestone you cannot execute yourself — real-device testing, store submission, anything needing credentials. Write it as a requirement, not an assignment ("사람이 기기를 직접 만져야 합니다"); name an owner only where one is recorded. Never turn "I cannot do this" into "you must do this".
7. **Next step.** One recommendation. Ask a decision only when work is truly blocked without it — then give the consequence in the reader's terms, a default, and what proceeds regardless. No blocker, no question.

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

The code is written and the automated checks pass. Nobody has opened one of these
files in the finance team's own tool yet, so "it actually works for them" is not
settled.

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

코드는 다 됐고 자동 테스트도 통과합니다. 다만 실제 기기 확인이 8개 중 3개만
끝나서, "실제로 잘 된다"까지는 아직 확정이 아닙니다.

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
| Giving a progress figure from memory | Count the boxes and use that number |
| Reading a commit as a finished milestone | Claim only the evidence level you verified |
| Forcing partial work into done or not-done | Mark it in progress or unverified |
| Ending on "it's all finished" | Say separately what can be used right now |
| Reading "explain it again" as "you decide" | Leave the decision; re-explain more plainly |
| Turning what you cannot do into the reader's job | State it as a requirement; name an owner only if one is recorded |
