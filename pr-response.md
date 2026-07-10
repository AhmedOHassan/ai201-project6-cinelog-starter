# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename
**What I did:** Renamed `save_to_watchlist()` to `add_to_watchlist()` in `services/watchlist_service.py` so it matches the `verb_to_noun` convention already used by `add_to_collection()` in `services/collection_service.py`. Updated the docstring's first line to say "Add a film" instead of "Save a film" so it stays consistent with the new name. Updated the one call site in `routes/watchlist/watchlist.py`, both the import and the function call.
**How I verified:** Searched the whole project for `save_to_watchlist` after the rename and got zero matches, so I know every call site was caught. Ran `pytest tests/ -v` and all 4 existing tests still pass, confirming the rename didn't break anything else.

## Comment 2 — Deduplication
**What I did:**
**How I verified:**

## Comment 3 — Missing test
**What I did:**
**How I verified:**

## Comment 4 — Default visibility
**My position:**
**Reasoning:**
**Tradeoff acknowledged:**

## Comment 5 — Sort order
**My position:**
**Reasoning:**
**Engagement with reviewer's point:**

## Comment 6 — Rebase
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->
