# Address Books and Tags

Two machines need no organisation.
Six — yours, the family's, the box in
the corner, the one at the other site
— and you are keeping IDs in a text
file and passwords in your head, which
is where mistakes breed. This chapter
covers the client's built-in answer:
an **address book** of named machines,
organised with **tags**, and is honest
about the line where the free tool's
book ends and the account-backed one
begins.

## The book itself

The main window keeps your machines
in lists: **Recent** sessions (everyone
you have connected to, whether you
meant to keep them or not), and the
**Address Book** proper — entries you
saved on purpose. Saving an entry
records what a connection needs:

- the **ID** (and, if the machine sits
  behind your own server with a direct
  address you use, its alias),
- a **name** you choose — the point of
  the exercise: *the corner box*, not
  *184 203 551*,
- **tags** (next section),
- optionally the machine's permanent
  password, stored hashed as it is
  everywhere in RustDesk, so a double
  click connects without typing — weigh
  that convenience with chapter 8's
  care, because a saved password makes
  *this* client's config folder worth
  stealing.

An entry can also remember the small
things per machine: which monitor you
usually open, the quality you prefer
for its link. The book lives in the
client's config folder, in per-peer
files, so it moves when the folder
moves and is backed up when the folder
is (chapter 12's backup recipe covers
the server's folder; the client's
deserves the same habit).

## Tags

A **tag** is a label an entry can
carry several of: `family`, `servers`,
`site-north`, `do-not-wake`. Clicking
a tag filters the book to its members.
Tags beat folders here precisely
because they overlap — the corner box
is both `servers` and `home`, and a
folder would force a choice. Two
habits make tags earn their keep:
name them for the *question you will
ask later* ("which of these may I
restart at night?"), and keep the set
small enough to remember without
looking.

## Where the free book ends

The book described so far is **this
client's own**: kept locally, visible
only here. Sign in to an **account**
— on RustDesk's public service, or on
an API server — and the book can
**synchronise** between your clients,
so the laptop and the desktop show
the same machines. That sentence hides
the line this guide has drawn before:
the free hbbs of chapter 5 is not an
API server. It has no accounts (its
port 21114 silence, chapter 5, is the
sound of that absence). Synced and
*shared* address books — a book a
whole team or household uses, device
groups, central management of who sees
what — belong to the paid edition's
side of the fence, where the API
server and the web console live
(chapter 11).

For the free self-hoster the honest
options are: one book per client,
kept where it is used; or the config
folder carried between your own
machines the way chapter 6 carries
server settings. For one person and
their own handful of machines — this
guide's reader — the local book with
good names and two or three tags is
not the consolation prize. It is the
tool, at the size the job actually
is.
