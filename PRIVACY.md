# kigu privacy policy

kigu shows this policy on its first-run screen, and it is published as PRIVACY.md at
https://github.com/checksum-studio/kigu.

Version 1, effective 2026-09-25. Changes are listed at the end.

## Who is responsible

kigu is made by one independent developer, known as **checksum.studio**. We are the controller for error reports
and for emails you send us. We choose which services kigu contacts, but n0, GitHub, the DNS providers, Ko-fi and
itch.io are independent controllers of what they receive.

- Privacy, legal, abuse and support questions: **legal@checksum.studio**
- Security problems: private vulnerability reporting on https://github.com/checksum-studio/kigu (Security tab), or
  **security@checksum.studio**
- Bug reports: https://github.com/checksum-studio/kigu/issues

## The short version

- **You can say no to error reports.** They are sent on the basis of our legitimate interest, and you have the right
  to object at any time: untick "Send error reports" on the first-run screen or in Settings. Nothing more is sent
  from then on.
- kigu has no accounts and no server of its own. Your video goes directly to the people you collab with, encrypted.
  The developer never receives, sees or stores your streams.
- To connect you, kigu uses relay and lookup servers run by number 0, Inc. (n0), the makers of iroh. They see your
  IP address and kigu's random node id, never your video.
- If "Send error reports" is on (the default), error and crash reports go to Sentry, with names, tickets, ids, IP
  addresses and user folder names removed from the text first.
- kigu checks GitHub for updates.
- kigu has no analytics, no tracking and no ads. We do not sell or share personal information, and we do not use it
  for advertising. The support reminder's counters stay on your PC.

## Every network connection kigu makes

kigu contacts nothing until you have accepted the licence agreement on the first screen. After that it makes only
the connections below.

This list is checked against the code of the release build. A connection that is not here is a bug; please report it.

| Where | When | What it sends and sees | Operator | Why (legal basis) |
|---|---|---|---|---|
| Your collab partners' PCs (their IP addresses, UDP) | While you are connected to them, or trying to connect | Your video (end-to-end encrypted QUIC), your display name, your node id, the node ids of the other people in the collab, your stream names and settings, whether your GPU can decode kigu video, and link statistics. During connection setup the other side learns your IP addresses, even if they are refused | The people you collab with | Providing the collab you ask for (art. 6(1)(b)) |
| n0 relays: `use1-1.relay.n0.iroh.link`, `usw1-1.relay.n0.iroh.link`, `euc1-1.relay.n0.iroh.link`, `aps1-1.relay.n0.iroh.link` (HTTPS and QUIC, plus a plain-HTTP captive-portal check to `http://<relay>/generate_204`) | Whenever kigu runs: to keep a home relay, measure latency, check for a captive portal, learn your public address, and carry traffic when a direct path fails | Your public IP address and port, your node id, the IP addresses and node ids of the peers you exchange traffic with through it, and the timing and size of that traffic. Not the content: relayed traffic is end-to-end encrypted | number 0, Inc. (USA) | Providing the connection you ask for (art. 6(1)(b)) |
| n0 DNS: `dns.iroh.link` (HTTPS `/pkarr`) | When kigu starts, whenever your home relay changes and every 5 minutes (publish); when kigu dials a partner, including other collab members (lookup) | A signed record "node id -> home relay URL"; no IP address is published. The server sees the IP address of whoever publishes or looks up | number 0, Inc. (USA) | As above |
| Your normal DNS resolver (usually your internet provider or router) | Whenever kigu looks up a relay or a partner | DNS queries for the relay host names and `dns.iroh.link` names, which contain node ids | Whoever runs your DNS | As above |
| Public DNS resolvers: Cloudflare (`1.1.1.1`, `1.0.0.1`), Google (`8.8.8.8`, `8.8.4.4`), Quad9 (`9.9.9.9`, `149.112.112.112`) and their IPv6 addresses, over plain DNS and DNS-over-HTTPS | Only when your system's DNS fails to answer: iroh then retries the same lookups there | The same DNS queries, and your IP address | Cloudflare, Inc.; Google LLC; Quad9 Foundation | As above |
| Your local network (UPnP discovery is multicast, so other devices on your network can see it; NAT-PMP and PCP go to your router) | Whenever kigu runs | Asks your router to open a port for kigu so partners can reach you directly | You | As above |
| Update feed: `github.com/checksum-studio/kigu/releases/latest/download/` (and the GitHub download host it redirects to) | Installed and portable-zip copies with Beta updates off: at start and every 4 hours until an update is found. Not a bare copied kigu.exe | Your IP address and a normal download request for `releases.<channel>.json` with kigu's name and version, and the update package when one exists | GitHub, Inc. (Microsoft, USA) | Our legitimate interest, and yours, in keeping kigu secure and working (art. 6(1)(f)) |
| Beta release discovery: `api.github.com/repos/checksum-studio/kigu/releases` and that release's feed/download assets | With Beta updates on, instead of the stable feed, at start and every 4 hours until an update is found | Your IP address and an unauthenticated releases request; no account, token or user identifier | GitHub, Inc. (Microsoft, USA) | As above |
| Sentry ingest server (the host in kigu's built-in Sentry DSN, in Sentry's EU region) | Only while "Send error reports" is on, and only when an error, panic or crash happens (at most about 20 reports per run) | The error report described below. Sentry sees your IP address; kigu's Sentry project is set not to store it | Functional Software, Inc. (Sentry) | Our legitimate interest in finding and fixing bugs (art. 6(1)(f)); see below |

Settings that change this list: `relays` in `config.json` (or `--relay <url>`) replaces n0's relays with other
relays; n0 DNS is still used. `KIGU_UPDATE_URL` replaces the update feed. A file named `kigu-no-updates.txt` next
to kigu.exe or Update.exe turns update checks off; `KIGU_UPDATE_URL` also replaces Beta discovery. `--headless` runs
never check for updates and do not show the licence screen (they are for scripted use).

Links kigu shows (GitHub, Discord, Ko-fi) open in your web browser only when you click them. kigu itself does not
contact those sites.

## What n0's relays and DNS see

kigu uses iroh, an open-source networking library, with the free public infrastructure of its makers, number 0,
Inc. (https://n0.computer). A relay sees your public IP address and port, the IP addresses of the partners you exchange traffic with through
it, your node id (a random public key that
kigu stores in `identity.key`), which other node ids you talk to through it, and when and how much you send. It
cannot read the traffic. n0's DNS server stores which relay your node id currently uses, so partners can find you
from your ticket; it does not store your IP address, but it sees the IP address of anyone who publishes or looks up
a record.

n0 runs its public relays as an independent service. Its documentation
(https://docs.iroh.computer/concepts/security-privacy and https://docs.iroh.computer/iroh-services/relays/public)
says relays see connection metadata and are monitored for abuse, and describes them as intended for development and
hobby use, with no uptime guarantee. n0's privacy page for its services is https://services.iroh.computer/privacy. We do not control or receive this data and do not know how long n0 keeps it. If this
matters to you, you can run your own relay and set it in `relays`.

## What your collab partners see

A ticket is your node id. Anyone who has your ticket can ask to join. Before you are even asked to approve them,
kigu's connection setup exchanges network addresses to find a direct path, so **someone who tries to join learns
your IP address even if you deny them**. Share tickets only with people you trust. If a ticket leaks, press **New
ticket**: old tickets stop working. With `relay_only_ticket` on, tickets contain only the relay address, but the
address exchange during connection setup still happens.

Everyone in a collab connects directly to everyone else, so **each member learns the IP address of every other
member**, including people someone else invited.

Partners you let in receive your video and your display name, and can record what they receive. **Your display name
starts as your Windows user name**; change it on the first-run screen or in Settings if you do not want partners to
see it. kigu shares each partner's layer with OBS over Spout under the name `kigu - <their display name>`, which
other apps on your PC can see (your own name too if you show yourself in the composite).

## Error and crash reports (Sentry)

**When:** only while "Send error reports" is on, and only after you have answered the error-report question on the
first-run screen. Nothing is sent before that answer: errors and crashes from before it are dropped, not kept
for later. The box is ticked by default; you can untick it there and turn it off in Settings at any time, and
nothing is sent from the next event on. Builds made without a Sentry key (all developer builds) never send anything.
If you are under 18, we recommend unticking it; a parent can do so in Settings.

**What is sent:**
- the error message, or for a panic the crash message, with a stack trace;
- for a native crash, a minidump: thread stacks, CPU registers and the list of loaded modules (no heap and no other
  memory contents);
- kigu's version and build, your GPU model and driver version (from the log), your Windows version and the CPU architecture (Sentry's standard device
  context), and the Rust version kigu was built with;
- up to 50 recent warning and info log lines, and the last 200 KB of `kigu.log`.

**Removed before sending:** from all text in the report: your Windows user name and the user folder part of file
paths, other user folder names, display names (yours and your partners'), tickets, node ids, UUIDs and other long
hex ids, and IPv4 and IPv6 addresses. Your computer's name is never sent. **A minidump cannot be filtered:** its
module list contains file paths that usually include your Windows user name, and its stacks could contain other
text kigu was handling at the moment of the crash.

**Why (legal basis):** our legitimate interest (GDPR art. 6(1)(f)) in finding and fixing crashes on hardware we
cannot test ourselves. We weighed this against your interests: reports carry no video, are scrubbed, are kept
for at most 90 days and are used for nothing else, and you can stop them at any time. **You have the right to object** by
turning the setting off. In the UK, we rely on the exception in the Privacy and Electronic Communications
Regulations for preventing or detecting technical faults in the service you asked for (Schedule A1, paragraph
4(2)(d)). Because nothing is sent until you have answered the error-report question on the first-run screen, you
always decide before any report leaves your PC.

**Who processes it:** Functional Software, Inc. ("Sentry"), as our processor under its data processing addendum
(https://sentry.io/legal/dpa/; sub-processors: https://sentry.io/legal/subprocessors/). Reports are stored in
Sentry's EU region (Frankfurt, Germany). Sentry may access them from outside the EU/UK for support and operations; it
relies on the EU-US Data Privacy Framework and its UK Extension, with standard contractual clauses and the UK
Addendum as a fallback. Only the developer can see the
Sentry project. We do not copy reports anywhere else except, with personal details removed, into a bug report.

**How long:** Sentry deletes events automatically after at most 90 days.

## Updates

Installed and portable-zip copies of kigu download a small file from GitHub at start and every 4 hours to see if a
new version exists. Beta updates first query GitHub's releases API, including prereleases, so no stable release is
needed. The request includes the app name and version and no identifier of yours: the updater's own random staging
ID (kept in its install folder) is not sent. GitHub sees your IP address and the request, as with any download.
The installer from itch.io is the same as the one on GitHub and checks the same way. A bare kigu.exe copied
elsewhere does not check.

## Supporting kigu (Ko-fi)

kigu may show a card asking for a tip after you have used it for many hours. The counters it uses (collab minutes,
sessions, install date, when and how often it last asked) stay in `config.json` on your PC and are never sent
anywhere. The Support button opens https://ko-fi.com/checksum in your browser. Ko-fi (Ko-fi Labs Limited) is a
separate service with its own privacy policy; the developer receives from Ko-fi only what Ko-fi shows creators about
a tip. kigu stores nothing about supporters; "I've supported kigu" is a checkbox on your own PC.

## Data on your PC

kigu keeps its files in `%APPDATA%\kigu`: `identity.key` (your node id's secret key), `config.json` (settings,
display name, capture source, window position, layouts, the Always-allow list with each partner's last display name,
support counters) and backups of a damaged `config.json`, `logs\` (up to 5 log files of 10 MB, plus a few from a
second copy of kigu; they contain names and addresses and stay on your PC unless you send them) and `crashes\`
(a crash dump exists only while error reports are on, and is deleted as soon as it has been handed to Sentry,
whether or not the upload succeeded). **Help > Copy diagnostics** saves your newest logs, scrubbed, to a zip file
you choose; nothing is sent. **Copy invite** puts your ticket on the clipboard. The installer puts kigu, downloaded updates and the updater's staging
ID in `%LOCALAPPDATA%\kigu` and registers `kigu://` links for your Windows account. While error reports are on, a
second, windowless kigu.exe runs to catch crashes, with a small file in `%TEMP%` for talking to it. Nothing here leaves your PC except as described above.

Uninstalling kigu removes the program and the link registration but **keeps** `%APPDATA%\kigu`, so a reinstall
keeps your ticket and settings. To delete it, use **About > Remove my data...** before uninstalling (it deletes kigu's files there and closes kigu), or
delete the folder yourself.

## Your rights and how to use them

Depending on where you live, you can ask for access to, correction or deletion of your data, restriction of its
processing, or a copy in a portable format, and you can object to processing based on legitimate interest. You do
not have to give us any data: error reports are optional, and without relays you may be unable to reach some
partners. We make no automated decisions or profiles about you.

Because kigu has no accounts, the developer cannot tie a Sentry report to you unless you help (GDPR art. 11):

1. Turn off "Send error reports" in Settings to stop new reports.
2. To have past reports deleted, email **legal@checksum.studio** with the approximate date and time of the crash
   and your kigu version (shown in About). We find and delete matching events in Sentry within 30 days and confirm
   by email. We will not ask for more than we need to find them.
3. For data held by n0, GitHub, the DNS providers or Ko-fi, contact them directly (their policies are linked below);
   they are independent of us and the developer has no access to it.

We keep emails you send us for up to 2 years after your request is closed, then delete them.

You can complain to a data protection authority, for example in the EU
(https://www.edpb.europa.eu/about-edpb/about-edpb/members_en) or in the UK (the ICO: https://ico.org.uk).

## International transfers

The developer is in the United States. n0 and GitHub are US companies, so your IP address and the other data
described above go to the US when kigu contacts them, under their own terms. Sentry transfers are described above.
Because our processing is occasional, limited and unlikely to put your rights at risk, we have not appointed an EU
or UK representative (GDPR art. 27(2)).

## Security

Your video and all partner traffic are end-to-end encrypted. Reports are scrubbed before they leave your PC, and
the Sentry account is protected by two-factor sign-in. If a breach affects your data, we will notify the authorities
and affected users where the law requires and where we can identify them.

## Children

kigu's licence requires users to be at least 13, with a parent's permission under 18. kigu does not knowingly
collect more from minors than described here. A parent can ask for deletion as described above.

## Do Not Track

kigu does no tracking, so there is nothing for a Do Not Track signal to switch off.

## Third-party policies

- number 0, Inc. (iroh relays and DNS): https://services.iroh.computer/privacy and
  https://docs.iroh.computer/concepts/security-privacy
- Sentry: https://sentry.io/privacy/
- GitHub: https://docs.github.com/site-policy/privacy-policies/github-general-privacy-statement
- Cloudflare 1.1.1.1: https://developers.cloudflare.com/1.1.1.1/privacy/public-dns-resolver/
- Google Public DNS: https://developers.google.com/speed/public-dns/privacy
- Quad9: https://quad9.net/privacy/policy/
- Ko-fi: https://more.ko-fi.com/privacy
- itch.io (optional payment when you download): https://itch.io/docs/legal/privacy-policy

## Changes to this policy

Each change is listed here with its date, and the current version is always published at the address above. A
change that sends new kinds of data or sends data to new recipients comes with a new version of the licence
agreement, which kigu shows, with this policy, before you can continue.

- 2026-09-25: first version.
