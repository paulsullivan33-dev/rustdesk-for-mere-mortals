# RustDesk for Mere Mortals

Sometimes the computer you need is not
the one in front of you. **RustDesk**
puts the far machine's desktop in a
window on yours — your mouse and
keyboard drive it — and it does so with
a piece of machinery most remote
desktop tools keep on their own
computers: the small server that
introduces two machines and relays for
them when they cannot reach each other
directly. RustDesk's offer is that the
machinery can be **yours**: the same
client, and free open-source server
software — **hbbs** and **hbbr** — that
you run on your own hardware.

This guide explains how the parts fit:
the client and its ID, the ID server
and the relay, and the **key** that
makes a server recognisably *yours*.
It takes you through a first connection
on the public servers, then through
running your own server, pointing every
client at it, and locking the result
down — permanent passwords, approval
modes, two-factor authentication. The
limits are stated plainly, including
the honest line between the free server
and the paid edition whose console and
accounts it does not have.

The rule that governs everything here
is stated in chapter 1 and returned to
in chapter 14: **remote access is a
door into the machine — your own
machines, your own accounts, strong
passwords, and a server only as exposed
as it needs to be.** The lab obeyed it
strictly. Every program it ran was on
one machine, talking to itself.

The lab behind this guide ran the real
thing: the official **rustdesk-server
1.1.16** binaries and the **client
1.5.0** AppImage, on a small Linux
machine with no display. Against them
it watched the server pair make its key
pair and its database and announce
every port; ran the project's own
`doctor` against the result; made and
validated key pairs with
`rustdesk-utils`; read the client's ID
from its command line, twice, and got
the same number; watched the client's
service write its config files, then
repointed the client at the lab's own
server by editing the file it wrote —
and saw its log move house, from the
public rendezvous server to 127.0.0.1,
and begin registering its key there.

What the lab could not do, it did not
fake. The sandbox it ran in refuses all
UDP traffic — even to the machine's own
loopback — and hbbs tests itself at
startup with a UDP packet to itself, so
the ID server could not stay up there;
the debugger trace of that wall, and a
three-line proof that the wall is the
sandbox's and not the software's, are
kept as transcripts. With no display,
no graphical session was ever opened:
the connection itself, and everything
that happens inside one, is taught from
the project's documentation and
labelled as such wherever it appears.
`criteria.md` keeps the full ledger of
what was run and what was only read.

## What this guide covers

- [**What RustDesk is**](what-rustdesk-is.md) —
  the picture and the rule: a desktop in
  a window, two ways of running the
  machinery, and why a door is the right
  metaphor
- [**The moving parts**](the-moving-parts.md) —
  the client and its ID, hbbs the ID
  server, hbbr the relay, and the two
  key pairs that hold the trust
- [**Installing the client**](installing-the-client.md) —
  the one program on every platform, the
  service it installs, and reading your
  ID before you ever connect
- [**Your first connection**](your-first-connection.md) —
  driving one of your machines from
  another over the public servers, and
  reading the session window properly
- [**Running your own server**](running-your-own-server.md) —
  hbbs and hbbr on your own hardware:
  the first run, the key pair, the six
  ports and which ones to open
- [**Pointing clients at your server**](pointing-clients-at-your-server.md) —
  the three values every client needs,
  by settings window and by config file,
  and the log line that proves it took
- [**The key and why it matters**](the-key-and-why-it-matters.md) —
  what the server's public key proves,
  what it encrypts, and the ceremony
  when it changes
- [**Passwords and two-factor**](passwords-and-two-factor.md) —
  one-time and permanent passwords,
  approval modes, and the authenticator
  code behind the password
- [**File transfer and the terminal**](file-transfer-and-the-terminal.md) —
  the two-pane file mover, and a shell
  on the far machine for the jobs a
  desktop is too slow for
- [**Address books and tags**](address-books-and-tags.md) —
  named machines instead of memorised
  numbers, and where the free book ends
- [**The web client and the console**](the-web-client-and-the-console.md) —
  a controller in a browser tab, the
  websocket ports it needs, and the
  console the free server does not have
- [**Recipes**](recipes.md) — helping a
  relative tonight, the screenless box
  for years, backing up and moving a
  server, the spare-door audit
- [**The neighbours**](the-neighbours.md) —
  TeamViewer, VNC and plain ssh: what
  each does that RustDesk doesn't, and
  vice versa
- [**Limits**](limits.md) — the small
  free server, networks that say no,
  platform edges, and the limit that is
  the operator
- [**Troubleshooting**](troubleshooting.md) —
  the client that never registers, the
  server that exits after "Start", the
  key that will not confirm, and the
  error on port 21114 that is not one
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Reaching machines that are not yours —
that is not a harder chapter of this
book, and chapter 1 explains why.
Running remote access for a whole
organisation, with accounts, audit
trails and policies, is the paid
edition's territory and is pointed at,
not taught; this guide stays with one
person, their own machines, and the
free server. Defeating other software's
protections, hiding sessions from a
machine's owner, and exposing other
people's desktops are likewise out —
the tool announces itself on the far
screen by design.

## How to use this guide

Read chapter 1 for the picture and the
rule, and chapter 2 before installing
anything — knowing which part holds
what turns later confusion into a named
problem. Take the first connection of
chapter 4 in the same sitting as the
install, with both machines in one
room; do not debut with a machine an
ocean away. Chapters 5 and 6 are a
pair: run the server, then repoint
every client, and check each one's log
rather than trusting the settings
screen. Chapter 7 is not the optional
chapter: understand the key before your
server matters to you, because the day
it changes unexpectedly is the wrong
day to learn what it was doing. The
lab's transcripts live in
`rustdesk-lab/transcripts/`; its notes
are `examples/lab-notes.txt` and its
distilled commands `examples/commands.txt`.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Tailscale for Mere Mortals](https://paulsullivan33-dev.github.io/tailscale-for-mere-mortals/)
- [SSH for Mere Mortals](https://paulsullivan33-dev.github.io/ssh-for-mere-mortals/)
- [WireGuard for Mere Mortals](https://paulsullivan33-dev.github.io/wireguard-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/rustdesk-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/rustdesk-for-mere-mortals/blob/main/LICENSE-CODE).
