# Pilcrow roadmap rounds

Pilcrow reads these files to show the features people can vote on. Nothing here is private: the app fetches them over
HTTPS from `https://raw.githubusercontent.com/PluriaLabs/Support/main/Pilcrow/Roadmaps/`.

## How it works

- `index.json` lists the rounds and the days each one runs. It is the only file the app always reads.
- Each round is one file named for its days, such as `20270101-20270331.json`, holding every item in every language.
- The app picks the round whose days include today. If none does, it shows the latest finished round read-only, and
  says when the next one opens. If none exists yet, it shows when the first one opens.

## index.json

```json
{
  "rounds": [
    { "start": "2027-01-01", "end": "2027-03-31", "file": "20270101-20270331.json" }
  ]
}
```

- Days are `YYYY-MM-DD` and count in **UTC**. A round is open through the whole of its `end` day.
- `file` is a plain file name in this folder: letters, digits, `-`, `_` and `.`, ending in `.json`. No folders, no
  `..`, no leading dot. An entry that breaks this, or whose `start` is after its `end`, is ignored.
- If two rounds overlap, the one that started later wins.
- A round can name its own app icon with `"icon": "frost"` (a lower-case id such as `frost` or `gilt-foil`). Subscribers who
  hold a paid month during the round earn that icon for good. The icon must already be in the app: a new one needs an app
  update, so add it to Pilcrow a release ahead. An app too old to contain it says so; a bad id is ignored.
- Add a round here **before** it begins if you want people to see "the next round opens on …".

## A round file

See `_example.json` (not listed in the index, so the app never reads it). Copy it, rename it for its days, and add a
line to `index.json`.

- `start` and `end` give the round its name (`20270101-20270331`), which is also what votes are counted under. You may
  instead give an explicit `cycle`, but then keep it unique forever.
- Each item has an `id` that is **never reused**, a `status` (`open`, `building` or `shipped`), a `title` and an
  optional `summary`. Only `open` items take votes, and only while the round is running.
- `title` and `summary` are either one plain string or an object with one entry per language, using the app's language
  codes: `en es fr de it pt-BR nl ru ja ko zh-Hans ar da hi id nb pl sv th tr uk vi zh-Hant`. Pilcrow shows the
  person's language, then English, so a missing translation is never blank.
- An item with no id, no usable title or an unknown status is skipped, and so is a repeated id. One slip does not blank
  the round.
- Keep each file under 256 KB.

## Changing a published round

Edits show up a few minutes after they are pushed (GitHub caches raw files briefly). Do not change an item's `id` once
people have voted on it: their votes stay with the old one.
