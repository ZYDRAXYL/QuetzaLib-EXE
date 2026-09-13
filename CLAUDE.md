# CLAUDE.md

Guidance for Claude Code working in **QuetzaLib-EXE**.

## What this repo is

The Windows desktop shell for QuetzaLib: an Electron wrapper (`electron/`) that
hosts the app's Flutter **web** build in a desktop window and packages it as an
installable `.exe`.

## The one rule that matters here

**The app is not in this repo.** `lib/` lives in
[QuetzaLib-APP](https://github.com/ZYDRAXYL/QuetzaLib-APP) and serves Android,
browser, and desktop from one source tree, splitting by platform through
conditional exports (`local_image_platform{,_io,_web}.dart` and four more of the
same shape). This repo packages the *output* of `flutter build web`.

So:

- A change to a screen, a service, a model, or a localization string belongs in
  **APP**, not here — even if you noticed it while running the desktop build.
- Never copy Dart source into this repo. That forks the app and every later fix
  has to be made twice.
- What belongs here: the Electron main process, the preload script, window and
  menu behaviour, the installer config, and anything genuinely desktop-only.
- Nothing here may point back at APP as a chain dependency. APP is the root of
  the graph; an edge into it makes propagation loop.

## Status

The chain wiring is in place; the Electron shell is **not built yet**. The
`app-source` handler in `tools/chain-propagate.mjs` reports that and skips
rather than writing an empty change.

## Mirrored files — do not edit here

`chain/chain.json`, `tools/chain-*.mjs`, and everything under `.claude/` are
**generated output**, mirrored from APP by `tools/mirror-claude.mjs`. Each
carries a header saying so. A local edit is lost on the next mirror.

To change one: edit it in `QuetzaLib-APP/.claude/` (or `chain/`, or `tools/`),
then run `node tools/mirror-claude.mjs` from APP.

The Flutter-shaped skills (`run-quetzalib`, `quetzalib-l10n-style`,
`version-update`, …) are deliberately **not** mirrored here — there is no
`pubspec.yaml` in this repo for them to act on.

## Useful commands

```bash
node tools/chain-lib.mjs      # this repo's upstream/downstream edges
node tools/chain-survey.mjs   # what moved in the other repos since last look
```

## Releases

`exe-vX.Y.Z`, from `electron/package.json#version`. Independent of APP's `v*`.
Both land in WEB's shared release mirror, so **never** drop the tag prefix: the
in-app updater filters on it, and without it a desktop release would offer
itself to Android installs as an update.
