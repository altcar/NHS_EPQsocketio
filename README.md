# NHS_EPQsocketio — archived

This repository was a scratchpad from 2023: three unrelated attempts at a real-time forum,
none of which ran end to end.

- `server background/server.js` opened a socket.io server that emitted an event to the socket
  that had just connected, and nothing else.
- `server background/block-list/` was an unused sketch of a hash chain. The `Blockchain` class
  referenced a `CryptoBlock` that was never defined, and `blockchain.js` used an undeclared
  `data` variable.
- `blockchain client/client.js` read a file before declaring `err`, and never received anything.
- `socketio/` rendered a page that emitted one hard-coded string every time a button was pressed.

**The working version lives elsewhere:**

- Live: <https://nhs-epqsocketio.hheylau.workers.dev>
- Code: <https://github.com/altcar/nhforum> (`realtime/`)
- The verifiable board: <https://nhforum.hheylau.workers.dev>

## Why it is not socket.io any more

socket.io cannot run on Cloudflare Workers. Its protocol assumes a persistent Node process and
falls back to HTTP long-polling, neither of which exists in a Worker. The replacement is a
Durable Object with the WebSocket Hibernation API, which is what socket.io was being used for:
one authoritative process holding the live connections, fanning messages out to every listener.

The messages themselves are now signed blocks in an append-only log, so the server can order them
but cannot forge or silently edit them. That is the part the 2023 version was reaching for and
never had.

## Security notice

`server background/database.js` contained a hard-coded MySQL host, username and password, and was
public from April 2023. The file has been deleted, but **it remains in this repository's git
history**. Deleting a file does not delete a secret.

If that database still exists:

1. Rotate the password now.
2. Close the port to the public internet.
3. Treat the data as disclosed.

Rewriting the history (`git filter-repo` / `filter-branch` and a force-push) removes it from
clones, but not from forks, mirrors, or anyone who already cloned it. Rotation is the only
remediation that actually works.
