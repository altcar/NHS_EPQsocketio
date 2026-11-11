# NHS_EPQsocketio — archived

**This repository is archived and read-only. Nothing here runs, deploys, or is maintained.**

It is the 2023 scratchpad from an EPQ project: three unconnected attempts at a real-time forum,
kept as a record of how the idea started. Everything was rebuilt from scratch in 2026.

| | |
|---|---|
| **Working version** | <https://github.com/altcar/NHS_nhforum> |
| **Live board** | <https://nhs-epqsocketio.hheylau.workers.dev> |
| **Live forum** | <https://nhforum.hheylau.workers.dev> |

---

## What is in here

Four directories, none of which is a working application.

### `socketio/`
The closest thing to a demo. An Express app serving one EJS page with a single button that emits
the string `"new Rectangle(0,5))"`. The server logs whatever it receives and does nothing else.

### `server background/`
A socket.io server that, on connection, emits an empty event back to the socket that just
connected. Alongside it, `block-list/` is a sketch of a hash chain:

- `blockchain.js` constructs a `CryptoBlock` that is never defined anywhere in the repository.
- `blockchain.js` also reads an undeclared variable `data`.
- `block1.js` builds blocks, hashes them with `crypto-js`, and prints to the console; the chain
  never leaves the process.
- `net.js` is entirely commented out.

### `blockchain client/`
A socket.io client that reads `readMe.txt` before declaring `err`, so the first thing it does is
throw. It never received a message.

### `README.md`
This file.

There is no `wrangler.jsonc`, no Worker entry point, and no build step. That matters: this
repository was once connected to the `nhs-epqsocketio` Cloudflare Worker as a deploy source, and
every push failed with `unable to verify Worker` because there was nothing here to deploy. It is
now disconnected.

---

## Why it was abandoned

The original plan was a decentralised forum with a blockchain and live updates. Neither half
survived contact with reality:

- **socket.io cannot run on Cloudflare Workers.** The protocol assumes a persistent Node process
  and falls back to HTTP long-polling, neither of which exists in a Worker.
- **A hash chain written by the server proves nothing.** In this code the server both writes the
  chain and is the only thing that ever reads it, so any tampering is invisible. A chain nobody
  verifies is decoration.

The rebuild addresses both: native WebSockets on a Durable Object, and a signed append-only log
that the reader verifies in their own browser. See the working repository for the design.

---

## Security notice

`server background/database.js` was committed here in 2023 with a hard-coded MySQL host, username
and password. It was deleted in `936630a`, but **it remains in this repository's git history**, and
the repository was public for three years. Deleting a file does not delete a secret.

If that database still exists, the password must be treated as disclosed and rotated. Rewriting the
history removes it from future clones, but not from forks, mirrors, or anyone who already cloned
it — so rotation is the only remediation that actually works.

---

## History

Two commits, both from 2026:

| Commit | What it did |
|---|---|
| `a2ff81e` | The original 2023 files, uploaded |
| `936630a` | Removed the credential file and the dead client; added this README |

Archived on 11 November 2026.

## Licence

None declared. The contents are kept for reference only.
