# Troubleshooting

Arranged by what you see. The
through-line of the whole chapter:
RustDesk's parts log plainly and the
doctor exists, so most failures can
be *named* before they are fixed —
and a named failure is usually one
setting, not a reinstall.

## "Not ready" — the client never registers

The status line never reaches *Ready*,
or drops away. The client cannot
complete registration with its ID
server. Work outward in:

1. **Is it the server you think it
   is?** Read the client's service
   log (`~/.local/share/logs/RustDesk/server/`
   on Linux): the mediator line names
   the server it is actually trying.
   If it names the public server and
   you expected yours, chapter 6 was
   not applied to this machine.
2. **Can it reach the port?** From the
   client machine, `rustdesk-utils
   doctor <your-server>` — 21116 must
   answer. Remember 21116 is TCP *and*
   UDP; a firewall that passed the TCP
   half and dropped UDP gives exactly
   this symptom (chapter 5).
3. **Is hbbs itself running?** On the
   server: process present, its log
   past the listening lines, and not
   the failure in the next entry.

## hbbs exits immediately after "Start"

A real one — this guide's lab met it.
hbbs finishes its startup by testing
itself: it sends its own UDP port a
small packet and must hear itself. If
the machine (or a sandbox, container
network, or firewall policy) refuses
UDP sends *at all* — even to
127.0.0.1 — the test fails and hbbs
exits with code 1, leaving a log that
simply stops after `Start`, or, in
some builds, one ERROR line naming
the failed test:

```text
ERROR [src/rendezvous_server.rs:181] Failed to run hbbs test with 0.0.0.0:21116: Operation not permitted (os error 1)
```

The fix is never in RustDesk: give
the machine working UDP (a real
network interface, a firewall that
permits loopback and outbound UDP),
and hbbs starts. Transcript
`rustdesk-lab/transcripts/t03-hbbs-self-test.txt`
shows the diagnosis — debugger trace
and a three-line UDP test that fails
with no RustDesk code involved — so
you can confirm the same cause rather
than guessing. hbbr, needing no UDP,
is unaffected and will happily run
alone; do not let its health convince
you the pair is up.

## The client says the key is not confirmed, or the connection "is not secure"

The client's key field (chapter 6) is
empty, stale, or not the key of the
server it is talking to. Causes, in
order of likelihood: the client was
never given the key; the server was
rebuilt and quietly made a *new* pair
(chapter 7's ceremony — update every
client); or the client is pointed at
a different server than the key came
from (a mistyped address — the check
working as designed). The
`[keys_confirmed]` table in the
client's `RustDesk.toml` shows which
servers it has confirmed; an empty
table on a machine that "used to
work" is the rebuilt-server story
told in one glance.

## doctor says ERROR on port 21114

Expected, forever, on the free
server. 21114 is the paid edition's
API and console port; the free hbbs
has nothing there, and doctor tests
it anyway (transcript
`rustdesk-lab/transcripts/t02-ports-and-doctor.txt`).
The same applies to the client log's
`Heartbeat failed ... 127.0.0.1:21114
... Connection refused` warning when
a client is pointed at a free server
(transcript `rustdesk-lab/transcripts/t06-client-custom-server.txt`):
it is the client politely knocking on
a door the free server does not have.
Fix nothing; recognise it.

## Connections work, but everything relays

Both machines register, sessions open,
but the connection information always
says *relayed* and speed suffers.
Direct connections are being refused
by the networks involved: symmetric
NATs at one or both ends, UDP blocked
somewhere in the path, or a router's
"features" interfering. The relay is
doing its job — this is a quality
problem, not an outage. The levers,
in order: open UDP 21116 properly at
both ends (above); put hbbr on a
machine nearer the path; accept the
relay for what it is on genuinely
hostile networks (hotel and
institutional Wi-Fi relay a lot, and
chapter 14 explains why that is a
limit, not a fault).

## It works at home and nowhere else

The server is at home and so was
every test. From outside, the ID
server address resolves to the home
network's *inside* name or address,
or the router was never told to
forward the ports, or the home
connection's address changes and the
clients hold a stale number. The
durable fixes are chapter 5's: a
public name for the server (dynamic
DNS if the address wanders), the
port-forwards set on the router, and
one test from a genuinely outside
network before the trip — the recipe
in chapter 12 exists because this
surprise is otherwise discovered from
an airport.

## The far machine connects but shows a black screen, or cannot be driven

The session is up; the picture or the
control is not. Platform territory:
macOS has revoked or never granted
Screen Recording / Accessibility
(chapter 3 — re-grant and restart the
client); a Linux Wayland machine is
in the unattended case chapter 14
names (use the purpose-built package,
or have the machine logged in);
Windows is showing a secure screen
(a login or consent prompt) that a
merely portable — not installed —
RustDesk cannot draw: install the
service properly. In each case the
connection was fine; it is the
platform's permission that is the
missing part, and reinstalling
RustDesk will not grant it.
