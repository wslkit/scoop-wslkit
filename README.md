# scoop-wslkit

The [Scoop](https://scoop.sh) bucket for [wslkit](https://github.com/wslkit/wslkit):
tools for WSL 2 — diagnose why it is broken or slow, then fix it.

```powershell
scoop bucket add wslkit https://github.com/wslkit/scoop-wslkit
scoop install wslkit
```

That gives you `wslkit` and `wsldoctor` on the path. They are the same binary:
invoked as `wsldoctor` it runs `wslkit doctor`, so anything written against the
older name keeps working.

```powershell
wslkit doctor          # a ranked diagnosis, read-only, no admin
wslkit disk list       # what each distribution's disk costs
wslkit proxy show      # what a distribution would get from your Windows proxy
```

## What is in here

One manifest, `bucket/wslkit.json`, pointing at the latest
[wslkit release](https://github.com/wslkit/wslkit/releases). It carries both
architectures and the SHA256 of each archive.

## How it is kept up to date

`.github/workflows/update.yml` checks for a new wslkit release every hour,
rewrites the manifest and commits it. It can also be run by hand from the
Actions tab.

The update runs **here** rather than being pushed from the wslkit release
workflow, so it needs no cross-repository token: this repository's own
`GITHUB_TOKEN` is enough. The cost is that a new release takes up to an hour to
appear, which for a Scoop bucket is nobody's emergency.

## Signing

The binaries are not Authenticode-signed yet, so SmartScreen warns the first
time you run one. That is tracked in
[wslkit#19](https://github.com/wslkit/wslkit/issues/19). Every release carries a
[build provenance attestation](https://github.com/wslkit/wslkit/attestations),
which you can verify:

```powershell
gh attestation verify wslkit_0.1.0_windows_amd64.zip --repo wslkit/wslkit
```
