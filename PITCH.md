# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A multi-tool disruption-care agent on the Claude Messages API: a tool loop over 9 given tools plus an MCP server we connected, carrying next_available_day and fare_rules.
Does: For a stranded customer, it looks up the booking, reads live flight status and policy, presents the options it is owed, and stops for the customer's own click before anything irreversible; it escalates out-of-scope cases to a human and answers "when is the first day I can fly?"
Number: Tool-schema cost 2,421 tokens/turn before MCP, 2,927 tokens/turn after, +506 per turn for the tool the server added, counted on the wire, not sampled.
Guardrail: 0 irreversible actions without the customer's click. On K7PQ2M the agent presented rebooking and refund options and stopped on end_turn, never calling hold_seat or confirm_rebooking; every flight fact it stated came from a tool result, not from memory.
Next: Author eval cases (Build 3), add the tone gate (Build 4), and run the bench to turn tokens/turn into dollars per resolved contact and compare models on the same cases.
Still broken: An abusive message (R8KD3F) still comes back a calm, helpful answer. There is no tone gate on the way in.
Lever: <cost | speed | intelligence>

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
