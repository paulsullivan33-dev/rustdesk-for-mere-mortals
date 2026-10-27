# Installing the Client

This chapter ends with RustDesk
installed on a machine, its service
running, and its ID read out and
written down — the three things every
later chapter assumes. The install is
the same program on every platform;
only the packaging differs.

## Getting it, and only from the source

Download the client from the project's
own site (rustdesk.com) or its GitHub
releases page — nowhere else. A remote
desktop tool from a random download
site is chapter 1's rule already
broken: you would be installing a door
whose keys you did not cut. The
release page lists a build for every
platform: an installer or portable
executable for Windows, a disk image
for macOS, packages for the Linux
families (`.deb`, `.rpm`, Arch and
openSUSE packages), an **AppImage** and
Flatpak for Linux generally, an Android
APK, and the iOS app through the App
Store. iOS is the one asymmetric
platform: an iPhone or iPad can
*control* other machines but cannot be
controlled itself.

## The install, per platform

**Windows.** Run the downloaded
executable. It can be used portably,
but choose the install: installing also
sets up RustDesk as a **service**, and
the service is what lets the machine be
reached when nobody is logged in, and
answer connection requests with proper
privileges. Unattended access (chapter
8) depends on it.

**macOS.** Open the disk image and drag
RustDesk to Applications, then grant
the permissions it asks for —
Accessibility and Screen Recording
among them. Without them macOS lets
the app *see* nothing and *drive*
nothing; the prompts on the app's left
side walk through them.

**Linux.** Install the package for
your family (`sudo apt install ./rustdesk-<version>.deb`
and its cousins), which likewise
installs a systemd service. The
AppImage needs no install at all: make
it executable and run it — which is
what this guide's lab did, below. One
Linux wrinkle is worth knowing before
it bites: on the newer **Wayland**
display system, an unattended machine
— nobody logged in — is not reachable
by the standard build. The project
publishes a separate
`rustdesk-unattended-wayland` package
for exactly that case; chapter 14
returns to it.

**Android.** Install the APK (or the
store build). Controlling another
machine works at once. Being controlled
needs extra permissions — screen
capture and the accessibility service —
which the app requests when you enable
its sharing.

## First run: meet your ID

Start RustDesk. The main window is
deliberately plain. At the top is
**your ID** — the big number — and
beneath it a **one-time password**.
On the right is a box for the ID of a
machine you want to reach, and at the
bottom a status line: *Ready*, once
the client has registered with a
server. On the public servers that
message adds a suggestion — that a
server of your own would be faster.
Chapters 5 and 6 take the hint; ignore
it happily until then.

The service behind the window is the
part that matters for being reached:
once installed, it starts with the
machine, registers the ID, and waits.
The window is how you drive; the
service is how you are found.

Check: the client's identity survives
a restart and does not depend on the
window being open. From a terminal,
even with no display at all, the ID
can be read directly — the lab did
exactly this with the AppImage build:

```bash
rustdesk --get-id
```

```text
$ rustdesk --get-id
473182205
```

Asked twice, the answer was the same
number both times (transcript
`rustdesk-lab/transcripts/t05-client-id-and-config.txt`).
That number, plus a password, is all
chapter 4 needs.

## Where the client keeps its things

On Linux the client's home is
`~/.config/rustdesk/` (on Windows,
`%APPDATA%\RustDesk\config`; on macOS,
`~/Library/Preferences/com.carriez.RustDesk`).
The lab watched the folder being born:
the first runs created
**`RustDesk.toml`** — the machine's own
record: its encrypted ID, the password
and salt fields (chapter 8), and its
key pair once the service has generated
one — and the service's first run added
**`RustDesk2.toml`**, where your
settings live, including which server
this machine reports to (chapter 6
edits it by hand). Logs live nearby
(on Linux, `~/.local/share/logs/RustDesk/`),
and chapter 15 will send you there:
when RustDesk misbehaves, its own log
usually says why, in sentences.

Treat the config folder as mildly
secret. It holds no readable password —
those are stored hashed — but it does
hold the machine's identity keys, and
a copied identity is a impersonated
machine (chapter 7).
