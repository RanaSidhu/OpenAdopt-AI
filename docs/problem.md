# The Problem

## In one sentence

When a human has to approve an AI's output, nobody measures whether that human
actually catches the AI's mistakes.

## The setup

Lots of AI systems are built like this:

```
AI produces output  ->  human reviews it  ->  action happens
```

The human in the middle is the safety mechanism. It's why the system was allowed
into production at all: "don't worry, a person checks everything."

## The problem

That safety mechanism has never been tested.

We log that someone clicked "approve". We don't know if approving meant anything.
Three questions nobody can answer with data:

1. **Detection** — if the AI got it wrong, would the reviewer notice?
2. **Engagement** — is the reviewer reading the item, or clearing a queue?
3. **Graduation** — which decisions has the AI gotten right so consistently that
   the review step is pure cost?

Concretely: a reviewer approves 400 extracted invoices a day at 4 seconds each.
Are they a safety net or a rubber stamp? Today there is no way to tell them
apart, and both look identical in the logs.

## Why that costs something

- **False safety.** A reviewer who has stopped looking is worse than no
  reviewer, because everyone downstream still believes the check happened. This
  isn't hypothetical — in one study of ~17k human reviews of AI-written code,
  approval rates climbed and review comments dropped 22% as reviewers gained
  experience with the tool. The human stayed; the reviewing stopped.
- **Automation that never arrives.** Without evidence that review is redundant
  somewhere, the careful move is always to keep reviewing everything forever. So
  the AI pilot never becomes an AI system, and the savings never show up.
- **Compliance you can't actually demonstrate.** Regulation increasingly asks
  for human oversight that is *effective*. Right now organizations can only
  prove oversight *existed* — a timestamp and a click.

## Why it's worth solving

The measurable part: you can test a reviewer the same way security teams test
employees with fake phishing emails. Slip known-bad items into the review queue
and count how many get caught. That single number — detection rate — turns "we
have human oversight" into something you can put on a chart, watch over time,
and use to make a decision.

Once you can measure it, the interesting outcome isn't more oversight. It's
being able to *safely remove* oversight where the data says it's doing nothing,
and keep it where the data says it's saving you.

## What would prove this problem isn't real

Worth stating up front so we can be wrong quickly:

- Reviewers catch essentially every seeded error. Then oversight is fine and
  this project is unnecessary (still a useful thing to publish).
- Reviewers are correcting the AI constantly on every category. Then nothing can
  ever be automated further, and the "blocked automation" half of the argument
  disappears.

## Not the problem we're solving

To keep scope honest, these are explicitly somebody else's problem:

| Not this | Already handled by |
|---|---|
| Building the approval gate / HITL workflow | HumanLayer, LangGraph interrupts, custom review UIs |
| Blocking, sandboxing, or policing agent actions | Agent gateways and policy tools |
| Evaluating model quality offline | DeepEval, promptfoo, Braintrust, Ragas |
| Tracing and observability of LLM calls | Langfuse, Helicone, OpenTelemetry |
| Writing your governance policy | Consultants, NIST AI RMF, ISO 42001 |

We sit on top of whatever review step you already have and measure it. We never
intercept, block, or change what the AI does.
