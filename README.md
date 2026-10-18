# Minecraft Blocked Servers Archive

This repository serves as an automated, verifiable archive of Mojang's official blocked servers list (`SHA-1` server hashes):
👉 **`https://sessionserver.mojang.com/blockedservers`**

## Branch Structure

- **`master`**: Contains commits **only when `blockedservers.txt` actually changes**. This keeps the commit history on `master` clean and focused strictly on additions or removals on Mojang's blocklist.
- **`status-update`**: Tracks **every check** (executed regularly via GitHub Actions).
  - If `blockedservers.txt` changed, `master` is merged into `status-update`.
  - If no changes occurred, an empty `Verified` status-check commit is created to record the exact timestamp and response metadata of the check.

## Historical Data & Reconstruction Notice

> [!NOTE]
> **All commits and states prior to October 18, 2026 (`2026-10-18`) are reconstructions from the [Internet Archive (Wayback Machine)](https://web.archive.org/web/*/https://sessionserver.mojang.com/blockedservers)** covering snapshots from **April 30, 2016** to **October 18, 2026** (marked with `[Wayback]` in the commit message).
>
> All commits starting from **October 18, 2026** onwards are captured live at regular intervals directly from Mojang's API via GitHub Actions.

## Authenticity & Verification (Live Captures)

Every live change committed to `master` includes multiple independent proofs of authenticity:

1. **GitHub `Verified` GPG Signature**: All automated commits (including empty status checks on `status-update`) are created via the GitHub GraphQL/Merge API and signed by GitHub.
2. **RFC 3161 Cryptographic Timestamp (`blockedservers.txt.tsr`)**: Signed by DigiCert's Timestamp Authority (`http://timestamp.digicert.com`). Verify locally with:
   ```bash
   openssl ts -reply -in blockedservers.txt.tsr -text
   ```
3. **SLSA Build Provenance Attestation (Sigstore)**: Cryptographically proves that `blockedservers.txt` was fetched by the unaltered GitHub Actions workflow on an official GitHub runner. Verify with the GitHub CLI:
   ```bash
   gh attestation verify blockedservers.txt --repo <OWNER>/<REPO>
   ```
4. **Wayback Machine Snapshot**: Each detected change automatically triggers a snapshot on `web.archive.org` and records the snapshot URL in the commit body.
