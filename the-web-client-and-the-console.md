# The Web Client and the Console

Two things with "web" in their
neighbourhood are easily confused,
and this short chapter exists to
separate them: the **web client**,
which lets a browser be a controller,
and the **web console**, which is the
paid edition's server dashboard. By
the end you can say which one you
have, which one you don't, and why
the free server's silence on port
21114 is correct behaviour.

## The web client: a controller in a tab

The web client is the RustDesk
controller as a web page: open it,
enter an ID and password, and the
desktop session runs in the browser
tab. Nothing to install on the machine
you are sitting at — which is its
whole case: the borrowed laptop, the
locked-down work machine, the quick
check from a device that will never
have the client on it.

It speaks to your server differently
from the desktop client: over
**websockets**, which is what ports
**21118** (to hbbs) and **21119** (to
hbbr) exist for (chapter 5's table).
Three consequences follow:

- Reaching *your own* server from the
  hosted web client needs those
  websocket connections wrapped in
  proper HTTPS (WSS), normally through
  a **reverse proxy** on your server
  that takes the public 443 traffic
  and passes it to 21118 and 21119
  locally.
- The project's documentation carries
  a specific warning about those two
  ports, and it is chapter 1's rule in
  uniform: behind a reverse proxy,
  hbbs and hbbr trust the proxy's word
  for where a connection came from
  (the `X-Real-IP` header) *without
  checking it*. Anyone who can reach
  21118 or 21119 directly can forge
  that word. So: **websocket ports
  belong behind the proxy, reachable
  from nowhere else** — and if you do
  not use the web client at all, keep
  them closed, as chapter 5 advised.
- A web-client session is always
  relay-shaped (the browser cannot
  punch or be punched the way the
  desktop client can), so it inherits
  the relay's speed limits and your
  server's bandwidth.

Self-hosting the web client's page
itself — serving it from your own
domain rather than using the project's
hosted page — comes integrated with
the paid edition's server (below).
With the free server, the practical
arrangement is the project's hosted
web client, pointed at your server
through your WSS proxy.

## The console: not in the free box

The **web console** is something else
entirely: the server's administrative
dashboard — accounts and their login,
the synced and shared address books
(chapter 10), device groups, audit
logs of who connected to what, and
the custom client generator that bakes
your server settings into installers
(chapter 6). It belongs to **RustDesk
Server Pro**, the paid edition, and it
lives with the **API server** on port
**21114**.

The free server has no console, no
API, and no accounts — and the lab
has the receipt for that absence from
three directions: the checker tests
21114 on every run and reports ERROR
(transcript `rustdesk-lab/transcripts/t02-ports-and-doctor.txt`);
a client pointed at a free server
logs its API heartbeat failing,
`Connection refused`, on 21114
(transcript `rustdesk-lab/transcripts/t06-client-custom-server.txt`);
and nothing in hbbs's own flags or
logs ever mentions the port. Managing
the free server is therefore the
old-fashioned way — its folder, its
logs, its restarts (chapters 5 and
15) — which for one person's handful
of machines is not a hardship but a
description.

So the line to carry away: the web
client is a way to *connect*, and you
can use it with your free server
today, carefully, behind a proxy. The
console is a way to *administer*, and
it is the paid edition's. A guide that
pretended otherwise would be selling
you a server you did not download.
