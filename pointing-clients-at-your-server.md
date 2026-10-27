# Pointing Clients at Your Server

This chapter ends with a client whose
service reports to *your* hbbs instead
of the public one — and with the check
that proves it, from the client's own
log. It must be done on every machine,
controller and controlled alike: a
machine that still reports to the
public servers cannot be introduced to
one that reports to yours.

Three values are the whole job:

- **ID Server** — your hbbs machine's
  address (a hostname or IP; add
  `:21116` only if you changed the
  port). Required.
- **Relay Server** — your hbbr's
  address. Often safe to leave blank:
  hbbs tells clients where its relay is
  (that was the `-r` of chapter 5).
- **Key** — the public half of your
  server's key pair: the contents of
  `id_ed25519.pub`, one line. Required
  for an encrypted connection to your
  server — chapter 7 explains what
  rides on it.

A fourth field, **API Server**, is for
the paid edition's account system.
With the free server, leave it blank.

## The settings window route

On the machine to configure: open
RustDesk, click the menu button (⋮)
beside the ID, then **Settings →
Network**. The network settings are
locked against accidents: click
**Unlock** and authenticate (an
administrator password on most
systems). Fill in ID Server and Key,
Relay if you are setting it, then
**Apply**. The status line at the
bottom of the main window returns to
*Ready* — now meaning ready on *your*
server. That is the whole procedure,
and for a handful of machines it is
the right one.

## The config file route, verified

The settings window writes a file, and
the file can be edited directly — the
route the lab verified, because it
leaves evidence. The settings live in
`RustDesk2.toml` (chapter 3), and the
server values are three entries under
its `[options]`:

```toml
[options]
custom-rendezvous-server = '127.0.0.1'
relay-server = '127.0.0.1'
key = 'Orc9URZ29Q4UegoYOPySQ3F48YhkjzASpOvGKfImh4M='
```

(127.0.0.1 only because the lab's
server shared the lab's machine; yours
will name your server's address.) The
key in that example is the lab server's
real public key, from chapter 5 — a
public half is meant to be passed
around, and chapter 7 separates what
may be shared from what may not.

Check: restart the client's service
and read its log
(`~/.local/share/logs/RustDesk/server/`
on Linux). Before the edit, the log's
registration lines name the public
server:

```text
INFO [src/rendezvous_mediator.rs:659] start rendezvous mediator of rs-ny.rustdesk.com
INFO [src/rendezvous_mediator.rs:369] start udp: rs-ny.rustdesk.com:21116
```

After the edit, the same lines name
yours:

```text
INFO [src/rendezvous_mediator.rs:659] start rendezvous mediator of 127.0.0.1
INFO [src/rendezvous_mediator.rs:369] start udp: 127.0.0.1:21116
INFO [src/rendezvous_mediator.rs:1309] register_pk of 127.0.0.1:21116 due to key not confirmed
```

Both are real lab lines (transcripts
`rustdesk-lab/transcripts/t07-client-default-server.txt`
and `rustdesk-lab/transcripts/t06-client-custom-server.txt`).
The third line deserves its sentence:
`register_pk` is the client offering
the server its own public key under
its ID — the moment your hbbs learns
not just where this machine is but
*which* machine it is. ("due to key
not confirmed" is the client noting it
has not yet confirmed *the server's*
key; a completed registration records
the confirmation in `RustDesk.toml`'s
`[keys_confirmed]` table, where the
lab's stayed empty for the sandbox
reasons in chapter 5.)

## The routes for many machines

For more than a few machines, per-
machine clicking stops being a plan.
The same settings travel by: **Export
Server Config** in one configured
client's Network settings, which
produces a string another client
**imports** from the same screen; the
command line — `rustdesk --config
<string>` applies an exported config,
and `--import-config <file>` applies
a `RustDesk2.toml`; and, in the paid
edition, a custom client generator
that bakes the values into the
installer itself. The free server's
honest fleet tool is the exported
config plus whatever you already use
to run commands on your machines.

## The failure this prevents

Do this on *every* machine. The common
first-day confusion of self-hosting is
two machines, one repointed and one
not, appearing not to see each other:
each is correctly registered — with
different introducers, who keep
different maps. Chapter 15 lists it by
symptom; here it is enough to say the
maps are per-server, and your server
only introduces machines that report
to it.
