# What RustDesk Is

This chapter gives you the whole
picture in one sitting: what RustDesk
does, the two ways of running it, and
the rule that governs everything after.
By the end you can say what the tool
is, what it is not, and why this guide
spends so much of its length on a
server most users never think about.

Sometimes the computer you need is not
the one in front of you. The file is on
the machine at home. The program only
lives on the one at the office. Someone
you love is staring at an error message
they cannot describe, in another town.
**Remote desktop** software answers
this: the far computer's screen appears
in a window on yours, and your mouse
and keyboard drive it, as if you were
sitting at its desk.

**RustDesk is a remote desktop you can
run entirely yourself.** One program —
the **client** — is installed on both
machines: the one you sit at (the
**controller**) and the one you reach
(the **controlled** machine). Each
machine has a number, its **ID**, and a
password. Type the far machine's ID
into yours, give the password, and its
desktop opens in front of you. Behind
that simple exchange sits a small piece
of machinery — a server that introduces
the two machines to each other — and
RustDesk's distinguishing offer is that
the machinery can be yours: free,
open-source server software you run on
your own hardware, so introductions and
even the traffic itself need never pass
through anyone else's computers.

## The two ways of running it

Out of the box, the client uses
RustDesk's **public servers** — machines
the project runs for everyone. That is
the right way to start (chapter 4):
install on two machines, swap IDs, and
you are looking at one desktop from the
other in minutes. Nothing to configure
and nothing to pay.

The second way is to run the server
yourself (chapters 5 and 6). Your
machines register with *your* server,
are introduced by it, and relay through
it when they must. You do this for
privacy (the introductions happen on
your hardware), for independence (your
remote access survives whatever happens
to a free public service), and for
speed (a server near your machines
relays faster than one an ocean away).
The client is identical either way;
only the address it reports to changes.

## The rule, stated before everything else

A remote desktop is not like a window
you look through. It is a door you walk
through — with the keys. Whoever
connects to a machine can do anything
the person sitting at it can do: read
its files, use its accounts, install
its software. That is the tool's whole
usefulness and its whole danger, and it
is why this guide has one rule, stated
here and in the README, before any
procedure depends on it:

> **Remote access is a door into the
> machine, so treat it as one: your own
> machines, your own accounts, strong
> passwords, and a server only as
> exposed as it needs to be.**

Helping a relative with their machine,
with them on the phone and the password
read out for the occasion: inside the
rule — it is their machine and their
account, opened with their knowledge.
Connecting to a machine that is not
yours, or leaving yours reachable behind
a weak password "just for tonight": the
rule exists for exactly those moments,
and chapter 14 returns to what they
risk. The lab behind this guide obeyed
the rule strictly: every program it ran
was on one machine, talking to itself.

## Who it is for

The person with a computer in two
places and a life in one of them. The
family's unpaid help desk, tired of
diagnosing by telephone. The owner of a
small always-on machine in a corner —
a home server, a media box — that has
no screen of its own and still needs
driving sometimes. The common thread is
one person and their own machines,
wanting the desktop itself, not just a
command line.

## What it is not

RustDesk is not a VPN: it does not join
networks together or carry your other
traffic, it carries a desktop (its
neighbours do the network jobs —
chapter 13). It is not a way to watch
anyone: a session is visible on the far
machine's screen, its presence is
announced, and chapter 1's rule covers
the rest. It is not magic about
networks: if two machines cannot be
introduced and no relay can reach them
either, there is no connection, and
chapter 14 is honest about the corners
where that happens. And the free server
is not a management platform: it
introduces machines and relays traffic,
and the accounts, consoles and policies
of the paid edition are a different
product (chapter 11 draws that line
exactly).
