
### [security.md](https://gist.github.com/lrvick/3b65e5a8b60108190ef6e5b1a383f510 ) Security upgrades most organizations need.

> This overview provides a high-level summary of each security practice and its respective implementation, protections, and resources.

---
#### Web Content Signing via Service Workers
- **Implementation**:
  - **M-of-n Signing**: Web interface bundle is compiled and signed by multiple parties.
  - **Service Worker**: Mandates that future updates are signed by valid keys certified by a pinned CA and have a newer timestamp than the current version.
- **Protections**:
  - Against compromised insider tampering with frontends.
  - Protects against BGP attacks, DNS takeover, and TLS MITM.
- **Resources**:
  - [Using Service Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers)
  - [Research Paper](https://arxiv.org/pdf/2105.05551)

#### Web Request Signing via WebAuthn
- **Implementation**:
  - **Public Key Collection**: Gather WebAuthn public keys from multiple devices for all users.
  - **Authenticators**: Includes external (Yubikey, Nitrokey, etc.) and platform (iOS 13+, Android 7+, etc.) authenticators.
  - **Certify Public Keys**: Trusted enclave certifies WebAuthn public keys.
  - **Sign Requests**: WebAuthn signs all impacting web requests (trades, transfers) and private key enclaves validate signatures before processing.
- **Protections**:
  - Guards against compromised insider tampering with backends and TLS MITM.
- **Resources**:
  - [Using WebAuthn for Signing](https://developers.yubico.com/WebAuthn/Concepts/Using_WebAuthn_for_Signing.html)

#### Internal Supply Chain Integrity
- **Implementation**:
  - **Public Key Certification**: Collect and certify asymmetric public keys from engineers.
  - **Code Signing**: Engineers sign all code commits and reviews; multiple CI/CD systems build and sign artifacts.
  - **Artifact Deployment**: Production systems deploy only artifacts signed by multiple CI/CD systems.
- **Protections**:
  - Mitigates risks of compromised insiders impersonating other engineers or injecting malicious code.
  - Protects against tampering by compromised CI/CD systems.
- **Resources**:
  - [Distrust Foundation SIG](https://github.com/distrust-foundation/sig)
  - [Git Signatures](https://github.com/hashbang/git-signatures)
  - [Git Commit Signing](https://github.com/hashbang/book/blob/master/content/docs/security/Commit_Signing.md)
  - [Git SSH Signatures](https://blog.dbrgn.ch/2021/11/16/git-ssh-signatures/)

#### External Supply Chain Integrity
- **Implementation**:
  - **Public Key Collection**: Collect and pin asymmetric public keys from code reviewers.
  - **Dependency Reviews**: Review third-party dependencies in critical codebases.
  - **Review Signing**: Sign reviews with certified public keys and publish reviews in a documented format.
  - **CI/CD Failures**: CI/CD systems fail builds when un-reviewed dependencies are present.
- **Protections**:
  - Prevents obvious malicious code from being injected into external libraries.
- **Resources**:
  - [GitHub Gist on Dependency Review](https://gist.github.com/lrvick/d4b87c600cc074dfcd00a01ee6275420)
  - [Lance Verifier](https://gitlab.com/wiktor/lance-verifier)
  - [In-toto Attestation](https://in-toto.org)

#### Accountable Air-gapped Workflows
- **Implementation**:
  - **Deterministic Builds**: Multiple parties compile and sign air-gapped OS and firmware artifacts.
  - **Firmware Verification**: New laptops are acquired and verified with signed hashes, CA keys pinned, and TPM verification devices.
  - **Storage Security**: Laptops are stored in tamper-evident vaults requiring multiple parties for access.
  - **Signature Verification**: Firmware verifies multi-party signatures on installation media.
- **Protections**:
  - Guards against tampering by any single compromised insider, compiler, or build system.
- **Resources**:
  - [Distrust Foundation Airgap](https://github.com/distrust-foundation/airgap)
  - [Hashbang Airgap](https://github.com/hashbang/airgap)
