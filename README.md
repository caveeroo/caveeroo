<picture>
  <source media="(max-width: 600px)" srcset="assets/header-mobile.svg">
  <img src="assets/header.svg" alt="caveeroo, Jaime Cavero Sánchez, security research, application security, and reverse engineering." width="100%">
</picture>

## Vulnerability disclosures

### [CVE-2026-54512](https://github.com/FasterXML/jackson-databind/security/advisories/GHSA-j3rv-43j4-c7qm) | Jackson Databind

A canonical generic type ID could satisfy a configured `PolymorphicTypeValidator` check on its outer container type while smuggling in an inner type the validator would have rejected on its own. Jackson resolved, instantiated, and populated that inner class regardless.

This is a type-policy bypass that enables arbitrary class instantiation. Exploiting it requires untrusted JSON to reach the affected polymorphic path, attacker control over the canonical type ID, and a suitable class already on the runtime classpath; reaching remote code execution additionally requires exploitable behavior during initialization, construction, or property binding. Fixed in 2.18.8 and 2.21.4 for `com.fasterxml.jackson.core:jackson-databind`, and in 3.1.4 for the Jackson 3 coordinate `tools.jackson.core:jackson-databind`.

### [GHSA-r625-mph7-wf6j](https://github.com/NationalSecurityAgency/ghidra/security/advisories/GHSA-r625-mph7-wf6j) | Ghidra

Two project-restoration paths instantiated a class named in the project data, using any accessible no-argument constructor, before checking whether it implemented the expected interface.

Exploitation required the victim to open a crafted project, and the target class had to be loadable from Ghidra's runtime classpath. The advisory doesn't identify a bundled gadget for arbitrary command execution. Version 12.1.1 is marked patched, but users installing a published binary should move to 12.1.2 or later.

### [CVE-2026-7375](https://www.wireshark.org/security/wnpa-sec-2026-50) | Wireshark

A malformed UDS define-by-memory-address request could drive both parsed field lengths to zero, and the dissector would loop without ever advancing its offset. Opening the capture, running it through `tshark`, or simply encountering the packet during a live capture was enough to hang the process and pin a CPU core.

Public evidence points to denial of service only: no memory corruption, code execution, information disclosure, or privilege escalation. Fixed in 4.6.5 and 4.4.15.

### [CVE-2026-39973](https://github.com/iBotPeaches/Apktool/security/advisories/GHSA-m8mh-x359-vm8m) | Apktool

A refactor dropped the traversal check on a resource-type string read from `resources.arsc`, and that string went straight into an output path. A crafted APK could use this to make `apktool d` write files outside the chosen decode directory.

Exploitation requires the victim to run an affected decoder on the APK, and the resulting write is limited to the Apktool process's own filesystem permissions. Turning it into code execution depends on where the file lands and on something else later loading or executing it. Fixed in 3.0.2.

## Security hardening

### [Material for MkDocs 9.7.4](https://github.com/squidfunk/mkdocs-material/releases/tag/9.7.4)

Material for MkDocs switched its social card renderer from Jinja's base `Environment` to `SandboxedEnvironment`, which the release notes describe as security hardening. There's no CVE, GHSA, severity rating, affected-version range, or public exploitability claim attached, which is why it's listed separately from the vulnerability disclosures above.

[caveeroo.dev](https://caveeroo.dev/) / Spain
