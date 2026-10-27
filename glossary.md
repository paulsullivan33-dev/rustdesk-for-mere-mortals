# Glossary

**Address book** — the client's saved
list of machines: ID, name, tags, and
optionally a stored password hash.
Kept per client; synced books need an
account on an API server (chapter 10).

**API server** — the paid edition's
accounts-and-console service, on port
21114. The free server has none —
doctor's ERROR there is permanent and
correct (chapters 5 and 11).

**Approval mode** — how a controlled
machine meets a correct password:
connect at once, require a click at
the far end, or require both
(chapter 8).

**Client** — the one RustDesk program
installed on every machine; it is both
the controller you drive from and the
service you are reached through
(chapter 2).

**Controlled / controller** — the
machine being driven, and the machine
doing the driving, in a given session.

**Direct connection** — a session
whose traffic flows between the two
clients themselves, after hbbs
introduces them (chapter 2).

**Doctor** — `rustdesk-utils doctor
<address>`, the project's own checker:
it tests a server's ports and reports
each as OK or ERROR (chapter 5).

**hbbs** — the ID server (rendezvous
server). Holds the map of IDs to
addresses, registers clients,
introduces them, helps them punch
direct connections (chapter 2).

**hbbr** — the relay server. Passes
encrypted session traffic for
connections that cannot go direct;
holds nothing at rest (chapter 2).

**Heartbeat** — the regular small
message by which a registered client
keeps its entry in hbbs's map fresh.

**Hole punching** — the technique by
which two machines behind routers open
a direct path to each other, with
hbbs coordinating the attempt
(chapter 2).

**ID** — a machine's number: nine or
ten digits, derived from its identity
and stable across restarts. An
address, not a secret (chapter 3).

**Key (server key)** — the Ed25519
pair hbbs makes on first run:
`id_ed25519` private in the server
folder, `id_ed25519.pub` public in
every client. Proves the server is
yours and makes encrypted sessions
possible (chapter 7).

**Key pair (client)** — each client's
own Ed25519 pair, generated on its
first service run and registered with
the server under its ID (chapter 2).

**NAT** — the router trick by which
many machines share one public
address; the reason introductions and
hole punching exist. **NAT type** is
hbbs's measure of how punchable a
network is, taken on port 21115.

**One-time password** — the short,
refreshing password shown under the
ID, for attended help (chapter 8).

**Permanent password** — the password
you set, for unattended access;
stored only as a salted hash
(chapter 8).

**Pro (RustDesk Server Pro)** — the
paid server edition: API server, web
console, accounts, shared address
books, custom client generator
(chapter 11).

**register_pk** — the client's act of
offering its public key to the server
under its ID, seen by that name in
its log (chapter 6).

**Relayed connection** — a session
whose traffic passes through hbbr,
encrypted end to end (chapter 2).

**Rendezvous** — the meeting-arranging
itself; hbbs's other name is the
rendezvous server, and the client's
registration worker is its rendezvous
mediator.

**rustdesk-utils** — the small tools
shipping with the server: `genkeypair`,
`validatekeypair`, `doctor`
(chapters 5 and 7).

**Service** — the client's background
half: registers the ID, accepts
approved connections, runs even when
nobody is logged in (chapter 3).

**STUN** — a public echo service the
client uses to learn what kind of
NAT it sits behind ("Testing nat",
in the lab's logs).

**Tag** — a label an address-book
entry can carry several of, for
filtering the book (chapter 10).

**Trusted device** — a controller a
2FA-protected machine remembers, so
it is not challenged for a code each
time (chapter 8).

**Two-factor authentication (2FA)** —
requiring, beyond the password, a
current code from an authenticator
app (TOTP) to connect (chapter 8).

**Unlock** — the authentication step
before the client's security and
network settings can be changed
(chapters 6 and 8).

**Web client** — the controller as a
web page, speaking to the server over
websockets on ports 21118 and 21119
(chapter 11).

**Web console** — the Pro edition's
administrative dashboard, on port
21114 (chapter 11).

**Websocket** — a web-style connection
carrying the RustDesk protocol, so
browsers (and proxied setups) can
reach the servers on 21118/21119.
