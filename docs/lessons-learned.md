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

## Day 3 - Tables and fields

* Created the update set `LAB SN - Build` and made it current before building, so configuration changes are captured for the update set export planned for day 10. Data records (users, incidents, table rows) are not captured by update sets.
* Added a String field `LAB D3 Note` (`u_lab_d3_note`) to the incident form. It saves text. The update set captured two entries for it: Field Label and Dictionary.
* Created a custom table `LAB D3 Equipment` (`u_lab_d3_equipment`) and added 3 records: `LAB D3 - laptop 1`, `LAB D3 - laptop 2` and `LAB D3 - Monitor 1`. The list is labeled "LAB D3 Equipments" because the platform adds a plural "s" to the label.
* A demo incident (`INC0009009`) still loads and shows the new field, empty.
* Form Layout did not respond at first and worked later. Form Builder and Form Design are also available under Configure on the incident form.
* New columns and tables get the `u_` prefix automatically.
* The table has three columns: `Name`, `Serial Number` and `Purchase date`. The update set `LAB SN - Build` holds 13 entries for the day 3 work (filter: target name contains `LAB D3`): the incident field (Field Label, Dictionary), the table (Table, Dictionary, Form Layout, Field Labels), the application menu and the modules.

## Day 4 - Incident management end to end

* Ran one incident (`INC0010004`, "LAB D4 - Printer not working") through the full lifecycle: New, In Progress, Resolved, Closed. States were never skipped.
* Priority is calculated from Impact and Urgency (Impact 2 - Medium gave Priority 3 - Moderate).
* Assigning the incident to `Lab Agent` moved the state from New to In Progress automatically.
* Setting the state to Resolved makes Resolution code and Resolution notes mandatory, so an incident cannot be resolved without them. Closing follows from Resolved.
* Once an incident is Closed, the record becomes read-only.
* The activity stream records every field change with its time and the user. It shows one work note (`Checking the printer`); the resolution reason is in the Resolution notes. Closing has no note of its own. Next time, add a work note at each state change so every transition has its reason.
* The incident was assigned to the group `LAB D2 - Service Desk` and to the agent `Lab Agent`, which reuses the users and the group built on day 2.

## Day 5 - Catalog item with variables and approval

* Built the catalog item `LAB D5 - Request a monitor` (variables `Monitor size` and `Justification`) and the flow `LAB D5 - Monitor approval`. The item is linked to the flow in the item's Process Engine > Flow field. The Service Catalog trigger has no item field, so the link is not made in the flow.
* Approve path: the request reaches the approver (My Approvals / Approvers tab on the RITM). Approving moved RITM0010002 and RITM0010005 to Work in Progress.
* Reject path: with only an `Approve when Anyone approves` rule, a rejection by the sole approver ended with Approval State `cancelled`, not `rejected`. Both Ifs evaluated False and the RITM stayed Open (RITM0010003, RITM0010006). Adding a second rule set (`Reject when Anyone rejects`) fixed it: RITM0010007 moved to Closed Incomplete by the system seconds after the rejection.
* Nothing reached fulfillment without an approval: every RITM stayed Open until the approval was decided.
* The RITM Stage field showed Request Approved before any approval, so it is not a reliable indicator here. Use State and the approval record.
* Flow Context / Execution Details (inputs, outputs, Not Run steps) is the fastest way to debug a flow. Flow Reporting at Full is for debugging only.
* A flow that is made Inactive needs the Edit flow button before Save and Activate appear.
* `Monitor size` only showed the 27" choice; the second choice is missing in Question Choices (to fix).
* Next time: change one thing in the flow at a time and confirm it before testing again. Test both the approve and the reject path from the start.
