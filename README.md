# local-first-repo-sync

Serverless, peer-to-peer-style sync for local-first apps: every device is an equal replica, merged through a
git repository you own, end-to-end encrypted.

> **Status: experimental and not yet published.** This repository is being set up. The code is being
> extracted from a working app, so nothing is installable yet, and the API will change before 1.0.
> **It has not been independently security-reviewed. Do not rely on it for sensitive data yet.**

## The idea

- Each device keeps the **full data locally** (a CRDT such as [Yjs](https://yjs.dev)), so the app works offline
  with no account and no backend of its own.
- To sync, each device publishes **one encrypted file holding its whole state** into a **private git repository
  you own**, and merges the other devices' files. A device only ever writes its own file, so devices never
  overwrite each other, and they do not need to be online at the same time.
- Files are encrypted in the browser (AES-GCM) with a key derived from a secret that travels only in a link you
  share between your own devices. The host sees ciphertext, plus file names, sizes and commit times.
- Access tokens are per device, limited to the one repository, and never leave the device.

## What it is not

- **Not direct device-to-device networking.** Devices share a store; they do not connect to each other. An
  optional WebRTC path exists in the original app but needs a signaling server and is not part of the first
  release.
- **Not multi-user collaboration.** It is for one person's devices.
- **Not a promise that the host learns nothing.** Contents are encrypted; metadata is not hidden.

## Planned shape

- A **core** that handles the adapter for your document, encryption, pull and push, scheduling and status.
- **Transports** for the hosting service. GitHub comes first; other source-code hosts are assumed to be similar
  but each needs its own check before it is supported.
- A command-line tool to list and clean up device files and to recover data from a repository.
- A fake repository and acceptance tests that each app can copy.

## Where the design lives

The working implementation and the full design notes (data-model rules, file format, security model,
decisions and the recovery plan) are in the
[Squirrel Tracks](https://github.com/mattscilipoti/squirrel-tracks) app, especially
[`docs/local-first-sync-handoff.md`](https://github.com/mattscilipoti/squirrel-tracks/blob/main/docs/local-first-sync-handoff.md).

## License

Planned: MIT. A `LICENSE` file will be added before the first release.
