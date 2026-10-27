# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's traffic is the lab talking to itself.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 14 names what the tool
   cannot do: the free server has no accounts, console or
   audit trail; one server is one point of failure; NATs and
   networks that refuse UDP defeat direct connections and
   sometimes registration itself; unattended Wayland needs a
   special build; iOS cannot be controlled; and no setting
   makes a badly hung door — weak passwords, an over-open
   firewall — safe.
8. **The ethics rule is in the text, early.** Chapter 1 and the
   README state the rule plainly — *remote access is a door
   into the machine: your own machines, your own accounts,
   strong passwords, and a server only as exposed as it needs
   to be* — before any procedure depends on it, and later
   chapters point back to it where the tooling would make
   crossing the line easy.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a 2-CPU, ~7 GB Ubuntu 24.04 sandbox with
no display. Everything was downloaded from the project's
official GitHub releases: rustdesk-server **1.1.16** (the
`hbbs`, `hbbr` and `rustdesk-utils` binaries from the Linux
amd64 archive) and the RustDesk client **1.5.0** (the x86_64
AppImage, extracted rather than installed, and run with its
home folder inside the lab). All transcripts live in
`rustdesk-lab/transcripts/`.

Run for real, with transcripts as evidence:

- Versions and help: `hbbs --version`, `hbbr --version`,
  `hbbs --help`, `rustdesk-utils --help`, and the client's
  `--version` printing 1.5.0 (t00).
- The server pair's first run in an empty folder: the key
  pair written to `id_ed25519` / `id_ed25519.pub` (with the
  public key read back), the registration database created,
  every "Listening on" line from both programs — 21115,
  21116, 21118 for hbbs; 21117, 21119 for hbbr — the
  relay-servers line honouring `hbbs -r 127.0.0.1:21117`,
  and hbbr's bandwidth defaults from its own log (t01).
- hbbr running for the whole lab: its sockets in `ss`, a
  TCP probe connecting and receiving the protocol's empty
  reply, and `rustdesk-utils doctor` against the machine —
  hbbr's ports OK, the stopped hbbs's ports ERROR, and
  21114 ERROR as it is on every free server (t02).
- Key tooling: `rustdesk-utils genkeypair` producing a pair,
  `validatekeypair` answering VALID for it, INVALID for a
  deliberate mismatch, and refusing a malformed secret —
  the pair printed in the transcript was made for the
  occasion and discarded (t04).
- The client headless: `--get-id` printing 473182205,
  twice, identically; the config folder it created, with
  `RustDesk.toml`'s fields read back (enc_id, empty
  password and salt, key_pair empty before the service
  ran and populated after) and `RustDesk2.toml` as the
  service itself wrote it (t05).
- The client's service, against the default public server:
  its log's mediator lines for rs-ny.rustdesk.com, its
  identity key pair being generated, `register_pk`
  attempted, and the service retrying after refusal
  rather than dying (t07).
- The client repointed at the lab's server by editing
  `RustDesk2.toml`'s `[options]` (custom-rendezvous-server,
  relay-server, key — the lab server's real public key):
  the service log's mediator lines move to 127.0.0.1 and
  `register_pk of 127.0.0.1:21116` is attempted; the same
  log captures the client's API heartbeat failing on
  21114 against the free server (t06).

Documented only — described from the project's
documentation, not run, and the text does not claim
otherwise:

- **Every graphical session.** The sandbox has no display,
  so no desktop was ever opened, driven, recorded or
  screenshotted for this guide. Chapter 4's connection,
  the session toolbar, chapter 9's file manager and
  terminal, chapter 8's settings-window procedures and
  chapter 10's address book are documented from the
  project's own documentation; the lab verified the
  client, service, config and logs *around* the session,
  never the session itself, and no claim in the text
  rests on having seen one.
- **A completed registration or connection, anywhere.**
  The sandbox refuses all UDP sends — even to 127.0.0.1,
  proven with a UDP echo test containing no RustDesk
  code — so no client registration with any hbbs (the
  lab's or the public one) ever completed. The attempts,
  the log lines they produced, and the retry behaviour
  are real and transcribed (t03, t06, t07).
- **hbbs running as a service.** hbbs's startup self-test
  sends its own UDP port a packet; with UDP refused,
  hbbs exits with code 1 immediately after "Start". The
  lab verified hbbs to that threshold — key generation,
  database, listening announcements, relay discovery —
  and the diagnosis itself, by debugger trace, is kept
  as t03 and taught as troubleshooting in chapter 15.
  hbbr, needing no UDP, is the fully run half.
- **Setting a permanent password.** The documented
  command (`rustdesk --password`) refused the lab's
  unpackaged client — "Installation and administrative
  privileges required!" — and the `--set-password`
  spelling fell through to the (display-less) GUI.
  Password storage's *shape* was verified (the salted
  hash fields in `RustDesk.toml`); setting one was not.
- Installing the client by package (deb/rpm), the
  systemd or Docker deployment of the server, macOS and
  Windows permission flows, the web client in a browser,
  and the Pro edition's console, accounts and generator.
- The comparison tools in chapter 13 are characterised,
  not benchmarked.

Fixtures are this guide's own and deliberately generic: one
throwaway sandbox machine, a server key pair and a client ID
generated inside it and discarded with it, one extra key pair
made only to be printed, and one invented lab password used
only to watch a command refuse it. No example is modelled on
any real person, network, service or organisation, and no
machine was connected to, or attempted to be connected to,
that the lab did not itself run.
