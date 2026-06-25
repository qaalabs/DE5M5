# What does "Production-Ready" Mean?

## How to run this discussion

Don't open with a definition. Ask first:

> "Before I tell you what I think - what do *you* think makes code production-ready?"

Take 3-4 answers from the group, acknowledge each one. Then land on the six criteria:

## The six criteria

| Criterion | Why it matters |
|-----------|---------------|
| Tested (>70% coverage) | You know it works now, and you'll know when it breaks later |
| Version controlled | Full history, ability to roll back, team can collaborate |
| Documented | Someone else can understand and run it without you being there |
| Automated deployment | No manual steps between "code merged" and "running in prod" |
| Error handling | Fails gracefully and tells you *why*, not just crashes silently |
| Security considered | Data access controls, no credentials in code |

## Assessment link

These aren't just good practice - they're what gets marked. The pipeline being functionally correct is necessary but not sufficient. Point them to the rubric now so they're not surprised on Day 4.

## Common discussion point

Learners often think "it runs without errors" = production-ready. The counter-question: "How do you know it handles bad data? How do you know it still works after your next change?"
