# Export and import update sets

Goal: keep the configuration built in the PDI as files in the repo, so it can be recreated if the instance is reclaimed.

## Export

1. Open the update set (Local Update Sets) and check its contents in Customer Updates. Group by Type; do not rely on name prefixes, some entries have an empty target name.
2. Sort by Created (newest first) and remove entries that are not from the lab (for example system records captured by side effect, such as an ECC queue authorization policy after creating a table, or the form layout of a log table after opening it). Removing an entry from the set does not undo anything in the instance.
3. Set State to Complete and click Update. The `Export Update Set` related link only appears in this state.
4. Click `Export Update Set` and save the XML in `update-sets/` with a date in the name.
5. Set State back to In progress and select the set again as the current update set (globe icon, top right).
6. Scan the XML before committing it: no passwords, secrets, tokens, API keys, private keys, and no instance name. Example: `grep -ciE 'password|secret|token|api_key|bearer|private_key|credential|<instance-name>' file.xml` must print 0.
7. Commit the XML.

## Import

1. Retrieved Update Sets > Import Update Set from XML > choose the file > Upload. Upload it once; a second upload creates a duplicate retrieved set (delete the older one).
2. Open the retrieved set and click Preview Update Set. Wait until State is Previewed.
3. Read the counters and the preview problems before committing: Inserted, Updated, Deleted, Collisions, Total. Total must match the exported entry count.
4. Click Commit Update Set. Open the commit log and read every Error line.
5. Check the result in the instance (open the main objects).

## Notes

* Committing a retrieved set creates a local update set with the same name in state Complete. Keep working and exporting from the one In progress.

## Known limits

* Data (users, groups, incidents and other records) is not captured; recreate it by hand. References from captured configuration to missing records (for example a user criteria pointing to a group) need the record to exist first.
* Importing into the same instance only proves the file is valid; a real rebuild needs a fresh instance.
