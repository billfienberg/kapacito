# Kapacito

A static, single-page daily/weekly dashboard for an always-on TV (FireTV +
Fully Kiosk Browser). No build step, no dependencies, no network calls — open
`index.html` and it renders.

## How it works

- Everything is computed client-side from the inline JSON in
  `index.html` (`<script id="kapacito-data">`): `tasks`, `recurring`,
  `milestones`.
- "Today" is read from the browser clock on every tick, so the page can run
  unattended for weeks and roll over at midnight on its own. No dates are
  hardcoded.
- The layout is authored at exactly 1920x1080 and CSS-scaled to fit whatever
  the display reports, so proportions hold on any screen.
- It re-renders on the minute (clock, now-line, task ages, day rollover). The
  milestone marquee is only rebuilt on a day rollover so the loop never jumps.

## Updating the data

Edit the JSON block in `index.html` and push. The fields it reads:

| Collection   | Fields                                                                          |
| ------------ | ------------------------------------------------------------------------------- |
| `tasks`      | `title`, `tag`, `date` (`YYYY-MM-DD` or `null`), `time`, `duration`, `dueDate`, `createdAt`, `completed` |
| `recurring`  | `title`, `tag`, `time`, `duration`, `days`                                       |
| `milestones` | `title`, `date`                                                                  |

Reading is deliberately forgiving: `time` accepts `"09:30"`, `"9:30am"` or
`"7 PM"`; `duration` can be replaced by `endTime`; `days` accepts `"daily"`,
`"weekdays"`, `"weekends"`, `["mon","tue"]`, `[1,2]` or a single day.

Tag colors: work-ish tags (work, podcast, dogs/walks, morning routine) get a
steel-blue border, anything else gets terracotta, `sleep` stays neutral. The
lists are `WORK_TAGS` / `NEUTRAL_TAGS` near the top of the script.
