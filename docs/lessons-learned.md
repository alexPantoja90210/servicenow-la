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

## Day 6 - Change, problem and knowledge base

* Created problem `PRB0040001` (`LAB D6 - Printer queue keeps stalling`) and normal change `CHG0030001` (`LAB D6 - Restart print spooler service`). The change is linked to the problem in the change's `Problems (1)` tab.
* Created knowledge article `KB0010001` (`LAB D6 - How to restart the print spooler`) in the `IT` knowledge base. It started as a draft (v0.01).
* Publish does not publish directly: the `IT` base uses the publish flow `Knowledge - Approval Publish`, so the article went to In review and needed an approval. The base owner is a demo-data user, not the admin; the admin approved it manually. After approval it became v1.0, Published, valid to 2100-01-01.
* Draft visibility: `lab.d2.nobody` did not find the draft, but that proves little, because it also did not find the published article.
* Why: the `IT` base `Can Read` list had only demo-data user criteria (a problem-solving group, all ACME North America employees, all ACME Corporation employees). Neither lab user matched any of them, so neither could read anything in the base. Admin could see the draft in search.
* Fix: created the user criteria `LAB D6 - Service Desk readers` (group `LAB D2 - Service Desk`) and added it to the `IT` base `Can Read` (now 4 entries). Then `lab.d2.agent` found `KB0010001` (published) and got 0 results for `KB0010002` (left as a draft). That is the real proof that a draft is hidden from a user who can read the base.
* Navigation: opening an article from the search shows the Knowledge Portal view, which has no edit buttons; use the Knowledge application list to get the form. The `Knowledge Bases` entry under `Now Mobile App` is a different thing. From the article form, the (i) icon next to `Knowledge base` leads to the base record and its `Can Read` / `Can Contribute` tabs.
* The instance is slow and gives no visual feedback on clicks; one click and wait.
* Cleanup list for day 12: `PRB0040001`, `CHG0030001`, `KB0010001`, `KB0010002`, user criteria `LAB D6 - Service Desk readers` and its entry in the `IT` base `Can Read`.
* Next time: before testing visibility with a user, check that the user can read the base at all; otherwise a "not visible" result proves nothing.

## Day 7 - Notifications and events

* Created the notification `LAB D7 - Incident assigned` on the incident table: Send when `Record inserted or updated` with Inserted and Updated both checked. Recipients: `Assigned to` and `Caller` (Users/Groups in fields). Subject `LAB D7 - Incident assigned: ${number}`.
* The visual Conditions builder has no `changes` operator for `Assigned to` (the `is` operator expects a person). Firing only on assignment needs the Advanced condition script, shown after saving through the `Advanced view` related link: `answer = current.assigned_to.changes() && !current.assigned_to.nil();`. The script must set the `answer` variable; a bare expression does not work.
* Saving with no Inserted or Updated checkbox is refused ("Select a value for Insert or Update before submitting").
* The new notification had `System Administrator` pre-filled in the Users field, which would have notified a user who is neither assignee nor caller. Check the Users field and clear it.
* Saving the form twice created a duplicate notification with the same name; only one is needed (the one with both Inserted and Updated checked).
* Test: INC0010005 (caller `lab.d2.nobody`, assigned to `lab.d2.agent`) produced exactly one email in System Logs > Emails, subject `LAB D7 - Incident assigned: INC0010005`, recipients `lab.d2.agent@example.com` and `lab.d2.nobody@example.com`, type `send-ready`, notification type SMTP.
* Negative test: a work note on the same incident without changing `Assigned to` produced no new email.
* PDI limitation: the email is generated and left `send-ready` in the log; it is not delivered to a real mailbox (the addresses are @example.com). Verification is through the email log only.
* Notification Preview (with a demo incident) shows the subject, body and recipients without sending anything; useful to check a template.
* Cleanup list for day 12: notification `LAB D7 - Incident assigned`, incident INC0010005.
* Next time: after creating a record, check the fields that are pre-filled by default, not only the ones you set.

## Day 8 - Flow Designer: flow on incident creation

* Built the flow `LAB D8 - Network incident note`: trigger Created on the incident table with the condition `Category is Network`, and one Update Record action on the trigger record that adds a work note.
* Matching case: INC0010006 (category Network) got the work note about 15 seconds after creation. Execution list: one execution of the flow, state Complete, created by admin, about 6 seconds after the incident was created.
* Non-matching case: INC0010007 (category Software) got no work note and produced no execution (the execution list shows exactly one).
* The `Run Trigger` (once / for each unique change) option does not exist for a Created trigger; it only applies to Updated triggers. A Created trigger runs once per record.
* The work note text `${number}` was written literally into the note (INC0010006): Flow Designer does not use `${...}` in text fields. To insert a field value, drag the data pill from the data panel (Trigger - Record > Incident Record > Number) instead of typing it. After the fix, INC0010008 (category Network) got the work note `LAB D8 - flow ran for INC0010008`.
* Evidence of a flow run: the work note on the record (activity stream) and the execution record in Operations > Executions.
* Cleanup list for day 12: flow `LAB D8 - Network incident note`, incidents INC0010006, INC0010007 and INC0010008.
* Next time: test the non-matching case in the same session as the matching one; it proves the condition, not only that the flow works.
