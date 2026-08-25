# p2p-chat

Serverless peer-to-peer chat. Messages travel directly between browsers over a
WebRTC data channel and never touch a server.

**Live:** https://jay23606.github.io/p2p-chat/

Open it, copy the shareable link, send it to someone. They join, and you are
talking to each other's browser.

## How it works

Two browsers still have to be introduced before they can talk directly, and
that introduction is the only part needing infrastructure. It comes from
[foyer](https://github.com/jay23606/foyer) over Supabase: a room, a short code,
and the offer/answer exchange that opens the connection. After that the
conversation is peer to peer.

Nothing is stored. No history, no account, and no server that could read what
you send even if it wanted to.

## What changed from the original

This began as a PeerJS example, which meant depending on a third party's broker
to introduce the two ends. Two things are different now.

**The link carries a room code rather than a raw peer id** — five characters,
from an alphabet with no `O`/`0` or `I`/`1`, because codes get read aloud.

**Everyone connects to everyone.** The old version made whoever opened the link
the hub and had them relay everybody else's messages, so closing that one tab
ended the conversation for the whole room. A mesh has no hub, no relay code,
and no tab whose loss takes the room with it.

## Running it

One HTML file, no build step. Serve the directory and open it:

```bash
npx http-server . -p 8080
```

Two browsers on one machine need two different origins, since a single origin
shares one session. Two ports is enough.

## Configuration

The Supabase URL and anon key are in the page. The anon key is public by
design — row-level security is what protects data, and this app stores none.

The schema lives in foyer's repository, applied with the `p2pc_` prefix so it
can share a project with other apps without their rooms mixing.

## Limitations

STUN only, so two peers both behind symmetric NAT will not connect. There is no
TURN relay, because a relay is a server and the point here is not having one.
