# The Moving Parts

By the end of this chapter you can
name the four parts of a RustDesk
setup — the client, the ID server, the
relay, and the keys — say what each one
holds, and follow a connection from
typing an ID to seeing a desktop. When
something later breaks, this chapter is
what turns "it doesn't work" into a
named part misbehaving.

## The client: one program, two jobs

Every machine runs the same client. In
any single connection it plays one of
two roles. As the **controlled**
machine, it runs a background
**service** that announces "I am here,
my ID is this" and, when a connection
is approved, sends its screen and
accepts the incoming mouse and
keyboard. As the **controller**, it is
the window you sit at: you type an ID,
answer a password prompt, and drive.

The ID deserves a moment. It is a
number of nine or ten digits, derived
from the machine's own identity and
kept in the client's config folder, so
it survives restarts: the lab asked its
client for its ID twice, a minute
apart, and got the same number both
times (transcript
`rustdesk-lab/transcripts/t05-client-id-and-config.txt`).
An ID is an address, not a secret —
knowing one is like knowing a house
number. The password is the key, and
chapter 8 is about keys of that kind.

## The ID server: hbbs

Machines move. Laptops change networks,
home addresses change, and almost every
machine sits behind a router that hides
it. So a controller cannot simply "go
to" an ID. Instead, every controlled
machine **registers** with an **ID
server** — a program called **hbbs** —
telling it "this ID is at this address,
and here is my public key". That
registration, kept fresh by a regular
**heartbeat**, is what hbbs holds: a
small database (`db_v2.sqlite3`, in the
folder it runs from) mapping IDs to
addresses.

When you connect, your client asks
hbbs where the ID you want currently
lives. hbbs introduces the two machines
and — this is the clever part — helps
them open a **direct connection** to
each other, a technique called **hole
punching**, because both sides are
usually behind routers that would
refuse an unsolicited knock. If the
direct path opens, the desktop traffic
flows between the two machines
themselves, and hbbs's work is done
until the next introduction.

hbbs listens on three ports, each seen
in the lab's startup log (transcript
`rustdesk-lab/transcripts/t01-server-first-run.txt`):
**21115** (TCP) for a test that measures
what kind of network a machine sits
behind, **21116** (TCP and UDP — UDP
carries the registrations and
heartbeats, TCP carries hole punching
and connection setup), and **21118**
(TCP), a **websocket** port the web
client uses (chapter 11).

## The relay: hbbr

Sometimes hole punching fails — some
routers and networks are built in ways
that refuse it. Then the two machines
connect *outward* to a second program,
the **relay server**, **hbbr**, and the
desktop traffic passes through it,
encrypted end to end: hbbr forwards
bytes it cannot read. It listens on
**21117** (TCP) for relaying and
**21119** (TCP) for web-client
websockets, and its log names its
housekeeping defaults — a per-connection
speed limit (32 Mb/s in the lab's log)
and overall bandwidth caps, so one busy
relay cannot eat a whole connection.

A relayed session works exactly like a
direct one, only with a middleman's
latency and bandwidth. That is why
chapter 5 puts the relay on a machine
near yours, and why the session window
shows which kind of connection you got
(chapter 4): "relayed" is not a failure,
but it is information.

## The keys

Two key pairs keep this honest — both
**Ed25519** pairs, a private half that
must stay secret and a public half that
is meant to be shared.

**The server's pair** is born the first
time hbbs runs, as two small files in
its folder: `id_ed25519` (the private
half, 88 bytes) and `id_ed25519.pub`
(the public half, one line of text, 44
bytes — the lab's is in transcript
`rustdesk-lab/transcripts/t01-server-first-run.txt`).
hbbs announces its key in its log
(`Key: ...`). Clients are given the
public half, and it does two jobs:
it proves the server answering is
*your* server and not an impostor, and
it is what makes the session's
encryption possible (chapter 7 is
devoted to this pair).

**Each client's own pair** is generated
by the client on its first service run
and stored in its config. When the
client registers, it offers its public
key to the server (`register_pk`, in
the lab's service log, transcript
`rustdesk-lab/transcripts/t07-client-default-server.txt`)
— so the server, and later anyone
connecting, can tell that the machine
answering to an ID is the same machine
that registered it, not a stranger who
guessed the number.

## A connection, in order

1. Both machines' clients are running;
   the controlled one has registered
   its ID and address with hbbs, and
   keeps the registration warm.
2. You type the ID into your client.
   Your client asks hbbs: where is it,
   and what is its public key?
3. hbbs answers, and helps the two
   clients attempt a direct connection.
   Failing that, both connect to hbbr
   and the session runs relayed.
4. The far client checks your password
   (and, if it is set up that way, a
   second factor — chapter 8) against
   its own rules. Nothing about the
   password passes through hbbs.
5. The session opens: screen one way,
   keyboard, mouse, clipboard and the
   rest the other, encrypted between
   the two clients whether the path was
   direct or relayed.

## Who holds what, when it breaks

- The **client config** holds the ID,
  the password's hash, and which server
  this machine reports to.
- **hbbs** holds the map: which ID is
  where, and its own key pair and
  database, in the folder it runs from.
  Lose the folder and the map — and the
  key your clients trust — are gone
  (chapter 12 has the backup recipe).
- **hbbr** holds nothing at rest: only
  the traffic of the moment, which it
  cannot read.
- The **keys** hold the trust. The
  server's public key is how clients
  know the introduction service is
  yours; chapter 7 covers what happens
  when it changes.
