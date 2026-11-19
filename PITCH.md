# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A support agent for disrupted Larkspur flights. It runs Claude in a loop (capped at 8 tool calls) over Larkspur's nine tools, one tool we wrote, what_automation_will_not_do, and two tools served over MCP, fare_rules and next_available_day.
Does: For a customer whose flight is disrupted, it finds the booking, checks the flight's status, checks what the policy says we owe, and explains it in plain words with the rebooking options. When the customer asks for something people handle, like a refund, it says so and hands over to Customer Care with a summary.
Number: Tool schemas sent on every turn went from 2,277 to 3,043 tokens (10 tools to 12), per turn, counted by run.py --tool-tax. Needs a before/after from bench.py before we claim it.
Safety check: It never issues a refund itself; it sends the request to the refunds queue for a person. We saw it in the trace for K7PQ2M: escalate_to_human returned ESC-7B98CD and no voucher or rebooking was made.
Next: Sharpen the tool descriptions so our tool and fare_rules stop overlapping on the same handbook text, then measure the token cost again.
Still broken: The booking K7PQ2M is dated May 2025 and today is November 2026. The agent notices, but still looks up policy and lists rebooking options for a flight that has already gone, instead of stopping to ask whether it is the right trip.
Lever: cost

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
