# Design decisions

Every part of the agent was decided before any code was written. Each decision records the options rejected and
any dissent.

## 1. Trigger
A new complaint landing in the case store starts the run. Complaints are processed as they arrive, not in batches,
so deadlines start counting at the right moment.

## 2. Brain
A fine-tuned classifier handles the common cases. One pinned LLM is the fallback when the classifier is unsure or a
complaint covers two products. Rules and a keyword net sit beside both, so a single model failure can't hide a
high-risk case.

## 3. Instructions
Three job modules (classify, flag vulnerability, draft) are built from one shared core and shared reference files,
so a policy change is made in one place.

## 4. Tools
Data is masked. Counterparty details are fetched only when a step needs them. Routing rules live in a versioned
repository owned by the business, not in the prompt.

## 5. Memory
No live memory. Corrections from handlers go through a reviewed path before they change anything.

## 6. Control loop
A mandatory spine of seven steps, then one of five predefined paths chosen by rules.

**Six stop reasons**

| Stop reason | Goes to |
|---|---|
| Customer safety | Vulnerability specialist, same day |
| Foreclosure sale or repossession date | Mortgage specialist, same day |
| Fraud or scam in progress | Fraud team, same day |
| Attorney representation or litigation | Legal, 1 business day |
| Contact restriction or abuse | Specialist, same day |
| Issue under a regulatory matter | Regulatory Relations, 1 business day |

**Rejected:** plan-then-act (paths can't be tested exhaustively), multi-agent (more parts to run with no quality
gain), letting the agent skip steps (the skipped step will eventually be the one that mattered).

## 7. Human in the loop
A person approves every reply and takes over any stop case straight away.

## 8. Guardrails
Built around the three outcomes to prevent:
1. Silent loss of regulatory clocks across a wording pattern, for example scam victims who write "I sent it" and
   get no Reg E clock.
2. Account details disclosed to the wrong person, such as a third-party filer.
3. A missed stop case causing irreversible harm, such as a foreclosure sale date.

Output checks: schema validation, a never-say list, no unapproved amounts, no admission of fault unless the error
is confirmed.

## 9. Evals
Test sets are labelled before the agent runs, double-checked, and rerunnable from one command.

## 10. Framework
LangGraph, kept thin, so the control flow is readable and the framework can be swapped out.
