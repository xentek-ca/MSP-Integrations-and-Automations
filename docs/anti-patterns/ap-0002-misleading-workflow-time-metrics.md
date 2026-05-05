# AP-0002: Workflow-level time-saved metrics are misleading

**Pattern category:** platform-ops

## What we tried
Used the orchestration platform's built-in "time saved" metric as a proxy for automation value. Each workflow declared an estimated time saved per execution; the platform multiplied it by run count and rolled it up to a dashboard.

## Why it didn't work
The number was overinflated and didn't reflect actual operational efficiency. Causes were structural:

- A single workflow execution often did the work of *part* of a manual task, not the whole task. Crediting the full per-execution time inflates the savings.
- Many workflow executions are no-ops (conditions didn't match, queue was empty). Counting them at full credit is wrong.
- Runs that error out partway through still got partial credit because the platform only knew the workflow started, not what actually happened.
- Different platforms compute the metric differently, so cross-orchestrator comparisons were meaningless.

The number ended up being big and persuasive and approximately disconnected from reality, which is the worst combination — leadership relied on it, and the operational team knew better than to.

## What we did instead
Built a task-based time-saving metric. Each meaningful unit of work *inside* a workflow gets a per-task estimate; the metric is computed from the actual operations performed, not from a coarse per-execution stamp. No-op runs contribute zero. Errored runs contribute only what completed before the error.

The headline number dropped substantially. It's also defensible.

## Lesson
Don't trust vendor-supplied measurement of your own work. If a metric matters — for executive reporting, for a quarterly business review, for your own roadmap — own the definition and the calculation. Otherwise the orchestration platform is implicitly telling your story for you, and the story is calibrated to make the platform look good, not to make your decisions correct.

A good rule: any metric you'd cite in a customer-facing or board-facing context should have a calculation you can defend in writing. If you can't reconstruct the number from first principles, it's not yours.

## Related patterns
- *(future)* Workflow-run observability and structured logging — the substrate that makes task-level metrics possible.
