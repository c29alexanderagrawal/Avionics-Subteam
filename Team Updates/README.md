# Team Updates

One folder per avionics team member. Each folder has a `Daily Updates.md` file where that person logs what they did.

| Member | Subsystem | Update file |
|---|---|---|
| Alex | Primary flight computer | [Alex/Daily Updates.md](<Alex/Daily Updates.md>) |
| Zach | Lessons | [Zach/Daily Updates.md](<Zach/Daily Updates.md>) |
| Lucie | Radio | [Lucie/Daily Updates.md](<Lucie/Daily Updates.md>) |
| Max | Sensors | [Max/Daily Updates.md](<Max/Daily Updates.md>) |
| Peter | Pyrotechnics | [Peter/Daily Updates.md](<Peter/Daily Updates.md>) |
| Caleb | Not assigned yet | [Caleb/Daily Updates.md](<Caleb/Daily Updates.md>) |

## How to add an update

1. Open your own `Daily Updates.md`.
2. Copy the template block at the top of the file.
3. Paste it **directly under the "Log" heading**, so the newest entry is always first.
4. Fill it in, then commit with a message like `Update: Alex 2026-11-13`.

Keep the headings and field names exactly as they are in the template (`## YYYY-MM-DD`, `**Worked on:**`, and so on). The mission dashboard reads these files every morning, and it relies on that layout. Leave a field as `-` if there is nothing to say.

## Fields

- **Worked on:** what you spent time on today.
- **Completed:** anything you finished. These roll up into your monthly accomplishments.
- **Next:** what you plan to do next.
- **Blockers:** anything stopping you, or help you need.
- **Helped:** teammates you helped today, comma-separated first names (for example `Zach, Lucie`).
- **Helped by:** teammates who helped you, same format.
- **Collaboration notes:** optional; what you worked on together, or people outside avionics.
- **Parts:** one line per part: `part name | subsystem | qty | status`. Status is one of `needed`, `ordered`, `received`, `installed`.
