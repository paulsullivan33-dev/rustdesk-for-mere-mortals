# The Key and Why It Matters

This chapter ends with you able to
answer three questions precisely:
what your server's key is, what a
client does with it, and what you must
do when it changes. Of everything in
the guide, this is the chapter whose
subject looks most like boilerplate
and matters most.

## The pair, once more

When hbbs first ran (chapter 5), it
made an **Ed25519 key pair** and wrote
it to its folder:

- `id_ed25519` — the **private key**.
  88 bytes of encoded text. It never
  leaves the server. It is not a
  configuration detail; it *is* the
  server, cryptographically speaking.
- `id_ed25519.pub` — the **public
  key**. One line, 44 characters of
  base64. It is meant to be copied
  into every client you own.

Everything in this chapter follows
from that split: the public half is a
name badge anyone may read; the
private half is the only thing in the
world that can prove the badge
genuine.

## What the client does with the key

When you set the key in a client
(chapter 6), two protections switch
on.

**The server must prove itself.**
Introductions are valuable: whoever
runs the ID server decides where
"your machine" currently is, and a
session's setup keys are agreed
through it. A client holding your key
checks the server against it. An
impostor server — a mistyped address,
a hijacked name, a hostile network's
substitute — fails the check, and the
client treats the connection as not
confirmed rather than quietly trusting
the stranger. The lab's log language
is exact about this state:
`register_pk ... due to key not
confirmed` is a client that has a key
to check against and has not finished
checking yet.

**The session is encrypted.** The
project's own instruction is blunt:
the key is *required for encrypted
connections to your self-hosted
server*. Sessions between clients of
your server are encrypted end to end —
through a relay (chapter 2) the
encryption is between the two clients,
and hbbr passes bytes it cannot read.
Leave the key out of a client and you
have told it to accept introductions
it cannot verify, for sessions it
cannot properly seal. There is no
configuration in which that is the
careful choice.

The clients' own key pairs (chapter 2)
complete the pattern: each client
registers *its* public key with the
server under its ID, so the machine
that answers a connection can be
checked against the machine that
registered. Keys all the way down —
server vouched for by the key you
carried to the client yourself,
clients vouched for by keys the
server recorded.

## Making and checking a pair ahead of time

hbbs makes its pair on first run, but
the server download's third program
can make one deliberately — useful
when preparing a server you will
start later, or replacing a pair on
purpose:

```bash
rustdesk-utils genkeypair
```

```text
$ rustdesk-utils genkeypair
Public Key:  DS6BuofcWjFBt2KyuwHKHD+vco7MSORtnra5YKV5ito=
Secret Key:  kKVikCFA/b7PuD1UskZZbeQCy44b...
```

(Transcript `rustdesk-lab/transcripts/t04-keypair-utils.txt`,
where the pair shown was made for the
occasion and thrown away — the only
safe way to print a secret key in a
book.) The companion check answers the
question "do these two halves belong
together?" — worth asking after a
restore from backup:

```bash
rustdesk-utils validatekeypair <public> <secret>
```

```text
Key pair is VALID
```

A wrong pairing answers `Key pair is
INVALID`, and a malformed key is
refused before any comparison. Place
a pre-made pair in the server's
folder as `id_ed25519` and
`id_ed25519.pub` before first start,
and hbbs adopts it as its own.

## When the key changes

The key changes when the
`id_ed25519` file does: a fresh
install, a rebuilt server, a restored
folder from *before* the pair was
made. The consequences are immediate
and deliberately unsubtle — every
client still holding the old public
key can no longer confirm the server,
and says so. This is the system
working: from the client's seat, "my
server got a new identity overnight"
and "something is impersonating my
server" are the same observation, and
only one of them is innocent.

So treat a key change as a small
ceremony, never a surprise: generate
or restore the pair, read the new
`id_ed25519.pub`, and update the key
in every client (chapter 6's routes)
as part of the same job. And make the
ceremony rare: back up the server
folder — pair and database together
(chapter 12) — so that a dead disk is
an errand, not a re-keying of every
machine you own. The private half, in
backups as everywhere else, stays
inside your own custody: a backup
that leaks `id_ed25519` leaks the
server's identity itself, and the
remedy then is a new pair and the
full ceremony, on purpose this time.
