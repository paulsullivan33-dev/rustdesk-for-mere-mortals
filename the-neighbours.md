# The Neighbours

RustDesk is not the only way through
the door, and three neighbours deserve
honest comparison — the commercial
giant it is usually measured against,
the old open protocol it quietly
replaced for many people, and the
command line that was there before
both. This guide's task list named
them: TeamViewer, VNC, plain ssh.

## TeamViewer

The long-established commercial
remote desktop: polished clients
everywhere, an account, and *their*
servers doing the introducing and
relaying. For helping a stranger over
the phone it remains the frictionless
benchmark — no server of yours, no
ports, no key ceremony, and a brand
the person at the far end may already
trust. Its costs are the mirror of
RustDesk's freedoms: the introductions
happen on someone else's machinery
under someone else's account rules,
serious use is a subscription, and
free use has historically come with
commercial-use suspicions and
sessions cut short. RustDesk's
counter-offer is precisely chapter 5:
the same shape of tool, with the
server — and therefore the trust —
moved onto your own hardware, no
account required. What you give up
for that is TeamViewer's concierge
polish: the console, the managed
accounts, the support line (chapter
11 drew that line).

## VNC

**VNC** (Virtual Network Computing)
is the elder open approach: a protocol
(RFB) rather than a product, with
servers built into some systems and
clients everywhere. Its virtues are
real — it is simple, ancient,
universal, and on a desk-side network
it is often already there (macOS
Screen Sharing speaks it; Linux has
TigerVNC, x11vnc and friends).

What VNC does not bring is the rest of
this guide. There is no VNC hbbs: no
introductions, no hole punching, no
relay, so reaching a machine across
the internet means building the road
yourself — a VPN (this series has
Tailscale and WireGuard guides), an
ssh tunnel, a forwarded port with its
own exposure. Classic VNC passwords
are short and its encryption varies by
implementation, so the road matters.
And there is no VNC address book,
file-transfer window or server key
ceremony because there is no VNC
*system* — only a screen protocol,
honestly doing one job. Choose it when
the machines share a network you
already trust and reach; choose
RustDesk when they don't.

## Plain ssh

For a machine whose life is text —
most servers — **ssh** is not the
primitive neighbour, it is the correct
tool: a shell, file copying (`scp`,
`sftp`), port forwarding, keys instead
of passwords, forty years of polish,
and nothing graphical to be slow.
Chapter 9's terminal is RustDesk
bending toward ssh's ground.

ssh's limits are the desktop-shaped
ones. It shows no screen: helping a
person means describing and being
described to, and a program that only
exists as a window cannot be driven.
(X11 forwarding exists and is a
period piece; per-application remote
windows are not a desktop.) It also
asks the far machine to be reachable
— an ssh port exposed, or the same
VPN road-building as VNC. The honest
summary: for *your servers*, ssh
first, and this series has a guide to
it. For *desktops, other people's
machines, and anything with a face*,
RustDesk. Many careful setups run
both, each on its own ground, and use
the address book of one and the
`~/.ssh/config` of the other as the
same list kept twice.

## And the built-ins, in one breath

Windows Remote Desktop (RDP) and
macOS Screen Sharing ship with their
systems and are genuinely good inside
their own worlds — RDP especially,
fast and first-class on Windows
networks. They are neighbours of the
same kind as VNC: excellent on a
reachable network, silent about the
introductions across the internet
that are RustDesk's reason to exist.
