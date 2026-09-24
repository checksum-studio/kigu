# kigu privacy policy

kigu shows this policy on its first-run screen, and it is published as PRIVACY.md at
https://github.com/checksum-studio/kigu.

Version 1, 2026-09-24. Changes are listed at the end.

## Who is responsible

kigu is made by one person, known as **checksum.studio**, the controller for the data
described here.

- Contact for privacy, legal, security, abuse and support questions: **legal@checksum.studio**
- Bug reports: https://github.com/checksum-studio/kigu/issues

## The short version

- kigu has no accounts and no server of its own. Your video goes directly to the people you collab with, encrypted.
  The developer never receives, sees or stores your streams.
- To connect you, kigu uses relay and lookup servers run by number 0, Inc. (n0), the makers of iroh. They see your
  IP address and kigu's random node id, never your video.
- If "Send error reports" is on (the default), error and crash reports go to Sentry, with names, tickets, ids, IP
  addresses and file paths removed first. You can turn this off on the first screen or in Settings at any time.
- kigu checks GitHub for updates.
- kigu has no analytics, no tracking and no ads. The support reminder's counters stay on your PC.

## Every network connection kigu makes

This list is checked against the code of the release build. A connection that is not here is a bug; please report it.

| Where | When | What it sends and sees | Operator |
|---|---|---|---|
| Your collab partners' PCs (their IP addresses, UDP) | While you are connected to them | Your video (end-to-end encrypted QUIC), your display name, your node id, stream settings and link statistics. During connection setup the other side learns your IP addresses | The people you collab with |
| n0 relays: `use1-1.relay.n0.iroh.link`, `usw1-1.relay.n0.iroh.link`, `euc1-1.relay.n0.iroh.link`, `aps1-1.relay.n0.iroh.link` (HTTPS and QUIC, plus a plain-HTTP captive-portal check to `http://<relay>/generate_204`) | Whenever kigu runs: to keep a home relay, measure latency, check for a captive portal, learn your public address, and carry traffic when a direct path fails | Your public IP address and port, your node id, the node ids of the peers you exchange traffic with through it, and the timing and size of that traffic. Not the content: relayed traffic is end-to-end encrypted | number 0, Inc. |
| n0 DNS: `dns.iroh.link` (HTTPS `/pkarr`, and DNS queries through your normal DNS resolver) | Every 5 minutes while kigu runs (publish), and when you join a ticket (lookup) | A signed record "node id -> home relay URL"; no IP address is published. The server sees the IP address of whoever publishes or looks up | number 0, Inc. |
| Public DNS resolvers: Cloudflare `1.1.1.1`, Google `8.8.8.8`, Quad9 `9.9.9.9` | Only when your system's DNS fails to answer: iroh then retries the same lookups there | DNS queries for the relay host names and for `dns.iroh.link` names, which contain node ids (yours when publishing, a partner's when joining), and your IP address | Cloudflare, Inc.; Google LLC; Quad9 Foundation |
| Your own router (UPnP, NAT-PMP or PCP, on your local network only) | Whenever kigu runs | Asks your router to open a port for kigu so partners can reach you directly | You |
| Update feed: `github.com/checksum-studio/kigu/releases/latest/download/` (and the GitHub download host it redirects to) | Installer (from GitHub or itch.io) and portable-zip copies: at start and every 4 hours. Not a bare copied kigu.exe | Your IP address and a normal download request for `releases.<channel>.json`, and the update package when one exists | GitHub, Inc. (Microsoft) |
| Sentry ingest server (the host in kigu's built-in Sentry DSN, `*.ingest.sentry.io`) | Only while "Send error reports" is on, and only when an error, panic or crash happens (at most 20 reports per run) | The error report described below. Sentry sees your IP address; kigu's Sentry project is set not to store it | Functional Software, Inc. (Sentry) |

Settings that change this list: `relays` in `config.json` (or `--relay <url>`) replaces n0's relays with other
relays; n0 DNS is still used. `KIGU_UPDATE_URL` replaces the update feed.

Links kigu shows (GitHub, Discord, Ko-fi, the Spout2 plugin for OBS) open in your web browser only when you
click them. kigu itself does not contact those sites.

## What n0's relays and DNS see

kigu uses iroh, an open-source networking library, with the free public infrastructure of its makers, number 0,
Inc. (https://n0.computer). A relay sees your public IP address and port, your node id (a random public key that
kigu stores in `identity.key`), which other node ids you talk to through it, and when and how much you send. It
cannot read the traffic. n0's DNS server stores which relay your node id currently uses, so partners can find you
from your ticket; it does not store your IP address, but it sees the IP address of anyone who publishes or looks up
a record. n0's own privacy terms apply to their servers.

## What your collab partners see

A ticket is your node id. Anyone who has your ticket can ask to join. Before you are even asked to approve them,
kigu's connection setup exchanges network addresses to find a direct path, so **someone who tries to join learns
your IP address even if you deny them**. Share tickets only with people you trust. If a ticket leaks, press **New
ticket**: old tickets stop working. With `relay_only_ticket` on, tickets contain only the relay address, but the
address exchange during connection setup still happens.

Partners you let in receive your video and your display name, and can record what they receive.

## Error and crash reports (Sentry)

**When:** only while "Send error reports" is on, and only after you have answered the error-report question on the
first-run screen. Nothing is sent before that answer: errors and crashes from before it are dropped, not kept
for later. The box is ticked by default; you can untick it
there and turn it off in Settings at any time, and nothing is sent from the next event on. Builds made without a Sentry key (all
developer builds) never send anything.

**What is sent:**
- the error message, or for a panic the crash message, with a stack trace;
- for a native crash, a minidump: thread stacks, CPU registers and the list of loaded modules (no heap and no other
  memory contents);
- kigu's version and build, your Windows version and the CPU architecture (Sentry's standard device
  context), and the Rust version kigu was built with;
- recent warning and info log lines, and the last 200 KB of `kigu.log`.

**Removed before sending:** your Windows user name and profile path, other user folder names, display names (yours
and your partners'), tickets, node ids and other long hex ids, and IPv4 and IPv6 addresses. Your computer's name is
never sent. A minidump cannot be filtered and could, rarely, contain one of these in a stack.

**Why (legal basis):** our legitimate interest (GDPR art. 6(1)(f)) in finding and fixing crashes on hardware we
cannot test ourselves. You can object at any time by turning the setting off.

**Who processes it:** Functional Software, Inc. ("Sentry"), as our processor under its data processing addendum.
Reports are stored in Sentry's EU region (Frankfurt, Germany). Sentry may access them from outside the EU/UK for
support and operations; it relies on the EU-US Data Privacy Framework and standard contractual clauses for that.

**How long:** events are kept for 90 days and then deleted automatically.

## Updates

Installed and portable-zip copies of kigu download a small file from GitHub at start and every 4 hours to see if a new version
exists. GitHub sees your IP address and the request, as with any download. The request carries no identifier of
yours. The installer from itch.io is the same as the one on GitHub and checks the same way. A bare kigu.exe copied
elsewhere does not check.

## Supporting kigu (Ko-fi)

kigu may show a card asking for a tip after you have used it for many hours. The counters it uses (collab minutes,
sessions, dates of past asks) stay in `config.json` on your PC and are never sent anywhere. The Support button opens
https://ko-fi.com/checksum in your browser. Ko-fi (Ko-fi Labs Limited) is a separate service with its own privacy
policy; the developer receives from Ko-fi only what Ko-fi shows creators about a tip. kigu stores nothing about
supporters; "I've supported kigu" is a checkbox on your own PC.

## Data on your PC

kigu keeps its files in `%APPDATA%\kigu`: `identity.key` (your node id's secret key), `config.json` (settings,
display name, layouts, the Always-allow list with each partner's last display name, support counters), `logs\` (up to 5 log files of 10 MB) and
`crashes\` (crash dumps before upload). Nothing there leaves your PC except as described above.

Uninstalling kigu **keeps** `%APPDATA%\kigu`, so a reinstall keeps your ticket and settings. To delete it, use
**About > Remove my data...** before uninstalling, or delete the folder yourself.

## Your rights and how to use them

You can ask for access to, correction of, or deletion of your data, object to processing, and complain to a
supervisory authority. Because kigu has no accounts, the developer cannot tie a Sentry report to you unless you help:

1. Turn off "Send error reports" in Settings to stop new reports.
2. To have past reports deleted, email **legal@checksum.studio** with the approximate date and time of the crash
   and your kigu version (shown in About). We find and delete matching events in Sentry within 30 days and confirm
   by email. We will not ask for more than we need to find them.
3. For data held by n0, GitHub or Ko-fi, contact them directly (their policies are linked below); the developer has
   no access to it.

If you are in the EU or UK and are unhappy with our answer, you can complain to your local data protection
authority (in the UK, the ICO: https://ico.org.uk).

## Children

kigu's licence requires users to be at least 13, with a parent's permission under 18. kigu does not knowingly
collect more from minors than described here. A parent can ask for deletion as described above.

## Third-party policies

- number 0, Inc. (iroh relays and DNS): https://n0.computer
- Sentry: https://sentry.io/privacy/
- GitHub: https://docs.github.com/site-policy/privacy-policies/github-general-privacy-statement
- Ko-fi: https://more.ko-fi.com/privacy


## Changes to this policy

Changes are dated here, and a changed policy is noted in the release notes of the next kigu version.

- 2026-09-24: first version (DOC-1).
