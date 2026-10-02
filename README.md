# WTF Dictionaries

Word lists for [WTF — Whiskey Tango Foxtrot](https://timo.is/wtf), a spelling app for Mac and
iPhone.

The app ships its dictionaries inside the app bundle. It also checks this repository, at most
once a day, for word lists that have been **added or corrected** since that release — so a typo
fix reaches people without an App Store submission, and a new dictionary doesn't have to wait
for one.

**This is data, not code.** Nothing here is executable, and the app downloads nothing else.

## What's served

```
v1/manifest.json           the catalog: what exists, at what version, with what checksum
v1/dictionaries/<id>.json  one dictionary: an id, a display name, and one word per letter
```

Served over HTTPS by GitHub Pages at `https://newforms.github.io/wtf-dictionaries/`.

The `v1/` prefix is part of the contract with shipped apps. If the format ever has to change
incompatibly, `v1/` is frozen exactly as it is and new builds read `v2/` — so a copy of the app
that is never updated keeps working.

## The manifest

```json
{
  "schema": 1,
  "dictionaries": [
    {
      "id": "nato",
      "version": 1,
      "url": "https://newforms.github.io/wtf-dictionaries/v1/dictionaries/nato.json",
      "sha256": "095be…",
      "pro": false,
      "about": "The international standard, used by pilots, sailors and call centers…",
      "credit": "Remi"
    }
  ],
  "removed": []
}
```

- **`id`** — lowercase letters, digits and dashes. It is also the filename, so nothing else is
  accepted.
- **`version`** — a whole number that only ever goes up. The app compares it against the copy in
  its bundle and keeps whichever is higher, so a download never undoes a fix that shipped in a
  release.
- **`sha256`** — of the served file's bytes. A download that doesn't match is discarded.
- **`pro`** — whether the dictionary needs WTF Pro. Read from here and never from the dictionary
  file, so a file from somewhere else can't declare itself free.
- **`about`**, **`credit`** — what the app shows on its About page.
- **`removed`** — ids to delete from devices that already have them. Dropping an entry from
  `dictionaries` only stops it being offered; taking something down is deliberate.

## Publishing a change

1. Edit the words in the app repository, where the tests live.
2. Bump `version` in that file **and** in the manifest entry, to the same number. Changing words
   without bumping means nobody gets the change.
3. Run `scripts/publish-dictionaries.py` from the app repository, which copies the files here and
   rewrites the manifest's checksums.
4. Read the diff, then commit and push. The push is the release.

## Scope

Fixes, and dictionaries in the same register as the ones the app already ships — all within its
App Store age rating. This is a maintenance channel, not a content platform.
