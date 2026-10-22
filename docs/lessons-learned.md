# Lessons learned

Add one entry per day. Keep it short: what was done, what was unexpected, what to do differently.

## Day 0 - Developer account, PDI and repo

* The developer site showed a notice that some PDIs were unavailable and PDI actions were paused. The request still went through on 2026-10-21 (note the real outcome here if it changes).
* Only direct logins to the instance count as activity; plan a login at least every 10 days.

## Day 1 - Navigation: lists, filters, forms, favorites

* Saved filter: `LAB D1 - High priority incidents`.
* Created test incident INC0010003. The form requires Short description and Caller.
* Favorite added: Self-Service > Incidents.
* No demo data was modified or deleted.
* The instance felt slow. Worth tracking on later days to see whether it is the first-load time or a constant.
* The web interface felt better than the Windows client used before.
* The All menu has far more applications and modules than expected. The navigator filter and favorites make it manageable; each day only needs a small area of it.

## Day 2 - Users, groups and roles

* Created group `LAB D2 - Service Desk` and two test users: `lab.d2.agent` (member of the group, role `itil`) and `lab.d2.nobody` (no roles, no group).
* The `itil` role contains other roles, so the agent shows 48 roles in total. Only `itil` was assigned by hand; the rest are inherited.
* No role starting with `admin` is assigned to the agent.
* Test with Impersonate User: the agent sees the incident list (41 active incidents); `lab.d2.nobody` sees an empty list ("No records to display"), with no access-denied message.
* Identity type matters: the users were first created with Identity type set to AI, which cannot be edited afterwards and kept them out of the impersonation search. They had to be deleted and recreated as Human. Set Identity type to Human when creating people.
* Test user emails use `@example.com` so that notifications on day 7 cannot reach real mailboxes.
