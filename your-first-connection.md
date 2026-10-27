# Your First Connection

By the end of this chapter you have
driven one of your machines from
another — seen its desktop, moved its
mouse, typed on it — using the public
servers and about five minutes. Do
this once with both machines in the
same room and the far one's owner (you,
or someone helping you) beside it. The
first connection is a ceremony worth
attending; later ones can be
unattended, and chapter 8 prepares
them properly.

An honest note before the steps. This
guide's lab ran on a machine with no
display, in a sandbox that also blocks
the registration traffic itself — so
the *session* in this chapter is
described from the project's
documentation, not performed, and
`criteria.md` keeps that ledger. Every
piece around the session — the client,
its ID, its service, its logs — the lab
did run, and the chapters before and
after this one show the transcripts.

## What you need

- RustDesk installed on both machines
  (chapter 3), the far one showing
  **Ready** at the bottom of its
  window.
- The far machine's **ID** and the
  **one-time password** under it. The
  person at that end reads them to you,
  or you wrote them down while you were
  sitting there. The one-time password
  is short — a handful of characters —
  and it is *not* a weak link to be
  embarrassed by: it refreshes, and it
  is only half of the door's lock, the
  other half being that the far machine
  must be running, registered, and
  willing.

## The connection

1. On your machine, type the far ID
   into the **Control Remote Desktop**
   box and press **Connect**.
2. A password prompt appears. Enter the
   one-time password. (There is a box
   to remember it — leave it unticked
   on a machine that is not yours.)
3. The far desktop opens in a window.
   Your mouse and keyboard now belong
   to it. If the far machine is set to
   ask first, someone there sees an
   accept prompt instead, and the
   session starts when they click
   **Accept** — for a first connection
   with a helper at the far end, that
   is the friendliest setting of all,
   and chapter 8 shows where it lives.
4. Move the mouse. Open something.
   Type a sentence. Check the small
   things people forget to test: the
   keyboard layout (does the far end
   read your keys as the right
   characters?), and the clipboard —
   copy a line of text on one side and
   paste it on the other.

## Reading the session window

The session window has a toolbar
(pulled down from the top) that the
rest of the guide keeps meeting:

- **Connection information** — the
  panel that says whether this session
  is **direct** or **relayed**
  (chapter 2), and shows the padlock of
  an encrypted session. Relayed plus a
  padlock is a perfectly good session;
  it is the route, not the safety, that
  the word describes.
- **Display controls** — scaling (fit
  the far screen into your window, or
  scroll at full size), quality
  presets, and choosing which monitor
  to look at when the far machine has
  two.
- **The working tools** — file transfer
  and the terminal (chapter 9), text
  chat with the far end, session
  recording, and the button that ends
  the session.

Check: you know the connection was
real and healthy when the connection
information names its route (direct or
relayed), the padlock shows the
session is encrypted, and text copied
on one machine pastes on the other.
If any of those fail, chapter 15 is
arranged by symptom.

## Manners and the rule, once more

End the session yourself rather than
leaving it hanging; a forgotten open
session is a door left ajar. If you
were helping someone, say when you are
done and that you have disconnected —
the far screen shows your presence the
whole time, which is a feature: RustDesk
sessions are attended by design on the
far end's screen, and the one-time
password you used has now been spent
or will refresh away. Nothing in this
chapter needed an account, a purchase,
or anyone's permission but the machine
owner's — which is exactly the shape
chapter 1 promised, and the shape the
rest of the guide works to keep while
moving the machinery onto your own
server.
