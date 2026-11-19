# Client review: Larkspur disruption-care agent

**To:** Room 11  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-19 14:04

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. PITCH.md says 2,277 to 3,043 tokens for 10 tools to 12, but agent.py's tool_list() now returns build_tools() + EXTRA_TOOLS + mcp_client.tools().**

PITCH.md also says the build offers "nine tools, one tool we wrote, what_automation_will_not_do, and two tools served over MCP". The diff leaves EXTRA_TOOLS empty and LOCAL_TOOLS empty, and the static scan finds no LOCAL_TOOLS executors, so I cannot find what_automation_will_not_do in agent.py. The last readout-trace.json run called next_available_day but never called a tool by that name.

Run python3 run.py --show-tools and paste the full tool list, so the count of 12 and the origin of what_automation_will_not_do are visible.

**2. readout-trace.json shows 24111 tokens in, a cache hit ratio of None, and no cache_control anywhere in agent.py.**

The run took 6 API turns, with tools and the system prompt resent on each one: 0 tokens read from cache, 0 written. Your lever is cost, and the diff added a 329-character search_alternatives description and mcp_client.tools() to the payload that is resent every turn. A larger or smaller model changes the price of those tokens. It does not change that none of them are cached.

Run python3 bench.py --label before --stage 1 --runs 3, then again with --label after, and paste the output of python3 bench.py --compare before after.

**3. agent.py's loop appends response.content and sets answer = text_of(response) after the while, but MAX_TOOL_CALLS stays 8 with nothing after the cap.**

Your diff fixes the dropped tool_use blocks and the final-text bug. The loop condition is turns < MAX_TOOL_CALLS, so a run that reaches 8 turns exits with whatever text the last response held, possibly empty, and nothing hands over to a human. The committed trace used 6 turns for one booking. No run in the material shows a booking that gets near 8.

Run python3 run.py --all --trace and paste the totals footer with the per-booking API turn counts.

**4. evals/cases.json has two entries with id team-0101 and identical message, expect and PNR R8KD3F, authored by Hari L M and Suganthi R.**

The duplicate id means a harness keyed on id counts one case where the file shows two. Both are hard_gate true with a single judge grader and no rules grader, so no must_call or must_not_call checks them. Five other cases still carry "author": "<your name>", including tone-0101 and irrv-0101. Gate 3.1 looks for a real name on at least one case, and the two Hari and Suganthi entries are the only candidates.

Run python3 eval_harness.py and paste the case count line and the per-case verdicts.

**5. PITCH.md's Still broken line says K7PQ2M is dated May 2025 and the agent still calls check_policy and search_alternatives.**

The committed readout-trace.json shows that order: lookup_booking, get_flight_status, check_policy, search_alternatives, next_available_day. The new search_alternatives description in your diff says "Check each option's date is still in the future before offering it to the customer". Nothing in agent.py or your eval cases tests that, and a stronger model reading the same description would face the same stale booking. The gap is in what the tools return and what the loop checks, and that is the part of Priya's question a model name does not answer.

Add an eval case for K7PQ2M with a rules grader on must_not_call search_alternatives, then paste the python3 eval_harness.py result for it.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (237 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- `evals/cases.json`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
