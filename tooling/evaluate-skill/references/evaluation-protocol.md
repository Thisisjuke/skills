# Skill evaluation protocol

Use this reference to design cases, isolate runner and judge information,
compare baselines and report evidence. Adapt storage to the target environment;
the schema matters more than one directory convention.

## Evidence lanes

Keep these claims distinct:

| Lane | Question | Minimum evidence |
|---|---|---|
| Structural | Can a client parse and load the package contract? | Relevant validator output |
| Routing | Is the skill selected for the right prompts and avoided for nearby prompts? | Real discovery context plus invocation trace |
| Execution | Does forced use produce the advertised behavior? | Isolated output and trace against a bar |
| Incremental value | Does the skill improve on no skill or the previous version? | Comparable baseline and target runs |
| Composition | Does the selected set cooperate without conflict or hidden dependency? | Same case under explicit compositions |
| Human acceptance | Is the result useful and ready for its intended users? | Human review of artifacts and trade-offs |

Do not substitute one lane for another. A structurally valid skill may be
useless, and a good forced run says nothing about automatic discovery.

## Case record

Represent every case with equivalent fields:

```yaml
id: concise-stable-id
mode: routing | execution | conformance | composition
prompt: realistic user request
fixtures:
  - path-or-description
allowed_side_effects: read-only or exact writable scope
baseline: no-skill | previous-version | named-composition
success_bar:
  - observable outcome or qualitative dimension
failure_smells:
  - plausible but unacceptable behavior
repetitions: justified count
```

Cases should come from real requests, known misses, neighboring intents and
high-cost failure modes. Synthetic cases are acceptable when they isolate a
specific invariant, but label them and avoid replacing all realistic work with
micro-tests.

Use assertions for exact observable properties such as valid JSON, a required
file, preserved data or forbidden side effects. Use qualitative dimensions for
judgment such as usefulness, prioritization or design coherence. A qualitative
bar must still say what good achieves and what bad looks like.

## Information separation

### Execution runner

Provide:

- target skill or explicit composition;
- prompt and fixtures;
- allowed tools and side effects;
- isolated output location.

Hide:

- success bar and failure smells;
- expected answer;
- baseline artifacts;
- suspected defect and prior conversation;
- outputs from other cases or repetitions.

### Routing runner

Provide the realistic skill catalogue and user prompt. Do not name or force the
target. Record discovery metadata available to the runner and the observed
selection decision. If the client exposes no skill-selection trace, routing is
unproved rather than assumed from the final answer.

### Judge

Provide:

- artifact and relevant execution trace;
- advertised capability and case bar;
- failure smells and objective assertions;
- environment limitations.

Hide configuration identity for blind A/B comparison when practical. Ask for
pass, fail or ungradable per dimension with a quote, artifact pointer or tool
result. Do not ask the judge to “make the new version win.”

## Baselines and repetitions

Prefer:

- no-skill baseline when testing whether the skill adds value;
- immutable previous-version snapshot when testing a revision;
- named alternate composition when deciding overlap or specialization.

Keep prompts, fixtures, permissions and environment equivalent across
configurations. Randomize presentation order for qualitative A/B judgment when
possible.

Repeat when consequences are high, results are borderline or runs disagree.
Report each outcome and the observed pass count. Means and standard deviations
become meaningful only with enough samples; do not decorate two runs with
false statistical precision.

## Failure diagnosis

Use this decision order:

1. Was the target actually selected or forced as intended?
2. Did environment, permissions or tools prevent a fair run?
3. Can the output prove or disprove the assertion?
4. Is the bar testing the advertised capability rather than personal taste?
5. Does the baseline show the skill adds, removes or merely changes behavior?
6. Does the trace point to a missing, vague, excessive or misplaced
   instruction?
7. Would the proposed repair generalize beyond this case?

An always-passing assertion may test default model ability rather than skill
value. An always-failing assertion may be impossible, badly specified or
outside scope. Both are case findings before they are skill defects.

## Report template

```markdown
# Skill evaluation: <name>

## Scope

- Target/version:
- Composition:
- Baseline:
- Harness/model:
- Evidence lanes:
- Authorization:

## Results

| Case | Configuration | Runs | Verdict | Evidence |
|---|---|---:|---|---|

## Comparison

- Quality gained or lost:
- Time/token/tool cost when available:
- Routing differences:
- Variability:

## Diagnosis

- Skill defects:
- Case defects:
- Environment/configuration gaps:

## Changes and regression

- Authorized edits:
- Rerun result:
- Previously passing cases checked:

## Claim and human decision

- Strongest justified status:
- Ungraded dimensions:
- Required human decisions:
```

Persist evaluation artifacts only when the user or repository contract asks
for them. Otherwise use an isolated temporary workspace and return the concise
evidence report.
