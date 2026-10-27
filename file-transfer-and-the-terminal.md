# File Transfer and the Terminal

This chapter covers the two working
tools that ride along with a desktop
session: moving files between the two
machines without the desktop in
between, and opening a command-line
shell on the far machine. By the end
you can pick the right one for a job —
and say why "just use the desktop" is
sometimes the wrong answer to both.

## File transfer

A desktop session can pass files by
copy and paste (the clipboard works
for files in many setups) — and for
one small file, do exactly that. The
dedicated tool is better for anything
more: from the session toolbar, or as
its own connection type from the main
window (the ID box's options offer
**Transfer File** alongside Connect),
you get a **two-pane file manager**:
your machine's folders on one side,
the far machine's on the other.

Select, send, watch the queue. The
transfer runs inside the same
encrypted, authenticated connection
as a desktop session — same password,
same approval, same direct-or-relayed
route — so a file crosses with
exactly the trust the desktop did, and
no extra service, account or open
port is involved. It is the answer to
the commonest real job in remote
access: *the document is on the other
machine*.

Its shape, honestly:

- It is a file *mover*, not a shared
  folder. Nothing is synchronised;
  send a newer copy over an older one
  and the older is replaced. For
  ongoing sharing between your own
  machines, a sync tool (this series
  has a Syncthing guide) is the right
  instrument — transfer is for now,
  by hand.
- Big trees of small files travel
  slowly compared with one archive.
  Zip first, on the far end, using the
  desktop or the terminal below.
- The controlled side can forbid it:
  file transfer is one of the
  per-session permissions in the
  security settings (chapter 8), and
  on a machine you only ever view for
  someone, off is a respectable
  setting.

## The terminal

The second tool is a shell. From the
session toolbar — or again as its own
connection type — **Terminal** opens a
command-line window on the far
machine: a real pseudo-terminal, in
the far machine's own shell (its
PowerShell or cmd on Windows, its
login shell on Linux or macOS),
running with the privileges of the
RustDesk service on that side.

Why it earns its place: a shell weighs
almost nothing. On a slow or relayed
connection where the desktop is a
slideshow, terminal text still flies;
for restarting a service, reading a
log, checking a disk — the jobs a
headless machine mostly needs —
typing is faster than driving a
desktop to open a terminal *in* it.
It is also the discreet tool: no
desktop lights up on the far screen
for a two-line fix, though the
connection itself is of course still
announced and approved exactly like
any other (the password, the 2FA if
set, the permissions of chapter 8 —
the terminal can be switched off
there too, and on machines whose
owner would not understand a shell,
off is again respectable).

Two cautions belong to the tool
itself. A shell in a window feels
low-stakes, but it is the whole
machine in text form: anything the
far user could do at that keyboard,
the shell can do, without the visual
feedback that makes desktop mistakes
obvious. Read the prompt before
pressing Enter; you are not on your
own machine, whatever the window's
familiarity suggests. And a terminal
session left open is a door left open
in the chapter 1 sense — close it
when the job is done, as you would
the desktop.

## Which tool, when

- One small file, desktop already
  open: clipboard.
- A folder, a log bundle, the only
  copy of something: the file manager,
  where you can see both ends and the
  queue.
- A service to restart, a log to
  read, a disk to check, a slow link:
  the terminal.
- The far machine is a server with no
  desktop worth the name: the
  terminal, and chapter 13 will point
  out that plain ssh is its neighbour
  and sometime rival for exactly this
  ground.
