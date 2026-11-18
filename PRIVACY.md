# Privacy Policy & Air-Gapped Zero-Telemetry Attestation — SbePrism SBE ITCH Viewer

**Effective Date:** November 18, 2026

SbePrism SBE ITCH Viewer (`dev.ediprism.sbe`) executes **100% locally and offline** inside your JetBrains IDE JVM process:

- **Zero Outbound Network Calls:** SbePrism contains zero HTTP clients, zero cloud APIs, zero LLM connectors, and zero analytics or crash-reporting beacons.
- **Zero Alpha / Market-Data Egress:** Proprietary `<sbe:messageSchema>` XML files, `.pcap`/`.pcapng` market-data captures, `ClOrdID`s, firm accounts, and generated C++20/Rust code never leave your workstation.
- **Offline Cryptographic License Check:** Commercial `[Pro]` subscriptions and 14-day trials are verified locally via IntelliJ Platform `LicensingFacade` and pinned JetBrains RSA X.509 root certificates.

Full canonical Privacy Policy: **[https://ediprism.github.io/privacy.html](https://ediprism.github.io/privacy.html)**
