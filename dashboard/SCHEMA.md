# MCD data schema

Every collection below is snapshotted nightly to `data/<collection>.json`.
Crew ids: `alex`, `zach`, `lucie`, `max`, `peter`, `caleb`.

## `crew/<id>` — member pages (editors only; written by the nightly sync)
```
name, role                     // set by team lead; sync never overwrites
focus                          // "Working on" paragraph, synopsis sentence first
latest: { date, workedOn, next, blockers, helped:[ids], helpedBy:[ids], collabNote }
workingOn: []                  // unused
accomplishments: [ { month: "YYYY-MM", text } ]           // newest first
teamwork: { helped:[{member,count}], helpedBy:[{member,count}], summary,
            external:[{name, subteam, count}] }
photos: [ { asset, date, caption, path, addedAt } ]        // newest first
parts: []                      // not used
recentUpdates: [ { date, workedOn } ]                      // ~8, newest first
updatedAt, consolidatedAt
```

## `updates/<id>` — daily-update queue (written by the "+ Daily update" form)
```
member, date, workedOn, completed, next, blockers,
helped:[ids], helpedBy:[ids], collabNote,
parts: [ { part, subsystem, qty, status } ],              // needed|ordered|received|installed
photos: [ { asset, path: "photos/YYYY-MM-DD-<10 chars of asset id>.jpg" } ],
submittedAt,
status: "pending" → "committed" → "consolidated",
committedAt, commit, consolidatedAt, photosPending
```

## `taskers/<id>`
```
title, detail, assignees:[ids], suspense: "YYYY-MM-DD" | "", taskedBy,
priority: routine|priority|urgent, link,
done_<id>: ISO time | ""       // one flat field per member
createdAt, updatedAt
```

## `calendar/<id>`
```
title, kind: meeting|review|test|deadline|social|other,
date, allDay, start: "HH:MM" | "", end, location,
repeat: "" | weekly | biweekly, until: "YYYY-MM-DD" | "",
attendees:[ids], link, detail, createdAt, updatedAt
```

## `events/<id>` — timeline milestones
```
title, date: "YYYY-MM-DD", time: "HH:MM", detail, final: bool    // final = launch
```

## `agendas/<YYYY-MM-DD>` — Avionics sync agendas
```
title, time, location,
items: [ { title, note, done, subsystems?: [ { name, owner } ] } ]   // delegation board
```

## `photos/<id>` — direct gallery uploads
```
asset, caption, uploadedAt
```

## `meta/sync`
```
lastConsolidatedCommit, lastRunAt
```

Access: `crew` is editors/owner only; contributors can write `updates`, `taskers` and `calendar`.
The full pipeline description lives in the Blue Horizon Rocketry project doc
"Avionics Mission Control Dashboard".
