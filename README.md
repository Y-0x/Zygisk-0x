# Zygisk-0x

[Español(Argentina)](/READMEs/README_es-AR.md)|[Bahasa Indonesia](/READMEs/README_id-ID.md)|[Português Brasileiro](/READMEs/README_pt-BR.md)|[Українська](/READMEs/README_uk-UA.md)|[Tiếng Việt](/READMEs/README_vi-VN.md)|

Zygisk-0x is a fork of [ReZygisk](https://github.com/PerformanC/ReZygisk), which is itself a fork of Zygisk Next — a standalone implementation of Zygisk that gives KernelSU, APatch, and Magisk users Zygisk API support.

The codebase is fully written in C — no bloat, no unnecessary abstractions, just a clean and fast implementation. It also ships with a custom linker, so it doesn't rely on the system linker at all in normal conditions, which future-proofs it against linker-based detection.

## Why does this exist?

Honestly? I kept finding bugs while reading through the codebase — a stack buffer overflow here, a file descriptor leak there — the kind of stuff that matters a lot when your code runs with root privileges. So instead of just filing issues and waiting, I decided to fork it and fix them myself, documenting every finding along the way.

That's the whole philosophy behind Zygisk-0x:

- **Memory safety first** — every buffer/string operation is bounds-checked, no exceptions.
- **No silent leaks** — file descriptors and memory get cleaned up properly, on every code path.
- **Race-condition hardened loader** — module injection shouldn't be a gamble.
- **Audit-friendly** — every fix is documented with the actual root cause, not just "fixed a bug."

If you're the kind of person who reads `git log` before trusting a root module, this project is for you.

## Advantages

- Fully open-source, forever (AGPL 3.0 — see [License](#license))
- Actively audited — every fix ships with a documented root-cause analysis
- Lighter, faster binaries thanks to the full C rewrite
- Custom linker, no reliance on the system linker

## Dependencies

| Tool          | Description                        |
|---------------|-------------------------------------|
| `Android NDK` | Native Development Kit for Android |

### C Dependencies

| Dependency  | Description                 |
|-------------|------------------------------|
| `PLTI`      | Simple PLT Hook for Android |
| `CSOLoader` | SOTA Linux custom linker    |

## Installation

### 1. Pick the right build

This part actually matters — it decides how hidden and stable Zygisk-0x will be on your device. But it's not complicated:

- `release` — what most people should use. No app-level logging, fully optimized binaries.
- `debug` — heavier, unoptimized, logs everything. Only use this if you're actively debugging something or need logs for a bug report.

Stick to the `main` branch unless a specific reason (or a maintainer) tells you otherwise. Other branches may be experimental and unstable.

### 2. Flash it

Flash the zip through your root manager of choice (Magisk or KernelSU) — `Modules` section, pick the zip, done.

After flashing, check the install logs for errors before rebooting. If everything looks clean, reboot.

> [!WARNING]
> If you're on Magisk, disable the built-in Zygisk first (`Settings` → toggle off `Zygisk`), otherwise it'll conflict with Zygisk-0x.

### 3. Confirm it's working

After the reboot, check the module description under `Modules` in your root manager. You should see something like:

`[Monitor: ✅, Zygisk-0x 64-bit: ✅, Zygisk-0x 32-bit: ✅] Standalone implementation of Zygisk.`

If both daemons show ✅ (or just one, depending on your device's architecture), you're good to go.

## Translation

Two ways to help translate Zygisk-0x:

- **README translations** — add a new file under `READMEs/`, named `README_<language>.md` (e.g. `README_pt-BR.md`), and open a PR against `main`.
- **WebUI translations** — contribute through [Crowdin](https://crowdin.com/project/rezygisk) first. Once approved, grab the `.json` file, drop it in `webroot/lang`, add yourself to `TRANSLATOR.md` (alphabetical order), and open a PR.

## Support & Contribution

Found a bug? Have a fix? Open an issue or a PR — that's genuinely the best way to reach me about this project.

If you're contributing code, try to keep it consistent with the existing style, and if you're fixing something security-related, a short explanation of the root cause goes a long way (it helps everyone reviewing later, including future-me).

## License

Zygisk-0x is licensed under [AGPL 3.0](./LICENSE), same as the projects it's forked from. Read more about it on the [Open Source Initiative](https://opensource.org/licenses/AGPL-3.0) page.
