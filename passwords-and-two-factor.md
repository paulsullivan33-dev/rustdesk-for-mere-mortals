# Passwords and Two-Factor

This chapter ends with the far end of
your connections set up the way
unattended access demands: a strong
permanent password where you choose
one, a clear decision about who may
connect with what, and — for the
machines that matter most — a second
factor standing behind the password.
Chapter 1 called remote access a door.
This chapter is its locks.

## The two passwords

A controlled machine answers to two
different passwords, and confusing
them causes most of the trouble in
this area.

The **one-time password** is the short
one shown under the ID in the main
window. It is for attended help: read
it out, use it once, and it refreshes
— after the session, or on a timer you
choose in the security settings. Its
safety comes from that short life and
from the machine being attended, not
from length; chapter 4's connection
ran on one.

The **permanent password** is one you
set, and it stays. It is what makes
**unattended access** possible —
reaching a machine with nobody beside
it: the always-on box in the corner,
your own desktop while you travel.
Everything careful in this chapter is
about it, because a permanent password
is a standing invitation to try the
door, from anywhere the server can be
reached.

Set it in **Settings → Security**,
under the password options, after
unlocking the security settings.
Choose it like a password that guards
a whole computer, because it does:
long, unique to this machine (not your
email password, not the same as your
other machines'), and kept in a
password manager rather than in
memory. Current versions will warn
about weak choices; treat a warning as
a veto.

How it is stored matters and is
checkable: the client's `RustDesk.toml`
holds `password` and `salt` fields
(transcript `rustdesk-lab/transcripts/t05-client-id-and-config.txt`),
and what lives there is a **salted
hash** — a fingerprint of the password
that can verify a guess without being
the password, salted per machine so
two machines' hashes never match even
for the same word. There is no file on
the machine from which the password
itself can be read back out.

One command-line route exists, with a
stern limit the lab verified:
`rustdesk --password <password>` sets
a permanent password, but only where
RustDesk is properly *installed*, with
its service and administrative
privileges — run from the lab's
unpackaged AppImage it refused
outright:

```text
$ rustdesk --password labpass123
Installation and administrative privileges required!
```

The settings window is the route this
guide teaches; the command is for
scripted installs on machines that are
already properly set up.

## Who may connect, and how they are met

The security settings also decide how
a correct password is *met*. The
approval modes, in increasing
strictness:

- **Password only.** The permanent (or
  one-time) password connects at once.
  The mode unattended access needs —
  and the mode that makes the password
  the whole lock.
- **Click to accept.** Someone at the
  far machine sees the request and
  clicks to allow it. No password
  knowledge helps an intruder if nobody
  there cooperates — and no unattended
  access is possible at all.
- **Both.** Password *and* a click:
  the password proves knowledge, the
  click proves presence.

The same settings page gates the
session's powers per controller —
keyboard and mouse, clipboard, file
transfer, audio, the terminal — so a
machine you only ever *look at* for a
family member need not accept file
transfer at all. Grant per need, not
per habit: chapter 1's "only as
exposed as it needs to be", applied
inside the session.

## The second factor

For a machine reachable through a
server on the open internet, a
password — even a strong one — is a
single lock on a door that can be
tried quietly and at scale. RustDesk's
answer is **two-factor authentication
(2FA)** for connections: on top of the
password, the controller must present
a six-digit code from an
**authenticator app** (the TOTP kind —
the code changes every thirty seconds
and is generated from a secret shared
at setup, never transmitted since).

Set it up at the machine itself, in
**Settings → Security**, in the 2FA
section: the client shows a QR code,
you scan it with the authenticator app
on your phone, and you confirm with a
first code. From then on, connecting
to that machine asks for the password
*and* a current code. Machines you
connect from regularly can be marked
**trusted devices** — the client's
config has a `trusted_devices` field
for exactly this — so your own laptop
is not challenged every time, while a
stranger with the password still is.
Trusted devices trade a little of the
factor's strength for a lot of its
usability; trust few, and only devices
that are themselves locked.

Keep the recovery story in mind
before you need it: lose the phone
with the authenticator and an
unattended machine's second factor
becomes a wall with you outside it.
Save the setup's recovery material
where you keep the password, and make
sure at least one path back in — a
trusted device, physical access —
exists before you rely on 2FA for a
machine you cannot walk to.

## The shape of a careful setup

For the machine in the corner that
only you ever reach: own server
(chapter 5), key set (chapter 7),
permanent password that is long and
unique, approval by password, 2FA on,
and the server's firewall no wider
than chapter 5's table. For the
relative's machine you help with:
public servers are fine, no permanent
password at all, one-time passwords
read out per session, click-to-accept
on. Both are "RustDesk set up
properly" — the rule is not maximum
strictness everywhere, it is strictness
fitted to the door's exposure, chosen
on purpose.
