# Changelog


### 1.8.1 Zoom, a resizable layout, and new privacy controls
2026-09-07

### New Features
- Zoom: the app can now be zoomed with Ctrl and +/- (or Ctrl and scroll), with a zoom control in the title bar and in the Tools menu. Your zoom level is remembered.
- Sites: the sites sidebar can be dragged wider or narrower, and double-clicked to reset.
- Privacy: new Privacy section in Preferences. ForgeKit now sends an anonymous snapshot of how it's set up — version numbers and counts, never project names, domains, paths, code or database contents. It's on by default, "View analytics data" shows you the exact payload, and one checkbox turns it off entirely.

### Improved
- enable OPcache by default for new PHP versions. It's free performance.
- phpMyAdmin: fixed "cannot prepare phpMyAdmin: ... Access is denied" on start/open for users whose config directory blocks file replacement (e.g. certain network drive permissions) - now falls back to a direct write if the atomic rename fails.
- Quitting: closing ForgeKit now shuts down phpMyAdmin and cleans up its background processes properly. Previously they could be left running after the app had closed.
- Databases: new "Connection info" button next to phpMyAdmin, with copyable Laravel .env and WordPress wp-config.php blocks for that server.
- Agent status: the status dot no longer pulses to save cpu and battery while in background
- Status bar: Improved UI
- Messages: "no code editor selected" and "could not open a terminal" errors now carry an "Open Preferences" button that jumps straight to Preferred Apps.
- Window: the app can now be resized much smaller, both narrower and shorter, and the layout adapts instead of clipping.
- Footer: improved footer UI with new Contact and Support us links.
- Downloads: moved next to the other footer buttons, and stays usable while a modal is open.
- Aura Farming Box: opens in its own window now, so it shows a lot more and doesn't take up space in the main app.
- Preferences: the section list on the left now highlights and jumps to every section correctly on a tall window.
- New Contact and Support us links in the app
- General UI improvements throughout the app.

---


### 1.8.0 | Mailpit, Redis, Memcached and a lot of UI improvements
2026-07-25

### New Features
- Mail Servers: added support for Mailpit, a local SMTP catcher. Create one or more named inboxes and connect a web server's PHP mail() to any of them with a click. Nothing connects automatically.
- Mail Servers: each inbox shows ready-to-copy SMTP connection info (.env-style, with Laravel and Symfony examples) for apps that configure their own mail client.
- Mail Servers: a web server can be moved between inboxes at any time, and inboxes respect the LAN preference the same way web servers do.
- Cache Servers: new "Redis / Cache Servers" tab. Add, start/stop/restart, and remove Redis and Memcached instances the same way as web servers, databases, and mail servers.
- Cache Servers: each instance shows ready-to-copy connection info (.env-style, with a Laravel example) for both Redis and Memcached.
- Cache Servers: Redis gets its own redis.conf per instance, created once and never overwritten - open and edit it directly from the instance's Config menu (e.g. to turn persistence back on).
- Cache Servers: Redis versions offered are 7.0.15 and 8.8.1 (via redis-windows/redis-windows); Memcached is 1.6.8 (via jefyt/memcached-windows). Installed and managed through the existing Manage Binaries flow.

### Improved
- Manage Binaries: fixed the list flickering/jumping while an install or removal is in progress.
- Error messages and toast notifications: text is now selectable, and a copy button was added, so you can grab the full message instead of only what's visible.
- Agent startup: fixed a race where forgekit-agent.exe would show a "port 80/443 already in use" warning for itself on launch.
- Agent widget: "Could not refresh status" and "Agent not running" no longer flash on normal startup or after an update. The widget shows a "Starting" state while the agent comes up instead of reporting an error.
- Agent widget: added a spinner for the "Starting" state. The port-in-use warning also now says when it's just a previous ForgeKit instance still shutting down (auto-resolves) versus another app you need to close yourself.
- Router landing page: added a search box above the site list once you have more than a handful of sites, and the list scrolls on its own instead of growing the whole page.
- Router landing page: added a Mail Servers section listing every Mailpit inbox with an open-inbox link (and a LAN link, when LAN access is on).
- Router: https://127.0.0.1/ no longer fails with a TLS error. The auto-issued localhost certificate now includes IP address SANs, and existing installs fix themselves automatically on next agent start.
- Router landing page: the site list no longer scrolls horizontally on narrow windows. Long folder paths, URLs, and domains wrap instead of overflowing.
- phpMyAdmin: fixed opening phpMyAdmin failing on a fresh install or new machine. First-time startup can take longer than the old 15s allowed (antivirus scanning php.exe for the first time is a common cause), so the timeout was raised and the modal now offers a "Try again" button instead of a dead end.
- PHP: gd is now enabled by default for new sites and for phpMyAdmin's runtime. It's only turned on when the matching DLL is actually present, so it can't break an install that doesn't ship it, and it works correctly on PHP 7.4 and earlier where the extension is named gd2.
- PHP: raised default limits for new sites so heavier scripts (image processing, API calls, larger imports/uploads) don't fail out of the box. memory_limit 128M to 512M, max_execution_time 30s to 300s, and date.timezone now defaults to UTC instead of being unset. Applied to both Apache and nginx sites; nginx sites previously got none of ForgeKit's PHP defaults.
- nginx: added client_max_body_size (50M) and fastcgi_read_timeout/fastcgi_send_timeout (300s) to site configs. nginx's own 1M upload cap and 60s timeout were overriding the more generous php.ini settings above before PHP ever saw the request.
- PHP: new PHP binaries and phpMyAdmin's runtime now get a CA certificate bundle out of the box, fixing "SSL certificate problem: unable to get local issuer certificate" on any outbound HTTPS call from PHP's cURL (Laravel's HTTP client, Guzzle, etc.). PHP's Windows builds don't ship one of their own. The bundle is refreshed automatically every time a new ForgeKit release is built, so it stays current without any runtime network dependency.
- Manage Binaries: opening the modal no longer waits on the custom catalog (a user-supplied JSON file or URL) to load - the official/local binaries list now appears immediately, with the Custom Binaries section loading separately and showing its own spinner. Switching between custom-binaries.json and a catalog URL also only refreshes that section instead of reloading the whole modal.
- fkit CLI: `fkit npm`/`node`/`npx`/`corepack`/`yarn`/`pnpm` now hand child processes a PATH prefixed with the site's own Node folder. Previously, `fkit npm install` itself always worked, but on a machine with no globally installed Node, any lifecycle/postinstall script that shelled out to `node`/`npm` by name could fail to find them (npm's own PATH-prepending safety net is off by default). This is scoped to the one process fkit spawns per invocation and never touches the parent terminal's real PATH.
- Preferred Apps: terminal picker now also detects Windows PowerShell, PowerShell 7, Command Prompt, WezTerm, Alacritty, and ConEmu/Cmder, not just Windows Terminal and Git Bash.
- Preferred Apps: code editor picker now also detects PhpStorm, WebStorm, Zed, GVim, and Neovim Qt. The logs/config editor picker can use any detected code editor too, not just Notepad++/Notepad.
- Preferred Apps: a manually picked .exe is recognized by filename too, so it still gets full support instead of a bare launch.
- Preferred Apps: WezTerm, Alacritty, and ConEmu now open with the user's own configured shell instead of always forcing PowerShell.
- Preferred Apps: fixed Windows PowerShell and Command Prompt opening a window that flashed shut immediately instead of staying open.
- Preferred Apps: fixed WezTerm not getting the site's PHP/Node on PATH at all.
- Preferred Apps: known limitation, not fixed - a shell profile that already defines its own php/node command (Laravel Herd does this) can still override ForgeKit's PATH in WezTerm.
- Preferred Apps: reverted registry/Scoop/Chocolatey/WinGet app detection and Alacritty/ConEmu's auto PHP/Node PATH, added earlier this cycle. Likely why Windows Defender started flagging ForgeKit as a false positive. Detection still works via PATH and each app's install folder.
- Add Instance: when a database, mail server, cache server, or web server has no matching binary installed yet, the modal now tries to send you straight to Manage Binaries instead of showing a form you can't submit. If an install is already running, it shows up inline as "Installing…" and becomes selectable the moment it finishes.
- Add Instance / Edit Label: UI Improvements by replacing the separate Preview/Name/Color fields with one row, an editable color pill for the name plus quick-pick swatches (blue, yellow, green, red, or a custom color).
- Switch PHP Version: a PHP version currently installing now shows up as "Installing…" instead of just not being there, and becomes selectable the moment it finishes.
- Node version dropdown (Add Site / Edit Site): same fix as above - a Node version currently installing shows inline and updates live once it's done.
- Manage Binaries: fixed the Custom Binaries section's loading spinner flickering on and off every second while any install was running anywhere in the app, not just in that section.
- Manage Binaries: title, back, and close buttons now stay pinned in place while the list scrolls instead of scrolling out of view; added a matching footer with the same actions.
- Add Site: typed-in fields (domain, folder, web server, Node version, HTTPS options) now survive a trip to Manage Binaries and back instead of resetting to blank.
- Add Site: auto-selects the web server when there's only one, instead of defaulting to "none" and confusing first-time users.


---

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
