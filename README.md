# QuetzaLib-EXE

Windows desktop shell for **QuetzaLib** — an Electron wrapper that hosts the
app's Flutter web build as an installable `.exe`.

The app itself lives in [QuetzaLib-APP](https://github.com/ZYDRAXYL/QuetzaLib-APP).

## What this repo is for

QuetzaLib is a Flutter app that already builds for Android and for the browser.
This repo adds a third target: the same web build, packaged in an Electron shell
with a desktop window, a native menu, and a Windows installer.

```
electron/   main process, preload, window setup, packaging config
```

**It does not contain a copy of the app.** `lib/` stays in APP and is built once
(`flutter build web`); this repo pins the APP commit it packages and hosts the
output. Copying Dart source in here would fork the app in two and every later
fix would have to be made twice.

## Status

The chain wiring is in place; the Electron shell is **not built yet**.
`node tools/chain-propagate.mjs` in APP reports the `app-source` edge as
unimplemented and skips it rather than writing an empty change. Implementing it
means adding an `app-source.json` pin here and the handler in APP.

## Releases

`exe-vX.Y.Z`, from `electron/package.json#version`. Its own namespace,
independent of APP's `v*` — they share WEB's release mirror, and the in-app
updater filters by tag prefix precisely so a desktop release never offers itself
as an Android update.

## The chain

This repo is part of QuetzaLib's six-repository architecture. `chain/chain.json`
is the contract; `tools/` and `.claude/` are mirrored from APP and must not be
edited here.

```bash
node tools/chain-lib.mjs      # this repo's place in the chain
node tools/chain-survey.mjs   # what moved in the other repos
```

Full write-up: [`chain/README.md` in QuetzaLib-APP](https://github.com/ZYDRAXYL/QuetzaLib-APP/blob/main/chain/README.md).
