---
name: java-me
description: Apply Java ME platform constraints when designing, implementing, debugging, or analyzing CLDC/MIDP software. Use for MIDlets, LCDUI or GameCanvas interfaces, RMS storage, GCF networking, optional JSRs, vendor APIs, constrained devices, or JAR/JAD compatibility; preserve the exact target profile instead of assuming Java SE.
license: MIT
---

# Java ME

Treat Java ME as a configuration, profile and device capability contract, not
as merely old Java SE. Preserve the actual target before applying general Java
guidance. A language feature, library class or familiar pattern is valid only
when the selected toolchain and deployed devices support it.

This skill owns CLDC/MIDP platform constraints and common mobile-device APIs.
It does not define a generic Java method, review workflow, detailed build or
preverification procedure, testing strategy, performance methodology, or
device-vendor folklore.

## Establish the platform contract

Determine the facts that can change the implementation:

1. **Configuration and profile:** exact CLDC, MIDP, IMP or other applicable
   versions. Do not collapse MIDP 1.0, 2.0 and later profiles into one target.
2. **Language and class format:** accepted source syntax, class-file version,
   compiler, preverifier or equivalent transformation, and obfuscation stage.
3. **API surface:** configuration/profile APIs, mandatory and optional JSRs,
   manufacturer APIs, bundled libraries, and any preprocessing variants.
4. **Devices:** supported models or capability classes, implementation quirks
   that have evidence, heap and persistent-storage expectations, application
   size limits, display shapes, input methods, and media/network capabilities.
5. **Suite contract:** MIDlet entry points, manifest and JAD attributes,
   permissions, signing assumptions, installation path, and upgrade behavior.
6. **Evidence level:** host JVM, target-compatible VM, emulator, exact runtime
   profile, or physical device.

Find these facts in repository instructions, build scripts, descriptors,
manifests, SDK libraries, release variants and existing compatibility notes.
When they disagree, surface the mismatch rather than selecting the broadest or
newest target.

The supported API set is closed: CLDC plus the selected profile, declared JSRs,
documented manufacturer APIs and bundled code. A class available on the host
JDK is not thereby available on the device.

## Override incompatible Java guidance

Hard Java ME constraints take precedence over general Java preferences. Unless
the target explicitly supports them, do not introduce Java SE-only APIs or
modern constructs such as streams, lambdas, records, `Optional`, modern NIO,
try-with-resources, reflection-based frameworks, dynamic class loading, or
newer concurrency utilities.

Do not modernize source syntax or bytecode independently of the target VM and
preverification path. Conversely, do not reject a newer Java ME profile's
documented feature merely because an older CLDC/MIDP target lacked it.

Prefer platform-compatible, simple code. Simplicity matters because every
extra abstraction can add classes, bytecode, initialization and objects, but
this is not permission to replace maintainable code with unmeasured historical
micro-optimizations.

## Keep optional capabilities truly optional

- Treat each optional JSR, protocol, media feature, pointer capability and
  manufacturer API as absent until the target contract or a runtime capability
  check establishes it.
- Isolate optional implementation classes so a baseline artifact does not link
  or verify unavailable types merely by loading an otherwise common class.
- Pair capability detection with a usable fallback, disabled feature or clear
  bounded failure. A reflection or preprocessing technique is valid only when
  the selected CLDC profile and toolchain support it.
- Keep required and optional permissions distinct. Do not make installation or
  startup depend on a permission needed only for an optional path.
- Do not infer behavioral quirks from a device brand or emulator profile.
  Preserve only differences backed by specifications, manufacturer material or
  reproducible device/runtime evidence.

When supporting several profiles or devices, decide deliberately whether to
ship one capability-adaptive suite or separate artifacts. Ensure the baseline
artifact contains neither optional classes nor descriptor requirements that
would defeat the separation.

## Respect the MIDlet lifecycle

A MIDlet is controlled by the application management software rather than a
`main` method. Design around repeated and externally initiated transitions:

- construction establishes only state safe before activation;
- `startApp()` may be called again after a pause, so activation must distinguish
  one-time initialization from resumable work;
- `pauseApp()` makes active work quiescent and releases or suspends shared
  resources according to the application's contract;
- `destroyApp(boolean)` terminates owned work, releases resources and preserves
  required state, including when destruction follows a partial startup;
- `notifyDestroyed()`, `notifyPaused()` and `resumeRequest()` communicate with
  the manager; they do not bypass cleanup or invent a desktop process model;
- a MIDlet must not use `System.exit()` as its lifecycle mechanism.

Keep constructors and lifecycle callbacks short. Move network I/O, RMS work,
parsing, asset loading and heavy computation off system and UI callbacks, while
retaining explicit ownership, cancellation and shutdown for any worker thread.

## Design for bounded resources

Memory, bytecode, persistent storage, threads, network transfer, decoded images
and UI buffers compete within the same constrained device. Define ownership and
a limit before allocation for data controlled by a server, file, RMS record,
compressed payload, media resource or user input.

- Prefer bounded collections, arrays, queues, caches and reusable buffers when
  the workload has a known maximum.
- Reject or truncate oversized external data before allocation or mutation,
  according to the product contract.
- Avoid retaining duplicate encoded and decoded forms longer than necessary.
- Account for decoded image and canvas buffers, not only compressed asset size.
- Keep thread count and stacks proportionate to the device; give each worker a
  lifecycle and stopping condition.
- Avoid allocation churn in measured hot paths such as rendering, parsing and
  input loops, without making allocation avoidance a universal style rule.
- Treat `OutOfMemoryError` prevention as design work. Recovery after exhausting
  the heap is not a reliable normal strategy.

Use named, project-owned budgets when exact device limits are unknown, and make
clear what device evidence would refine them. Detailed measurement and tuning
belong to a focused Java ME performance capability.

## Use LCDUI according to its execution model

Use high-level LCDUI screens and commands where native navigation, forms,
lists, alerts and device adaptation are desired. Use `Canvas` or `GameCanvas`
when low-level drawing and input control justify the additional responsibility.

- UI event callbacks are serialized and must return promptly. Do not perform
  blocking network, RMS, filesystem, media or long computation in them.
- Marshal worker results back through the platform's supported UI mechanism,
  such as `Display.callSerially()` when applicable; do not assume arbitrary
  background mutation of display state is safe.
- Treat repaint as scheduled work. Avoid lock arrangements where a blocking
  repaint wait and `paint()` need the same lock.
- Map intent through commands and game actions where possible. Numeric keys,
  softkeys, D-pads, keyboards, pointer input and vendor keys vary by device.
- Handle small and changing display bounds rather than assuming one desktop-like
  resolution or that full-screen mode exposes every pixel.
- A `GameCanvas` owns an off-screen buffer. Reuse it when practical, structure
  loops around input, update, render and flush, and make pause/resume/shutdown
  explicit.

Device-specific UI and keypad behavior requires device-level evidence; an
emulator screenshot alone establishes only the emulator behavior exercised.

## Treat RMS as a constrained record system

RMS is persistent record storage scoped by MIDlet suite rules, not a relational
database or filesystem substitute.

- Define record layout, version, ownership, maximum size and migration or
  discard policy.
- Treat persisted bytes as untrusted after interrupted writes, upgrades,
  partial data, full storage, or implementation failure. Validate lengths and
  counts before allocating or applying state.
- Individual RecordStore operations are serialized and atomic at the API
  boundary, but a multi-record or multi-step application invariant still needs
  explicit coordination and a recoverable commit strategy.
- Record IDs are identifiers, not dense list indexes; deletion can leave gaps.
- Close stores through all completion paths and avoid scattered open/close
  churn when a bounded grouped operation is clearer.
- Handle full, missing, invalid and inaccessible stores as product states, not
  impossible exceptions.
- Request cross-suite sharing only when required and account for its privacy
  implications.

Keep secrets proportionate to the platform's actual protection. Local RMS does
not automatically provide secure storage, encryption or resistance to device
extraction.

## Use GCF and networking defensively

The Generic Connection Framework exposes capabilities through URI schemes and
connection interfaces. A protocol's API, permission and implementation can
vary independently.

- Verify the scheme and connection type supported by the selected profile and
  device before casting or using protocol-specific behavior.
- Keep connection, input stream and output stream ownership explicit and close
  each on normal, exceptional, cancellation and lifecycle paths.
- Keep blocking connection and I/O work off lifecycle and UI callback threads.
- Bound response sizes, frame lengths, decompression output, item counts,
  queues, retries and buffered data before reading to completion.
- Expect intermittent connectivity, permission denial, partial reads, slow
  progress and device-specific protocol support. Preserve a recoverable user
  state when communication fails.
- Do not assume desktop TLS, proxy, DNS, timeout or socket behavior. Verify the
  actual GCF implementation and any required server-side compatibility layer.

## Preserve the deployable suite contract

Treat source, class files, preverification output, JAR contents, manifest, JAD,
permissions and installation behavior as one compatibility chain.

- MIDlet class names and suite identity must match the packaged classes and
  upgrade expectations.
- Configuration, profile and permission attributes must describe the artifact
  that is actually shipped.
- Attributes duplicated between manifest and JAD must agree; the JAD's JAR URL
  and size must identify the exact paired artifact.
- Optional variants must exclude unavailable implementation classes and their
  mandatory permissions, not merely hide the UI entry point.
- Keep tests, host-only helpers and tooling classes out of production suites.

Detailed compiler, preverification, obfuscation and JAR/JAD commands belong to
a Java ME build capability. The base rule is that a successful host compilation
or ordinary JAR creation does not prove an installable Java ME suite.

## Match claims to evidence

Keep these evidence levels distinct:

- static/source compatibility;
- successful target-toolchain compilation and preverification;
- host-side behavioral tests;
- execution on a target-compatible VM;
- emulator interaction and visual evidence;
- physical-device behavior for the exact artifact and target device.

Use the lowest level that can prove the claim, and escalate when behavior
depends on the VM, display, input, permissions, networking, media, heap or
installation environment. Never present host or emulator success as physical
device proof, and never treat an unavailable validation lane as a pass.
