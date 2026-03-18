# Cybertt

Facilitator tool for running cybersecurity incident response tabletop exercises. Single HTML file — no build step, no server, no dependencies to install.

## Usage

Open `index.html` in any modern browser.

## Features

- **Scenario library** with import/export (`.json`)
- **Scenario builder** — injects, roles, dice outcome tables
- **Live session view** — tension meter, inject queue, dice roller, decision log, live event feed
- **Scenario time** — 5 real minutes = 1 scenario hour, clock starts at the incident time in the brief
- **Debrief** — full timeline, decision log, text export

## Built-in scenarios

| Scenario | Language | Injects |
|---|---|---|
| Ransomware Outbreak | English | 6 |
| Phishing-Led Breach | English | 5 |

## Data

Everything is stored in `localStorage`. Nothing leaves the browser. To move scenarios between machines, use the export/import buttons in the library.

See `GUIDE.md` for full facilitator documentation.
