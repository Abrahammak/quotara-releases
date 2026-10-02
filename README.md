# Quotara Pro releases

Official Windows installers and signed update metadata for Quotara Pro by Heytronix Limited.

This repository contains release downloads and this README. Application source is maintained privately.

## Downloads

Use the assets on the [latest stable release](https://github.com/Abrahammak/quotara-releases/releases/latest). Release candidates are marked as pre-releases.

Each production release includes the installer, its `.sha256` file, `SHA256SUMS.txt`, the Ed25519-signed `update-manifest.json`, and release notes.

## 4.1.0 signing trust

Version 4.1.0 uses a self-signed Heytronix Limited certificate for controlled environments. It is not publicly CA-trusted and carries no SmartScreen reputation claim. Windows may reject the signature until the verified public certificate is explicitly trusted in the controlled environment. The updater does not bypass this trust check.

Expected signer thumbprint: `6355FB71B4BC1B605A45C5BACD055C89326759F2`.

## Verify the installer

In PowerShell, compute the downloaded installer's SHA-256:

```powershell
Get-FileHash -Algorithm SHA256 -LiteralPath .\Quotara-Pro-4.1.0-Setup.exe
Get-AuthenticodeSignature -LiteralPath .\Quotara-Pro-4.1.0-Setup.exe | Format-List Status,SignerCertificate
```

The hash must match both the installer entry in `SHA256SUMS.txt` and the `sha256` field of the signed manifest. The Authenticode status must be `Valid`; inspect the publisher certificate before running the installer. A checksum alone does not authenticate a download.

## Signed update manifest

Quotara Pro's built-in updater verifies the v1 manifest schema and Ed25519 signature using its embedded trusted public key before accepting an update. It also verifies the downloaded installer's exact size, SHA-256, Authenticode validity and pinned signer before offering installation. Use About → Check for updates to use this verification flow.

Public update key (base64, raw Ed25519): `ilKr32Ypd2rP00giKjgvyGXzPCJvCY2WT4xWoqIL+MU=`

SHA-256 fingerprint of the raw public key: `c71c15d216b5c0caf296de5e61820bdd6e9a2308add8e6157057f86cd1a016d5`.

The signature covers the manifest with the `signature` field omitted, JSON keys sorted lexicographically, compact comma/colon separators, no added whitespace, and non-ASCII characters preserved, then encoded as UTF-8. Do not treat merely reading a hash from an unverified JSON file as signature verification. Never replace the trusted key in an installed app with a key supplied alongside an unexpected download.

Updates are read anonymously. The application requires no GitHub account or token.
