# servicenow-lab

Hands-on ServiceNow practice on a free Personal Developer Instance (PDI). Everything is built by hand in the instance, one topic per day, and documented here so the work can be reviewed and rebuilt.

This lab is part of a set: [linux-admin-lab](https://github.com/alexPantoja90210/linux-admin-lab) and [cloud-lab](https://github.com/alexPantoja90210/cloud-lab).

## Goal

Learn the ServiceNow platform from an ITSM / project management point of view: access control, incident and change processes, service catalog, automation, reporting, and how to move work between instances with update sets.

## Environment

| Item | Value |
|---|---|
| Instance type | Personal Developer Instance (PDI), free |
| Release | Brazil |
| Created | 2026-10-21 |
| Cost | $0 |
| Tracking | Jira project LNX, epic LNX-61 "ServiceNow Lab", label `servicenow` |

The instance URL and the admin password are **not** stored in this repo.

## Rules

* Log in directly to the instance URL at least every 10 days. Visiting the developer site does not count, and an inactive PDI can be reclaimed and reset.
* Export update sets to this repo before any long pause.
* Never commit credentials, instance passwords or tokens.
* Do not use the PDI for business or production purposes (Developer Program terms).

## Plan (days 0-12)

| Day | Topic | Jira |
|---|---|---|
| 0 | Developer account, PDI and repo | LNX-62 |
| 1 | Navigation: lists, filters, forms, favorites | LNX-63 |
| 2 | Users, groups and roles | LNX-64 |
| 3 | Tables and fields | LNX-65 |
| 4 | Incident management end to end | LNX-66 |
| 5 | Service catalog item with approval | LNX-67 |
| 6 | Change, problem and knowledge base | LNX-68 |
| 7 | Notifications and events | LNX-69 |
| 8 | Flow Designer | LNX-70 |
| 9 | Reports and dashboard | LNX-71 |
| 10 | Update sets: capture, export, import | LNX-72 |
| 11 | CMDB: register the RHEL lab VMs | LNX-73 |
| 12 | Wrap-up: inventory, lessons, CSA decision | LNX-74 |

## Documentation

* [docs/setup.md](docs/setup.md) - how the instance was created and how to access it
* [docs/decisions.md](docs/decisions.md) - decisions and the reason behind each
* [docs/lessons-learned.md](docs/lessons-learned.md) - what worked, what did not
* [docs/sprints.md](docs/sprints.md) - sprint log
* [docs/runbooks/](docs/runbooks/) - repeatable procedures
* `update-sets/` - exported update sets (XML), added from day 10 on
