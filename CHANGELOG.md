# Changelog

### 1.7.1 Clearer startup diagnostics and a smoother update experience
2026-07-21

### New Features
- Installer: added an option (checked by default, also offered on every update) to install the latest Microsoft Visual C++ Runtime, needed by PHP and some other bundled tools
- Notifications: the message bar now pauses its auto-dismiss timer while your mouse is over it, and restarts the countdown once you move away, so it won't disappear before you've had a chance to read it or copy the message

### Improved
- Startup errors: nginx, Apache, PHP, and MySQL/MariaDB now report the real reason when they fail to start (e.g. a missing Visual C++ runtime DLL) instead of a generic "did not come up in time" timeout
- Web servers: reduced the startup wait for nginx, Apache, and PHP from 15s to 10s
- Databases: reduced the startup wait for MySQL/MariaDB from 60s to 15s
- Updates modal: each version in "What's changed" can now show a title, subtitle, small image, and a "Read more" link, in addition to the New Features/Improved categories it already had

---

### 1.7.0 | Safer Foundations, Better Database Workflows and Community Catalogues
2026-07-18

Read the story behind v1.7.0 at https://forgekit.tools/articles/forgekit-v1-7-big-chungus

### New Features
- phpMyAdmin: redesigned the panel with runtime controls (start, stop, restart) and quick-open buttons for its config and log files
- phpMyAdmin: panel now also shows the connected database server's own my.ini and error log, alongside phpMyAdmin's runtime files
- phpMyAdmin: added a large imports folder - drop a big SQL file in and pick it from the Import tab instead of uploading it through the browser
- Database servers: added a Logs dropdown next to Config on each database panel, with quick-open access to its mysql-error.log
- Updates modal: now shows what changed in every version between yours and the latest, not just the newest release, broken out per version with a link to the full changelog if you're several versions behind
- Binaries: added support for an external binary catalog URL as an alternative to custom-binaries.json, with a simple radio switcher in Manage Binaries to pick which one is active
- Jobs/Downloads tray:  added a Cancel button to in-progress jobs (e.g. an update download), which aborts the work and cleans up any partial file. Also moved to bottom right so it's more out of the way


### Improved
- phpMyAdmin: fixed large SQL imports failing with a maximum execution time error
- Database: tuned MySQL/MariaDB settings so imports and exports run noticeably faster
- Updates modal: removed unnecessary channel/minimum-supported info
- Updates modal: Close/Download/Install bar now stays pinned to the bottom instead of scrolling away with the release notes above it
- Updates modal: clicking Download twice in a row no longer starts a second download; the button now reads "Downloading…" until the file is actually ready
- Binaries: fixed a bug where a custom binary (e.g. Node, PHP) installed with the same ID as an official one could disappear from the Official section
- Binaries: custom binaries now show whether they came from custom-binaries.json or your catalog URL, via a badge and hover tooltip, everywhere you pick a binary (not just Manage Binaries)
- Binaries: custom catalog entries whose ID collides with ForgeKit's official catalog now show a clear warning instead of silently not appearing
- Jobs tray: fixed the loading spinner being invisible in both light and dark mode (its highlight color barely differed from the tray background)
- Logs: opening a log file (e.g. mysql-error.log) that hasn't been written yet now opens an empty file instead of failing with "file not found"; config files still error when missing, since that usually means something's actually wrong
- Apache: enabled mod_access_compat by default, so old .htaccess files using the legacy Apache 2.2 `allow from`/`deny from` syntax (common in older WordPress installs) work instead of throwing a 500. Existing Apache instances get this automatically on next start, no action needed
- UI: redesigned the main window - sites and web-servers/databases panels now resize properly, with clearer visual structure, a live status strip, a redesigned jobs tray, and an improved light theme
- Reliability: significantly refactored how ForgeKit reads and writes its own configuration and binary state internally, closing several rare race conditions where a site, instance, or binary change could be silently lost or overwritten by another action happening around the same time


---

### 1.6.3
2026-07-15


### Improved
- Fixed HTTPS sites showing mixed content errors (http:// stylesheets/scripts blocked on an https:// page). PHP now receives the real request scheme through both Apache and Nginx, so Laravel's asset()/url() helpers and similar framework URL generation work correctly
- Fixed the ForgeKit router silently failing to bind port 80 or 443 if something else briefly held the port at startup. Previously this was only recoverable by toggling LAN on and off; it now retries automatically and keeps retrying in the background until the port is free
- Added a persistent warning in the app showing which process is blocking port 80 or 443, instead of sites just silently failing to load
- Agent activity is now logged to logs/agent/agent.log instead of being discarded


---

### 1.6.2
2026-07-15


### Improved
- Add Web Server modal no longer lists Node.js versions under "Installed Back End Languages". Node is assigned per-site, not per-web-server, and never worked as a backend language selection there
- The php.ini fix from 1.6.1 now also runs when a new web server instance is created, so a fresh Nginx server gets a working php.ini immediately instead of on its first start

---

### 1.6.1
2026-07-15


### Improved
- fixed onboarding for new users where nginx would not work. Just restart nginx and it will fix itself.
- fix flashing cmd windows on startup when checking npm versions in installed nodejs for the new 1.6 functionality
- make the installer shut off fkit.exe cli tool if it's already running in the background, so it doesn't interfere with the update

---

### 1.6.0
2026-07-15

### New Features
- Added Node.js / npm / npx / Corepack / Yarn / pnpm support, selectable per site
- New `fkit node`, `fkit npm`, `fkit npx`, `fkit corepack`, `fkit yarn`, `fkit pnpm` CLI commands
- Node version picker added to Add Site / Edit Site modals 
### Improved
- Binaries modal: categories are now collapsible, with nested/indented binary lists and the npm version shown next to Node versions
- Custom Binaries section now grouped by kind (PHP, Node, etc.), same as the main list
- Add Site / Edit Site modals redesigned into a two-column layout
- Better error message when trying to remove a Node version that's still assigned to a site
- Fixed an edge case where switching between two binaries with the same version (e.g. official vs custom) could show the wrong one as current
- Fixed a bug affecting fresh Nginx-based installs where the first PHP version installed could end up missing php.ini, breaking extensions like mbstring

---

### 1.5.1
2026-07-12

### Improved
- LAN vhosts/server blocks are now consolidated per site (using ServerAlias for Apache, a combined server_name for Nginx) instead of generating a separate duplicate block per hostname.
- Fixed LAN links being silently upgraded to HTTPS by the browser, which broke access since other LAN devices don't trust ForgeKit's local certificate authority.
- LAN access is now a real network-level boundary. Previously, turning LAN off only hid the convenience links, but the router still listened on all interfaces and would proxy through to any site whose domain was sent as the Host header. It now binds loopback-only when LAN is off, so the machine is genuinely unreachable from the network.
- The custom localhost app now also loads when visiting the machine's LAN IP directly, matching its existing behavior on localhost. Previously it always fell back to the default ForgeKit landing page in that case.

---

### 1.5.0
2026-07-09

### New Features
- Lan functionality added. It can be turned on or off from preferences and allows access to local IP address and sites via custom sslip.io urls per site.
- Preferred apps section added in Preferences tab, with preferred terminal, logs editor and code editor.
- Open site in terminal and open site in code editor buttons added.
- Added Sites section in the tray functionality. 
- Custom binaries in Manage Binaries modal. Add your own binaries in custom-binaries.json file for ease and convenience.

### Improved
- fkit.exe terminal shim improvement for when there are 2 environments running the same project root. It now allows you to pick which one to use.
- Installer no longer feels unresponsive. It might also be faster in cases.
- Various UI improvements.

---

### 1.4.1
2026-07-03

### Improved
- added support for older bootstrapping of mysql 5.6 and older
- small improvements to UI

---

### 1.4.0
2026-06-28

### New Features
- added system tray functionality
- on startup preferences options for start on windows start and restart previous servers
- added site specific access and error logs

### Improved
- changed fk-local to localhost-fk in terms fo the custom localhost app redirection
- improvements to port checking
- various improvements to UI


---

### 1.3.1
2026-05-15

### Improved
- fix redirection bug for custom-localhost-app
- fix bug that can leave orphaned apache processes on close instead of fully closing them too
- small UI bug that showed up on preferences data update that might affect the update process

### If you cannot automatically update from the ForgeKit update tab, close ForgeKit fully and manually run the installer ForgeKit-Installer-1.3.1.exe from your ForgeKit installation folder /downloads. Run it on top of your current ForgeKit installation folder and it should all be gucci.


---

### 1.3.0
2026-05-14

### Improved
- default localhost page

### New Features
- added the allow directory listing checkbox to sites.This allows folder browsing in the web",
- added preferences page and moved the theme there",
- added custom localhost page configuration to preferences which allows user to fully customize the localhost page"

---

### 1.2.2
2026-05-07

### Improved
- fix onboarding modal running again on startup when it was not needed to, which created default web server

---

### 1.2.1
2026-05-07

### Improved
- allow httpd folders to work not just apache, to better work with newer apache versions
- make labels text colour black if background is lighter
- more space and improvement of the sites sidebar
- added on hover action for the sites in sidebar along with open in browser, open in folder and edit buttons
- improvements to local binary detection constraints and possible dupplicate IDs
- added link to updated binary docs in Binary Management Window
- updated binary labels for installed vs user installed and added them in more places

---

### 1.2.0
2026-04-24

### New Features
- Added per-site logs in /logs/sites
- Added fallback php logs in /logs/php-fallback

### Improved
- improved PhpMyAdmin configurations and reliability
- better PhpMyAdmin logging
- min width and height to the UI window
- fix dropdown menus being hidden by overflow
- remove legacy mysql error log
- fix duplicate mysqli php extension for future and existing php.ini files

---

### 1.1.2
2026-04-01

### Improved
- update apache mod_php with local env setup to improve php extension compatibility
- rearchitected phpMyAdmin to use it's own independent php runtime

---

### 1.1.1
2026-03-12

### New Features
- Added links to docs and other locations in main page app

### Improved
- Fixed link to changelog in updates modal

---

### 1.1.0
2026-03-03

### New Features
- SSL functionality per site.

---

### 1.0.17 - 1.0.18
2026-02-13 to 2026-03-03

### New Features
- Added support for full site config paths, allowing sites to point to any path on the machine.
- Added **Open in browser** and **Open in explorer** actions for sites.
- Added a localhost router dashboard landing page.

### Improved
- Improved scrolling in the sites and main sections to better handle larger numbers of items.
- Improved server action layout by moving remove buttons to the right side of web and database server entries.
- Updated binaries for PHP 8.3 Thread Safe builds.
- Applied UX fixes and general polish.
- Auto-enabled `mod_rewrite` for Apache.

---

### 1.0.16 
2026-02-10

### New Features
- Updated application icons.

---

### 1.0.9 - 1.0.14 
2026-02-02

### Improved
- Improved hosts file editing.
- Improved elevation flow and UAC handling.
- Added creation of a scheduled task to support future hosts file editing without repeated friction.

---

### 1.0.8
2026-01-04

### First public release

### Improved
- Improved the update procedure.
- Applied UI fixes.

---
