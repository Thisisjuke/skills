---
name: java-me-testing
description: Test Java ME behavior across target-compatible VMs, emulators, and physical devices, including MIDlet lifecycle, LCDUI/input, RMS, GCF, permissions, and exact-artifact compatibility.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation; CALLS WHEN NEEDED java-me-build"
---

# Java ME Testing

Test Java ME behavior at the weakest evidence lane that can prove the claim,
then escalate when behavior depends on the VM, application manager, display,
input, storage, permissions, network or physical device. Keep every result tied
to the exact runtime profile and artifact exercised.

This skill owns Java ME target-VM, emulator and device-testing mechanics. It
does not own technology-neutral test design, Java framework mechanics, static
JAR/JAD construction, generic performance experiments or broad platform rules.

## Define the claim and target

Before selecting a lane, establish the relevant contract:

- configuration, profile and source/class-file level;
- mandatory and optional APIs, manufacturer APIs and permissions;
- MIDlet variant, entry point and exact JAR/JAD or diagnostic suite;
- emulator or VM implementation and its configured runtime profile;
- device model, firmware and Java runtime when physical behavior matters;
- clean, persisted, upgraded, corrupted or capacity-constrained state;
- display size, keypad/pointer capabilities, locale and network conditions;
- whether external accounts, services or device mutation are authorized.

State the behavior to prove and the evidence level it requires. A test that
passes on an idealized VM does not establish a vendor quirk, and a device result
does not automatically generalize beyond the named model, firmware and runtime.

## Use an explicit evidence ladder

Keep the lanes and claims separate:

| Lane | Good evidence for | Does not establish |
|---|---|---|
| Host-side deterministic test | Pure algorithms, parsers, serialization and differential oracles | CLDC linkage, target bytecode, AMS, LCDUI, RMS, GCF or device limits |
| Target-compatible VM | Target class execution, bounded probes, runtime semantics exposed by that VM | Another VM/vendor, physical input, radio, installability or device resources |
| Emulator integration | Configured AMS, UI, input, permissions, RMS, media or network behavior of that emulator | Physical-device timing, heap, keypad ergonomics, radio or vendor quirks |
| Physical device | Exact-artifact behavior on the recorded model/firmware/runtime | Every handset or even every firmware in the same family |

Build and package evidence is a separate prerequisite owned by
`java-me-build`. Record when a lane runs a diagnostic JAR instead of the
production suite; a probe can isolate a mechanism but cannot silently replace
end-to-end proof on the shipped artifact.

Choose a lane from the failure mechanism, not from convenience. Use host
oracles for fast deterministic coverage, rerun runtime-sensitive behavior in a
target VM, exercise presentation and integration in an emulator, and use the
physical device for claims that depend on real implementation or hardware.

## Preserve target semantics in tests

Test code that executes inside Java ME must compile and package under the same
language, API and preverification constraints as its target lane. Keep host-only
test helpers outside production and target suites. Do not let a host JDK API,
thread model, default encoding or collection behavior become an accidental
oracle for CLDC/MIDP.

Prefer bounded, deterministic probes with machine-readable completion markers
for runtime mechanisms. Each probe should:

- exercise one named behavior and emit a small result;
- distinguish pass, failure, skipped and unavailable states;
- fail on missing expected progress rather than waiting forever;
- avoid credentials, personal data and unbounded diagnostic output;
- leave enough target/runtime identity to interpret the result;
- preserve the first causal exception or mismatch.

When a host implementation serves as an oracle, compare explicit outputs or
state rather than assuming both runtimes share implementation details. Keep
known negative cases and malformed, truncated, oversized or unsupported inputs
when they represent reachable device failures.

## Isolate emulator sessions

Use the project's pinned emulator/runtime and established automation entry
point. Treat emulator startup and scripts as executable code; inspect unfamiliar
commands before running them.

For each scenario:

- allocate isolated emulator state, scratch storage and dynamic local ports;
- identify whether the run starts fresh or intentionally reuses persisted
  state;
- bind observations to the exact JAR hash and selected runtime profile;
- wait for semantic state, event or log markers rather than fixed sleeps alone;
- resolve current controls, commands and display state instead of assuming
  unstable runtime object identifiers;
- capture before/after evidence when a transition is otherwise ambiguous;
- stop only the owned session and remove only its owned temporary state;
- retain bounded logs, screenshots and receipts on failure according to policy.

A missing display, unavailable emulator capability or unsupported automation
control is an unavailable lane, not a product pass. Do not install a virtual
display, switch emulators or weaken an assertion merely to turn the lane green
unless that substitution is explicitly part of the test design and reported.

An emulator profile name is not evidence that vendor behavior is reproduced.
Accept a vendor-specific emulation rule only with a named runtime/device,
authoritative documentation or a reproducible physical probe, and a regression
that leaves the generic profile unchanged.

## Exercise MIDlet lifecycle

Test lifecycle behavior through the application management mechanism when the
lane supports it, not by calling callbacks in one ideal order only. Cover the
transitions relevant to the change:

- construction followed by first activation;
- repeated activation after pause;
- pause while workers, UI, RMS, media or network work exists;
- destruction after full and partial startup;
- unconditional destruction and application-initiated termination;
- relaunch with clean, valid, legacy or damaged persisted state;
- interruption and resume behavior exposed by the target runtime.

Observe duplicate workers, stale callbacks, resource cleanup, state recovery
and visible navigation after the transition. Lifecycle callbacks are intended
to return promptly; a test that invokes them directly may miss AMS timing,
resource and state behavior.

## Test LCDUI, Canvas and input

Use semantic UI state and user intent where the emulator exposes them. For
high-level LCDUI, inspect the current displayable, title, items, selection and
commands. For `Canvas` or `GameCanvas`, use rendered output plus application
oracles when available.

Cover the input surfaces relevant to supported devices:

- numeric keypad, star and pound;
- directional and fire game actions;
- softkeys and command dispatch;
- press, release, repeat and held-key timing when behavior depends on them;
- pointer/touch presence, absence, press, drag and release;
- several declared screen sizes, orientation or full-screen modes;
- focus, traversal, clipping and return navigation.

Prefer game actions for portable intent, but record raw key codes, key names and
mapped actions when diagnosing a device. Softkey placement and key mappings are
implementation evidence, not universal constants. A coordinate action is valid
when testing Canvas geometry or touch hit regions; otherwise prefer semantic
controls to brittle coordinates.

A screenshot proves only the visible frame captured. It does not prove command
reachability, keypad navigation, repaint timing, accessibility, memory safety
or another resolution. Compose with visual or accessibility skills when those
claims are required.

## Test RMS and persistent state

Give every scenario an explicit state lifecycle. Cover as applicable:

- fresh install and first open;
- close/reopen and application relaunch;
- upgrade from supported older data;
- truncated, malformed, incompatible and oversized records;
- missing records, non-dense record IDs and interrupted multi-step updates;
- full or permission-denied storage where the runtime can model it safely;
- discard/rebuild or rollback behavior after corruption;
- isolation between suites, variants and concurrent test runs.

Do not reset shared emulator or device storage to manufacture a passing test.
Use an isolated test store or an explicitly authorized backup/reset procedure.
Verify both the durable bytes/state and the user-visible recovery outcome when
the feature crosses that boundary.

## Test GCF, permissions and external systems

Start with deterministic offline endpoints under test ownership. Exercise the
target protocol and connection type, access modes, permission allow/deny paths,
partial reads/writes, zero progress, close ordering, timeout behavior and
bounded failure recovery as relevant.

A loopback test proves the hosted GCF path it exercised. It does not prove
device radio, DNS, TLS/ciphers, proxy behavior, billing prompts or production
service interoperability. Likewise, an emulator network monitor can observe
its own hosted traffic but not physical-device networking.

Tests that contact a production or third-party service, send messages, request
login codes, mutate account state or consume billable device/network resources
require explicit authorization immediately before execution. Use a dedicated
test identity when available, show the intended effect, bound retries and
requests, and never print credentials, tokens, sessions or private payloads.
Keep offline regression evidence even when a live scenario is authorized.

## Record physical-device evidence

For a physical run, preserve enough provenance to reproduce and bound the
claim:

- source revision and clean/dirty state;
- JAR/JAD hashes, variant, byte size and relevant build receipt;
- device model, firmware and reported Java platform/runtime properties;
- available storage, network type, security domain and permission decisions;
- test state, steps, expected result and observed result;
- safe diagnostics, screenshots or photos when useful;
- omitted scenarios and environmental limitations.

Install and exercise the matching unmodified JAR/JAD pair when installability
is the claim. If a failure occurs only on device, retain the exact artifact and
a minimal redacted reproduction. Add a lower-lane regression when possible,
but do not mark the device issue fixed until the relevant exact-artifact lane is
rerun.

Device installation, account use, storage reset, network calls and log
collection may mutate or expose external state. Perform only the actions
authorized for the named device and test identity.

## Report evidence without collapsing lanes

Return a compact test record:

```text
Behavior and target contract
Artifact/probe identity and hash
Host, target-VM, emulator and physical-device lanes attempted
Runtime profile, device/firmware and state precondition
Exact scenarios and oracles
Pass, fail, skipped or unavailable per lane
Retained logs, screenshots and receipts
External effects and authorization used
Unproved compatibility or device claims
Smallest next evidence lane
```

Do not report “Java ME tested,” “device-safe” or “all supported” when only a
subset of lanes, devices or variants ran.

## Composition boundaries

- `testing` owns behavior contracts, oracle quality, determinism and regression
  strategy; `java-me-testing` owns Java ME evidence lanes and mechanics.
- `java-testing` owns Java test frameworks and runner semantics; this skill owns
  in-VM probes, emulator sessions and physical-device evidence.
- `java-me` owns platform semantics; this skill exercises them on selected
  runtimes and devices.
- `java-me-build` owns target test/production suite construction and static
  JAR/JAD proof; this skill executes and observes those artifacts.
- `implementation` owns mutation safety; this skill supplies test/probe edits.
- `performance` and `java-me-performance` own measured resource and timing
  claims; constrained emulator settings here remain test context, not device
  performance evidence.
- `accessibility` owns access requirements and barrier findings; this skill can
  provide keypad, focus, display and interaction evidence.
- visual-review skills own screenshot comparison or critique; this skill owns
  the runtime state and interaction that produced the capture.
- `security` owns security risk; this skill owns permission-path and authorized
  runtime evidence without weakening trust controls.

For authorized test or probe source mutation, call `implementation` if it is
not already active in the invocation chain. If dependencies, test-suite
packaging, descriptors or target-runner build configuration must change, call
`java-me-build` instead and let it delegate mutation safety. Do not call either
target when it is already active in the invocation chain. Report an unavailable
capability or cycle rather than copying its workflow. This skill does not
automatically call generic testing, Java, Java ME, performance or review skills.
