<picture>
  <source media="(max-width: 600px)" srcset="assets/header-mobile.svg">
  <img src="assets/header.svg" alt="caveeroo, Jaime Cavero Sánchez, security research, application security, and reverse engineering." width="100%">
</picture>

I am Jaime Cavero Sánchez, a security researcher working across application security and reverse engineering. Most of my published work starts at a trust boundary: a parser, type resolver, class loader, or filesystem path consuming data it should not trust.

## Vulnerability disclosures

### [CVE-2026-54512](https://github.com/FasterXML/jackson-databind/security/advisories/GHSA-j3rv-43j4-c7qm) | Jackson Databind

A canonical generic type ID could pass a configured `PolymorphicTypeValidator` on its raw container while carrying an inner type that the validator would deny. Jackson then resolved, instantiated, and populated the inner class.

The demonstrated behavior is a type-policy bypass with arbitrary class instantiation. Exploitability depends on untrusted JSON reaching the affected polymorphic path, attacker control of the canonical type ID, and a suitable class already present on the runtime classpath. Remote code execution further requires exploitable behavior during initialization, construction, or property binding. For `com.fasterxml.jackson.core:jackson-databind`, the fixes are 2.18.8 and 2.21.4; for the Jackson 3 coordinate `tools.jackson.core:jackson-databind`, the fix is 3.1.4.

The advisory credits `caveeroo` and `75ACOL` as reporters, and `omkhar` as finder.

### [GHSA-r625-mph7-wf6j](https://github.com/NationalSecurityAgency/ghidra/security/advisories/GHSA-r625-mph7-wf6j) | Ghidra

Two project-restoration paths initialized a class named in project data and, when an accessible no-argument constructor was available, instantiated it before verifying the expected interface.

The victim had to open a crafted project, and the selected class had to be loadable from Ghidra's runtime classpath. The advisory does not identify a bundled gadget for arbitrary command execution. It marks 12.1.1 patched; users installing a published binary should use 12.1.2 or later.

The advisory credits `caveeroo` as reporter.

### [CVE-2026-7375](https://www.wireshark.org/security/wnpa-sec-2026-50) | Wireshark

A malformed UDS define-by-memory-address request could make both parsed field lengths zero. The dissector entered a loop without advancing its offset, so opening a capture, processing it with `tshark`, or encountering the packet during live capture could hang the process and hold a CPU core.

The public evidence establishes denial of service, not memory corruption, code execution, information disclosure, or privilege escalation. Fixed in 4.6.5 and 4.4.15.

Wireshark credits Jaime Cavero with the discovery, and the CVE record lists him as finder.

### [CVE-2026-39973](https://github.com/iBotPeaches/Apktool/security/advisories/GHSA-m8mh-x359-vm8m) | Apktool

A resource type read from `resources.arsc` became an output path component after a refactor removed the traversal check. A crafted APK could therefore make `apktool d` write outside the selected decode directory.

The victim must run an affected decoder on the APK, and the write has only the Apktool process's filesystem permissions. Code execution remains conditional on the destination and on software later loading or executing the written file. Fixed in 3.0.2.

The advisory lists `caveeroo` as reporter and `IgorEisberg` as remediation developer; the 3.0.2 release thanks `caveeroo` for responsible disclosure and Igor Eisberg for the fix.

## Security hardening

### [Material for MkDocs 9.7.4](https://github.com/squidfunk/mkdocs-material/releases/tag/9.7.4)

Material for MkDocs moved its social card renderer from Jinja's base `Environment` to `SandboxedEnvironment` after a recommendation credited to [@caveeroo](https://github.com/caveeroo). The release calls this security hardening. No CVE, GHSA, severity, affected-version range, or public exploitability claim accompanies the change, so this record stays separate from the vulnerability disclosures.

[caveeroo.dev](https://caveeroo.dev/) / Madrid, Spain
