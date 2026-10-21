# Decisions

| # | Date | Decision | Reason |
|---|---|---|---|
| D1 | 2026-10-21 | Use a free PDI instead of a course lab instance | Free, admin access, and it stays available as long as it is used; course lab instances are short-lived |
| D2 | 2026-10-21 | Pick the latest release (Brazil) | Matches current documentation; differences from older releases are small for the topics in this lab |
| D3 | 2026-10-21 | Onboarding answer: guided experience (not IDE) | The lab is administration and ITSM, not code |
| D4 | 2026-10-21 | Track the lab in Jira LNX (epic LNX-61) with label `servicenow` instead of a new project | Reuses the existing board, workflows and automations; a dedicated sprint is built from the label |
| D5 | 2026-10-21 | Build by hand; export update sets as the "rebuildable from code" equivalent | The goal is to learn the platform; update sets allow recreating the work if the PDI is reclaimed |
