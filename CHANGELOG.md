# Changelog

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
