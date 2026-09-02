---
name: java-me-review
description: Review Java ME changes for CLDC/MIDP compatibility, optional-API linkage, MIDlet lifecycle, bounded-memory, LCDUI, RMS, GCF, packaging, and device-specific defects. Use for a focused Java ME review or alongside broader Java and review guidance.
license: MIT
---

# Java ME Review

Inspect Java ME changes for failures caused by constrained profiles, managed
MIDlet lifecycle, optional capabilities, small-device resources and deployment
artifacts. Treat emulator and host behavior as partial evidence, not as a
substitute for the actual target.

This skill supplies Java ME-specific review signals. It does not define the
generic review workflow or finding taxonomy, repeat general Java checks, teach
the whole CLDC/MIDP platform, or prescribe detailed build, test and performance
procedures.

## Anchor every finding to a target

Resolve enough of the target contract to make each claim valid:

- configuration and profile versions;
- source and class-file levels plus preverification path;
- mandatory, optional and manufacturer APIs;
- preprocessing or artifact variants;
- permissions and signing assumptions;
- device capabilities and budgets relevant to the change;
- whether evidence came from a host, target-compatible VM, emulator or device.

Check repository instructions, compiler inputs, SDK libraries, manifests, JADs
and shipped variants. If the target remains unknown, qualify the issue instead
of assuming MIDP 2.0, CLDC 1.1, Java SE, or a particular Nokia implementation.

## Review profile and linkage compatibility

- Check introduced syntax, class-file output and referenced APIs against the
  exact configuration/profile, not merely the host compiler.
- Look for Java SE classes or language features unavailable on the target,
  including indirect use through bundled libraries or generated code.
- Trace optional JSR and vendor types through field descriptors, method
  signatures, static initialization, inheritance and common entry points. Code
  can fail verification or class loading before a guarded branch executes.
- Verify that capability detection tests the same capability later used and
  has a bounded fallback for absence, denial or partial implementation.
- When preprocessing creates variants, review the generated variant or its
  deterministic inputs. A source-level guard is not evidence that excluded
  imports, classes, permissions or code are absent from the artifact.
- Distinguish an intentional vendor-specific artifact from accidental vendor
  coupling in a supposedly portable artifact.

Do not flag `Class.forName`, preprocessing or a broad linkage catch
mechanically. On some targets they are deliberate isolation techniques. Prove
that the technique itself is supported and that the baseline artifact does not
link the optional implementation eagerly.

## Review MIDlet lifecycle transitions

Trace construction, first activation, repeated activation after pause, partial
startup, pause, requested destruction and unconditional destruction.

- Verify that `startApp()` is safe when called more than once and does not
  duplicate workers, connections, screens, timers or durable mutations.
- Check whether blocking RMS, network, parsing, media or asset work delays a
  lifecycle callback.
- For `pauseApp()`, identify active threads, connections, media, timers, input
  and mutable UI state. An empty callback is a finding only when owned work is
  required to quiesce, release shared resources or preserve resumable state.
- Ensure `destroyApp(boolean)` terminates every owned worker and resource even
  after partial initialization or a previous pause.
- Check that worker completion cannot update a destroyed or superseded screen,
  connection generation or MIDlet instance.
- Verify that lifecycle flags represent actual resource state rather than only
  whether a callback has previously run.

Report the concrete surviving resource, duplicate action or invalid transition;
do not claim every MIDlet needs identical pause behavior.

## Review bounded memory and work

For every size or count controlled by network, file, RMS, decompression, media
or user input, locate the bound before allocation, copying, iteration or core
state mutation.

- Flag read-until-EOF accumulation without a project-owned maximum, even when
  buffers grow incrementally.
- Check integer overflow and negative values before using lengths in array
  allocation, offsets or loop bounds.
- Trace simultaneous ownership of encoded bytes, decoded objects, images,
  canvas buffers, parser scratch, queues and cached models. Peak live memory is
  more important than any allocation viewed alone.
- Inspect loops and callbacks for unbounded object, string, image or thread
  creation. Tie the concern to a reachable workload rather than style.
- Verify that caches, histories, directory listings, retry queues and pending
  requests have eviction or fixed capacity consistent with the product.
- Check oversized or malformed input rejection before it partially mutates RMS,
  navigation, peer state or another durable model.

Do not demand object pooling, manual nulling, fixed arrays or compressed code
without target-specific evidence. Escalate measured allocation, rendering or
CPU questions to a focused performance review.

## Review LCDUI, Canvas and input behavior

- Find network, RMS, filesystem, media, waits and heavy parsing inside command,
  key, pointer, paint, `callSerially` or other serialized UI callbacks.
- Verify that background workers marshal visible state changes through a
  supported UI boundary and cannot race screen replacement or destruction.
- Check `serviceRepaints()` and similar waits for locks also needed by
  `paint()`, which can deadlock.
- Review `Canvas` and `GameCanvas` loops for explicit pause, resume and stop
  behavior, bounded frame work and correct repaint/flush assumptions.
- Look for repeated creation of canvases or decoded images where their backing
  buffers remain live concurrently.
- Trace key handling through game actions, commands and device-specific paths.
  Hard-coded key codes, softkey positions, pointer presence and repeat behavior
  require target evidence and a fallback where portability is claimed.
- Check layout against runtime dimensions, command areas and missing pointer or
  full-screen capabilities rather than one emulator resolution.

A screenshot proves only the visible state exercised. It does not prove keypad
navigation, timing, memory safety, other resolutions or physical-device input.

## Review RMS invariants and recovery

- Verify record lengths, counts, versions and identifiers before allocation or
  deserialization.
- Look for code that treats record IDs as dense indexes or assumes deletion
  renumbers later records.
- Individual RMS operations are atomic, but multi-record state changes are not
  automatically one transaction. Trace interruption between steps and verify a
  recoverable generation, marker, ordering or discard strategy where needed.
- Check startup and upgrade behavior for missing, older, newer, truncated,
  corrupt and full stores.
- Verify every opened store is closed on success and failure, and that ignored
  close failures cannot leave a required state transition ambiguous.
- Review cross-suite sharing and stored credentials for concrete privacy risks;
  `AUTHMODE_ANY` and local persistence are not secure defaults.
- Check that RMS work on UI or lifecycle callbacks remains bounded enough for
  their responsiveness contract.

Do not import relational-database expectations. Judge recovery against the
application's actual record layout and failure model.

## Review GCF and external I/O

- Confirm each URI scheme, cast and protocol operation exists on the target and
  requests the matching required or optional permission.
- Trace connection plus input/output stream ownership independently; closing
  one object is not proof that every implementation releases the others as the
  application expects.
- Handle unknown content length, partial reads, zero progress, permission
  denial, intermittent network and mid-response failure without unbounded
  retries or stale UI state.
- Validate frame, response, vector, decompression and media sizes before
  allocation. Do not trust server headers as the only bound.
- Check cancellation and MIDlet pause/destroy while a blocking operation is in
  progress. The worker must not become immortal merely because a device ignores
  interruption in an I/O implementation.
- Separate failures caused by a proxy or compatibility server from claims about
  the device's native HTTP, TLS, socket or DNS support.

## Review the shipped suite, not only source

When a change affects platform reach, optional APIs, permissions or release
variants, inspect the produced artifact or disclose that this was not done.

- Verify MIDlet entry classes, configuration/profile attributes and suite
  identity against packaged classes.
- Check that manifest and JAD duplicates agree and that `MIDlet-Jar-Size` and
  `MIDlet-Jar-URL` identify the exact JAR.
- Confirm required versus optional permissions match reachable features.
- Inspect baseline variants for forbidden optional/vendor classes and their
  transitive references.
- Ensure production suites exclude tests, host helpers and unintended assets,
  and remain within declared artifact or class-count budgets.
- Distinguish compilation, preverification, installation and runtime success;
  one does not imply the next.

Detailed remediation commands belong to the project or a Java ME build skill.
The review responsibility is to identify which artifact contract is violated
and provide evidence from the actual shipped variant.

## Report Java ME-specific evidence

When used alone, describe each material issue with:

- source or artifact location;
- affected configuration, profile, capability or device class;
- lifecycle, memory, UI, RMS, GCF or packaging invariant;
- reachable device consequence;
- evidence already obtained and the weakest additional lane that could confirm
  the issue;
- a correction direction that remains valid on the target.

Distinguish an incompatibility proved by source/artifact inspection from a
runtime risk needing target-VM, emulator or physical-device evidence. Do not
turn “not tested on device” into a defect unless device proof is part of the
stated acceptance contract.

## Avoid false positives

Do not report an issue solely because code:

- uses Java 1.x-era syntax, raw collections or explicit cleanup required by the
  platform;
- avoids Java SE abstractions unavailable on the target;
- uses a documented preprocessing or vendor-specific path in the artifact that
  intentionally owns that dependency;
- catches a broad failure at a narrow optional-API linkage boundary;
- uses a small fixed buffer, bounded array or simple architecture;
- lacks modern concurrency, collection, serialization or UI APIs;
- differs from desktop Java practice without violating a Java ME contract.
