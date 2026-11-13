# Local Asahi validation

Build source: `3798eb130eac50f1a72eeb1af2360b4ca2f68f8c`
Builder image ID: `6ed05c9c64b9fcaf22c310bec53af242c06f3d15826ccb1797ee325e37889717`
Host: Asahi aarch64, 16384-byte pages. Native rootless Ubuntu 24.04 container.

Observed results:

- Build container `rewindos-arm64-build-3` exited 0.
- Frontend: 21 files, 122 tests passed.
- Rust workspace test suites: 33 passed / 1 ignored; 198 passed;
  104 passed / 2 ignored. All other test/doc-test targets reported zero failures.
- Both binaries passed the AArch64 / 16 KiB ELF load-segment check.
- Daemon executed on the Fedora host and reported `rewindos-daemon 1.0.14`.
- Host `ldd` resolved all libraries for both binaries.
- Final isolated, network-disabled UI smoke passed: RewindOS window appeared
  and stayed alive for ten seconds on the real Asahi kernel, using container
  userspace. Only an accessibility-bus warning appeared in its log.
- Archive checksum verified with `sha256sum -c`.

Artifact on Asahi: `~/dev/rewindos/dist/rewindos-linux-aarch64.tar.gz`
Size: 22963078 bytes.
SHA-256: `27af326725c757cca0e5b606d9efd266af51a4c28f496c7bdb6dd334ff016d95`
Extracted app: `~/dev/rewindos/dist/rewindos-linux-aarch64/rewindos`.

No GitHub push or CI run. Workflow passed local actionlint 1.7.12 validation.
No daemon service, autostart, screen capture, audio capture, or model downloads
were enabled. Native Wayland rendering/capture, OCR, and model inference remain
untested. The host lacks the Tesseract executable; install its runtime package
through the normal Asahi image configuration before testing OCR. The host has
`libappindicator3.so.1`, so missing Ayatana specifically is not a blocker.

Earlier failed builds exposed missing OpenSSL development headers (now in the
Containerfile) and an overlapping private SELinux mount (resolved by sequential
build/smoke execution). The host's QEMU service warning is unrelated to this
native build. Build log: `~/dev/rewindos-arm64-build.log` on Asahi.
