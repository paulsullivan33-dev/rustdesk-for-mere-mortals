# Limits

What RustDesk cannot do, and what it
must not be asked to do — the chapter
the rest of the guide has been
promising. Read it before the setup
you care about depends on any of
these.

## The free server is deliberately small

The open-source hbbs and hbbr
introduce machines, help them punch
direct connections, and relay what
cannot go direct. That is all. There
are **no accounts** on the free
server, no web console, no audit log
of who connected to what, no shared
address books, no device groups, no
custom client generator — those live
in the paid edition behind its API
server (chapter 11). If your need is
a *fleet with policies*, the free
server is not a smaller version of
that product; it is a different,
smaller tool, and this guide has
treated it as one throughout.

Equally, the free server is **one
server**. If it is down — its machine
off, its disk dead, its network cut —
introductions stop. Sessions already
open continue, but nothing new can be
arranged until it is back (or restored
from chapter 12's backup, on another
machine, keeping the same key). The
public servers hide this problem
behind the project's fleet; a
self-hoster owns it, which is the
honest price of chapter 5.

## Networks can still say no

RustDesk is good at difficult
networks, not exempt from them. Some
NATs and some institutional firewalls
defeat hole punching entirely; those
sessions relay (chapter 2), and a
relayed session is bounded by the
relay: hbbr's defaults cap a single
connection's speed (32 Mb/s in the
lab's log) and the total bandwidth it
will spend, and the far side of a
slow uplink is slow whatever the
software wants. Networks that block
UDP outright, or only allow web
ports, can defeat registration itself
(short of the websocket arrangement of
chapter 11, which relays everything
and pays for it in speed). And a
machine that cannot reach *any*
server — yours or the public ones —
cannot be introduced at all; no
remote desktop tool crosses a total
network refusal, and claims otherwise
are marketing.

## Platforms have edges

**Linux and Wayland.** The standard
client on a Wayland session could
historically not be reached
*unattended* — nobody logged in, no
screen to share. The project's
separate `rustdesk-unattended-wayland`
build exists for it and is, by the
project's own description, a preview:
keep another way into such a machine
until you have proven it (chapter 3).
**macOS** gates screen and input
behind system permissions that an
update can reset. **iOS** can control
but never be controlled, by the
platform's own rule, not RustDesk's
choice. On every platform, reaching a
machine *before login* — the login
screen itself — needs the installed
service running with proper
privileges; the portable, run-it-once
builds cannot do it.

## The security limit is you

Chapter 1's rule is restated here as
a limit because it bounds what the
software can promise. RustDesk gives
you strong pieces — server keys,
client keys, hashed passwords, 2FA,
approval modes — and every one of
them can be configured away: a blank
key field, a permanent password of
"1234", a server with every port
open and the websocket pair exposed
bare (chapter 11), a saved password
on a borrowed laptop. The tools also
concentrate risk by design: one
successful guess at one weak permanent
password is not a toehold, it is the
whole machine, its files and its
signed-in accounts. There is no
version of this tool where that is
not true, because a door that opens
for you is a door. The limits chapter
of every honest remote-access guide
is the same sentence: the software
can make the door strong; it cannot
make you hang it carefully.

## Performance and fidelity, briefly

A remote desktop is a video of a
screen, compressed live. It is good
enough to work by — this guide's
readers do whole jobs through it —
and it is not the same as sitting
there: colour is approximated, fast
motion smears, audio sync is
approximate, and two monitors at full
resolution over a relay is an
ambitious request of a home uplink.
For pixel-true or frame-true work,
remote access is the scouting tool,
not the instrument.
