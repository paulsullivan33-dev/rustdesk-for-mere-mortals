# Running Your Own Server

This chapter ends with two small
programs running on a machine of
yours — hbbs and hbbr — a key pair
they made, and a clear account of
which ports they listen on and why.
From that moment your machines have
somewhere of their own to report to;
chapter 6 points them at it.

## What "your own server" is, exactly

Two programs, both from the same
download as each other and free
(open-source) from the RustDesk
project:

- **hbbs**, the ID server: keeps the
  map of IDs to addresses, introduces
  machines, helps them punch a direct
  connection (chapter 2).
- **hbbr**, the relay: passes
  encrypted traffic for the sessions
  that cannot go direct.

They are small, single-purpose
programs. They need no database
server, no web server, no accounts
system — hbbs keeps its map in one
SQLite file beside itself. The pair
commonly run together on one modest
machine: a home server, a mini PC, a
small rented virtual server. If the
machines you care about are all at
home and so are you, a box at home is
enough. If you travel to your
machines, the server wants a public
address of its own — and then the
firewall section below matters most.

## Getting and placing them

Download `rustdesk-server` for your
platform from the project's GitHub
releases. The Linux archive holds
three binaries: `hbbs`, `hbbr`, and
`rustdesk-utils` (small tools the
later chapters use). Make a folder
that is *theirs* — the folder they run
from is also their data folder — and
put the binaries there (in Docker
deployments the same folder is the
container's `/root` volume, and the
official `rustdesk/rustdesk-server`
image runs these same two programs).
The lab's folder, after one run:

```text
$ ls
db_v2.sqlite3  db_v2.sqlite3-shm  db_v2.sqlite3-wal
hbbr.log  hbbs.log  id_ed25519  id_ed25519.pub
```

## The first run

Start the relay first, then the ID
server, telling the ID server where
its relay lives:

```bash
./hbbr
./hbbs -r 127.0.0.1:21117
```

(If hbbr lives on another machine, the
`-r` value is that machine's address
as *clients* should reach it — it is
the address hbbs hands out when a
session needs relaying.) Two things
are born in the first seconds, both
in hbbs's log
(transcript `rustdesk-lab/transcripts/t01-server-first-run.txt`):

```text
INFO [src/common.rs:147] Private/public key written to id_ed25519/id_ed25519.pub
INFO [src/rendezvous_server.rs:1243] Key: Orc9URZ29Q4UegoYOPySQ3F48YhkjzASpOvGKfImh4M=
INFO [src/peer.rs:84] DB_URL=./db_v2.sqlite3
```

The **key pair** — chapter 7 gives it
a chapter, because clients will trust
this server by that key, and the
`id_ed25519` private half is now the
most sensitive file in the folder.
Guard the folder like a password file.
The line beginning `Key:` prints the
*public* half — the same text as
`id_ed25519.pub` — which you will copy
into every client (chapter 6).

Then the listening lines, which are
the chapter's real inventory:

```text
hbbs: Listening on tcp/udp :21116
hbbs: Listening on tcp :21115, extra port for NAT test
hbbs: Listening on websocket :21118
hbbr: Listening on tcp :21117
hbbr: Listening on websocket :21119
```

## The ports, and opening only the needed ones

| Port | Program | Job |
|---|---|---|
| 21115 TCP | hbbs | NAT-type test for clients |
| 21116 TCP + UDP | hbbs | Introductions: UDP carries registrations and heartbeats, TCP carries hole punching and setup |
| 21117 TCP | hbbr | Relayed sessions |
| 21118 TCP | hbbs | Websocket side of 21116, for the web client |
| 21119 TCP | hbbr | Websocket side of the relay, for the web client |
| 21114 TCP | — | **Not the free server.** The paid edition's API and web console live here; the free server has nothing on this port |

Chapter 1's rule — *only as exposed as
it needs to be* — applied to a
firewall: a server whose clients are
all desktop clients needs **21115,
21116 (both TCP and UDP — the UDP half
is the one everyone forgets) and
21117**. The websocket pair, 21118 and
21119, exist for the web client
(chapter 11): if you do not use it,
leave them closed, and chapter 11
explains why the project's own
documentation insists on that rather
than treating it as tidiness. And
21114 is never the free server's to
open.

Check: the project's checker, run
against your server's address, reports
each port it tests
(`rustdesk-utils doctor <address>`).
With everything up, the five real
ports answer OK; the lab's run, taken
while its hbbs was down (below), shows
what "OK" and "ERROR" look like in
transcript `rustdesk-lab/transcripts/t02-ports-and-doctor.txt`.
Expect 21114 to say ERROR forever on
the free server — doctor tests it
anyway, which is the checker's one
misleading habit, and chapter 15
names it.

## Keeping it running

A server that stops when you log out
is a hobby, not a door. On Linux the
deployment routes are: the project's
**Docker** images under Compose (the
route the documentation recommends,
with the data folder as a volume), or
the two binaries under **systemd**
units of your own — each is a plain
long-running program with no special
needs beyond its folder and the
network. Whichever route: restart
policy on, logs where you can find
them (both programs log to their
standard output), and the data folder
backed up (chapter 12).

## What the lab could not do, in the open

The lab's build machine is a sandbox
with an unusual property: it refuses
all UDP sends, even to the machine's
own loopback. hbbs tests itself at
startup by sending its own UDP port a
small packet and hearing itself answer
— and where UDP cannot be sent at all,
the test fails and hbbs exits, as the
lab's debugger trace shows line by
line (transcript
`rustdesk-lab/transcripts/t03-hbbs-self-test.txt`).
hbbr, needing no UDP, ran the whole
lab. So this chapter's hbbs behaviour
is verified to the threshold of the
self-test — key pair, database,
listening sockets announced, relay
found — and the running service
beyond it is described from the
project's documentation, with the
registrations it would have received
visible from the client's side in
chapters 6's transcripts. On ordinary
hardware none of this arises; the
episode is kept because the failure
mode itself is real troubleshooting
material (chapter 15).
