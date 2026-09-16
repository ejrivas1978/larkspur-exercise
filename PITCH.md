# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A multi-tool disruption-care agent on the Claude Messages API: a tool loop over 9 given tools plus an MCP server we connected, carrying next_available_day and fare_rules.
Does: For a stranded customer, it looks up the booking, reads live flight status and policy, presents the options it is owed, and stops for the customer's own click before anything irreversible; it escalates out-of-scope cases to a human and answers "when is the first day I can fly?"
Number: Tool-schema cost 2,421 tokens/turn before MCP, 2,927 tokens/turn after, +506 per turn for the tool the server added, counted on the wire, not sampled.
Guardrail: 0 irreversible actions without the customer's click. On K7PQ2M the agent presented rebooking and refund options and stopped on end_turn, never calling hold_seat or confirm_rebooking; every flight fact it stated came from a tool result, not from memory.
Next: Author eval cases (Build 3), add the tone gate (Build 4), and run the bench to turn tokens/turn into dollars per resolved contact and compare models on the same cases.
Still broken: On an ambiguous missed connection (case ambg-0101), the agent assumes the destination and offers rebooking without first asking which segment was missed — an irreversible path built on a guess, and its eval still fails.
Lever: intelligence

## Priya asked

Costs: ~$0.059 model cost per resolved contact (bench, stage 1, 3 runs/shape) against $6.90 for a human chat. This is model cost, not loaded — infrastructure and evals add roughly 40% (Larkspur's loaded number was ~$0.14). Priced 2026-09-16.
Wrong: The first failure is ambiguity, not invention. On an ambiguous missed connection (case ambg-0101) it assumes the destination and offers rebooking without asking which segment was missed — an irreversible path on a guess. It does not fabricate flight facts (grnd-0101 passes): every flight statement comes from a tool result.
Runs it: The client's platform/ops team. The MCP-served tools (next_available_day, fare_rules) are owned and versioned by them, the eval suite (evals/cases.json) runs as the regression guard, and human escalation keeps a person on out-of-scope, refunds, and legal-threat cases.
Left out: Voice and everything non-chat (59% of contacts), plus refunds, group changes, and unaccompanied minors — all escalated to a human by design. Humans still take ~42% of in-scope chats. Not proven yet: accuracy at volume, storm-day concurrency, and the loaded cost.
