# Recipes

Whole jobs, start to finish. Each
assumes chapter 1's rule — your
machines, your accounts — and names
the chapters it draws on.

## Help a relative, tonight, with nothing installed ahead of time

The public servers earn their keep
here (chapters 4 and 8):

1. Both of you install the client
   (chapter 3). No server settings are
   touched at all.
2. They read you their ID and one-time
   password over the phone — the
   one-time password exists for exactly
   this call.
3. Connect, fix the thing, talk them
   through what you did while their
   screen shows it.
4. Disconnect, and say that you have.
   If it will be a regular thing, set
   click-to-accept on their machine so
   future help always begins with
   their click, and leave no permanent
   password behind you.

## A screenless box in the corner, reachable for years

The unattended pattern, complete
(chapters 5 to 8):

1. Run your own server (chapter 5) on
   something always on; firewall to
   21115, 21116 TCP+UDP, 21117.
2. Point the corner box's client at it
   (chapter 6), key included (chapter
   7).
3. Set a long, unique permanent
   password; enable 2FA; note the
   recovery path (chapter 8).
4. Address-book it on your controller
   with a name and a tag (chapter 10).
5. Test from outside the house before
   you need it — a connection that has
   only ever worked from the same room
   has not yet met the internet.

## Prove a fleet really moved to your server

After a repointing round (chapter 6),
do not trust the settings screens;
trust the logs. On each machine, the
client's service log should name
*your* server in its mediator line,
and your server's database should
hold the registrations. A machine
whose log still names a public server
is a machine you missed — the two
maps never mix.

## Back up a server, and move it

The server's whole identity and memory
live in its run folder (chapter 5):

```bash
systemctl stop rustdesk-hbbs rustdesk-hbbr   # or your Docker stop
tar -czf rustdesk-server-backup.tar.gz /path/to/server-folder
```

The archive holds `id_ed25519`,
`id_ed25519.pub` and `db_v2.sqlite3`:
the key pair (so clients keep trusting
the server) and the registration
database (so IDs keep their history).
Guard the archive like the private
key it contains. To move servers:
unpack the same folder on the new
machine, run the same binaries there,
point the *address* (DNS name, if you
were wise) at the new home — clients
need no changes at all. This is why
chapter 6 prefers a hostname for the
ID server over a raw address: machines
move; names can be re-aimed.

## The key changed — recover cleanly

If a server was rebuilt without its
folder, its key is new and every
client distrusts it (chapter 7). The
recovery is mechanical, not dramatic:
read the new `id_ed25519.pub`, and
update the Key field on each client —
settings window, exported config, or
the `[options]` line of chapter 6 —
then confirm one connection per
machine. Do not respond to a key
mismatch by deleting the key from
clients "to make it connect": the
mismatch was the lock working.

## Keep a spare door closed

A periodic five-minute audit of a
self-hosted setup, against chapter
1's rule:

- Firewall shows only the ports of
  chapter 5's table — and 21118/21119
  closed unless the web client is
  genuinely in use behind its proxy
  (chapter 11).
- No client anywhere has a blank key
  field (chapter 7).
- No permanent password is shared
  between two machines, or with any
  other account (chapter 8).
- The server folder's backup is
  current, and stored where a leak of
  it would not be a leak of the key
  (above).
- The address book's saved passwords
  are only on controllers you would
  lend your house keys with (chapter
  10).
