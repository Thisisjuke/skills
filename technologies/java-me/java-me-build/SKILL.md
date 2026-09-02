---
name: java-me-build
description: Build, preverify, package, and inspect Java ME JAR and JAD artifacts under constrained toolchains.
license: MIT
metadata:
  skill-calls: "CALLS WHEN NEEDED implementation"
---

# Java ME Build

Produce an installable Java ME suite for its declared configuration, profile
and device class. Preserve the repository's established toolchain and prove the
final JAR/JAD pair rather than treating host compilation or archive creation as
deployment compatibility.

This skill owns Java ME target compilation, preverification, suite metadata,
variant packaging and artifact inspection. It does not own generic Java build
topology, Java ME application semantics, test strategy, device interaction or
release publication.

## Discover the target build contract

Inspect the project instructions, build entry points, CI, SDK configuration,
manifests, JAD generation and shipped artifacts. Establish:

- exact configuration and profile, such as CLDC and MIDP versions;
- accepted source syntax and class-file version;
- compiler JDK, Java ME SDK or equivalent toolchain and preverifier;
- configuration/profile API libraries, optional JSRs and manufacturer APIs;
- whether those libraries define the target platform or are bundled code;
- preprocessing symbols, generated sources and per-device variants;
- shrink, optimize, obfuscate and preverification stages in their actual order;
- MIDlet entry classes, resources, permissions and signing expectations;
- JAR/JAD names, installation path, size/class/resource budgets and checksums;
- the narrowest established build and artifact-verification commands.

Keep the host JDK, source target, class-file target and device runtime distinct.
Do not infer a valid CLDC/MIDP artifact from a modern `javac` invocation or from
Java ME API stubs being present somewhere on an ordinary classpath. Use the
project's target-platform mechanism—such as its boot class path, platform class
path or compiler target—so accidental Java SE API references are rejected by
the supported toolchain.

Do not replace a pinned SDK, preverifier, emulator, ProGuard configuration or
custom build with a preferred modern build system. If the declared profile,
compiler options, API libraries, manifest and release documentation disagree,
surface the mismatch before building.

## Preserve the bytecode pipeline

Follow the repository's supported pipeline. Conceptually it may contain:

```text
preprocess/generate → compile against target APIs → transform bytecode
→ preverify final classes → package resources and manifest → generate JAD
→ inspect the exact artifacts
```

The concrete order depends on the toolchain. When the target requires legacy
preverification, every bytecode-changing stage must occur before the final
preverification result used by the device. Some tools combine shrinking,
obfuscation and Java ME preverification; do not add a redundant or incompatible
second pass.

When required, preverification must use the same target class hierarchy
available at runtime, including the selected configuration, profile, optional
APIs and bundled libraries. A class can compile yet fail preverification or
device verification because of its bytecode, hierarchy or an unavailable
referenced type.

When diagnosing a failure, preserve the first causal stage and classify it:

- preprocessing or generated source differs from the intended variant;
- source syntax, target API or class-file level is unsupported;
- target API libraries are missing, duplicated or ordered incorrectly;
- shrink/obfuscation removed or renamed an entry point, resource or reflective
  reference;
- preverification rejected bytecode or resolved a different class hierarchy;
- package metadata does not describe the generated classes or resources;
- installation or launch fails despite a structurally valid archive.

Do not disable required preverification, broad warnings, shrinking rules or
target API checks merely to obtain a JAR. Fix or explicitly revise the owning
target contract.

## Build coherent variants

Treat each device/profile variant as a complete artifact contract. Derive its
source set, symbols, API libraries, resources, manifest, JAD, permissions and
filename from one identified variant definition.

For a baseline variant without an optional JSR or manufacturer API, verify the
final artifact excludes:

- implementation classes and transitive signatures that reference that API;
- optional-only resources or native/tooling payloads when they are not needed;
- permissions, push declarations and metadata that require the capability;
- entry points or generated registries that still load the optional path.

Hiding a feature in the UI or excluding one source file is insufficient if a
common class still links the optional type. Conversely, do not split variants
without a target or product reason; one capability-adaptive artifact may be the
project's deliberate contract.

Keep test classes, host adapters, emulator tools, diagnostic MIDlets,
unintended secrets or private build configuration, and intermediate artifacts
out of production suites unless a named distributable variant intentionally
includes them. Preserve required licenses and notices for bundled code and
resources.

## Verify the manifest and JAD as a pair

Inspect the final archive and descriptor, not their templates alone.

- The JAR contains the manifest, every declared MIDlet class, referenced icons
  and required runtime resources with correct case-sensitive paths.
- Suite identity attributes such as name, vendor and version are valid and
  agree where the selected profile requires duplication.
- Every `MIDlet-<n>` declaration names an actually packaged public MIDlet entry
  point and an existing icon when one is declared.
- `MicroEdition-Configuration` and `MicroEdition-Profile` describe the actual
  target artifact.
- Required and optional permissions match reachable features and the chosen
  variant; unknown required permissions can prevent installation.
- The JAD's `MIDlet-Jar-URL` resolves to the intended final JAR, and
  `MIDlet-Jar-Size` equals that JAR's exact byte length after every transform.
- Attributes are case-correct, not duplicated, and serialized using the
  applicable manifest/JAD continuation and encoding rules.
- Project-specific vendor attributes are included only for the device variants
  that own and verify them.

For an unsigned MIDP suite, descriptor values can affect installation and
runtime properties according to the target profile. For a trusted suite,
attributes duplicated between the signed manifest and JAD must satisfy the
profile's equality rules. Do not create one generic “JAD overrides manifest”
rule for every trust state or Java ME generation.

## Handle signing as a device trust contract

Sign only when the requested artifact, certificate chain and target protection
domain require it, and only with explicit authority to access the signing
identity. Use the repository's approved signing workflow and secret store.

Verify that:

- the target devices trust the certificate chain and supported algorithms;
- the requested permissions are available in the resulting protection domain;
- the signature covers the exact final JAR;
- certificate and signature attributes are written to the matching JAD;
- no class, resource or signed-manifest change occurs after signing; rebuild
  and re-sign whenever the packaged JAR changes;
- keys, passwords and certificate material never enter logs or source output.

A mathematically valid signature from an unrecognized or expired chain does not
establish an installable trusted MIDlet. Never substitute a bundled example
private key or self-signed certificate for a deployment identity.

## Enforce evidence-based device budgets

Check the final compressed JAR size, class count, resource sizes and any
project-owned method, bytecode or descriptor limits. Derive thresholds from the
supported device/install path or an explicit project guardrail; do not copy the
512 KiB or 300 KiB limits of a reference project.

Inspect both compressed artifact size and runtime-sensitive content when the
device constraint requires it. A small JAR does not prove heap headroom, and a
large firmware-bundled suite does not prove that third-party installation
accepts the same size.

For a reproducibility claim, control toolchain versions, generated inputs,
archive entry order, timestamps and host-specific metadata, then compare clean
independent builds. Checksums bind later emulator or device evidence to the
exact artifact but do not prove compatibility by themselves.

## Match the build claim to the evidence

Keep these results separate:

1. sources compile against the declared target API;
2. final classes pass the target preverification path;
3. JAR contents, metadata, variants, budgets and JAD pairing pass inspection;
4. the exact suite installs and launches on a target-compatible runtime;
5. the exact JAR/JAD passes relevant emulator or physical-device checks.

Use `java-me-testing` when selected for emulator/device behavior and install
evidence beyond static artifact checks. Do not report a skipped, unavailable or
substituted lane as passed. A successful host or Java SE launch is not Java ME
installation evidence.

Do not publish, upload, deploy, release or alter a device merely because the
build succeeded. Those external mutations require explicit authorization.

## Report build and artifact evidence

Return a compact record:

```text
Variant and target configuration/profile
Build entry point, compiler JDK and Java ME toolchain
Source/class-file target and target API libraries
Preprocessing, transformation and preverification path
Produced JAR/JAD and exact checksums
Entry points, resources, permissions and optional APIs inspected
JAD/manifest identity, URL and byte-size result
Artifact and device budget result
Signing/trust state when applicable
Compilation, preverification, artifact, emulator and device evidence separated
Unexercised variants or compatibility risks
```

## Composition boundaries

- `java-build` owns general build topology, wrappers, dependencies, toolchains
  and ordinary Java artifacts; `java-me-build` owns the constrained suite delta.
- `java-me` owns platform and application validity; this skill encodes and
  verifies that target in build artifacts.
- `implementation` owns shared mutation safety; this skill supplies Java ME
  build-specific changes and evidence.
- `testing` and `java-me-testing` own test strategy and runtime/device behavior;
  this skill owns static build and package gates.
- `security` owns trust and credential risk; this skill verifies that an
  authorized signing operation produced the intended suite contract.
- `performance` and `java-me-performance` own runtime measurement; this skill
  owns artifact budgets and exact binaries used by those measurements.
- `review` and `java-me-review` own finding methodology and review checks; this
  skill provides build remediation and artifact proof.

For authorized build-script, descriptor or packaging mutation, call
`implementation` if it is not already active in the invocation chain. Report
the missing capability if unavailable; do not copy its workflow. This skill
does not automatically call `java-build`, `java-me` or a testing capability.
