# Evaluation

> Results will be published here when the evals run. Nothing is reported before then.

## Test sets

| Set | Size | Purpose |
|---|---|---|
| Main test set | 300 CFPB complaints (checking, credit card, mortgage) | Routing and deadline accuracy |
| High-risk set | 60 complaints containing a stop reason | Missed stop cases |
| Double-labelled | 60 cases labelled by a second reviewer | Label agreement |

All labels are set before FirstLook runs, and the test sets are frozen. A separate tuning set is used for prompt
and threshold changes, so the test sets are never tuned on.

## Headline measures

- Complaints that reached the right team with every deadline set correctly, out of 300
- High-risk complaints missed, out of 60
- Agreement between the two labellers

## What counts as a failure

- Wrong team
- Any missing or shortened regulatory deadline
- A stop case that was not stopped
- A draft that breaks an output guardrail
