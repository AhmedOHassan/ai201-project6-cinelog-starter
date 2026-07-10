# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename
**What I did:** Renamed `save_to_watchlist()` to `add_to_watchlist()` in `services/watchlist_service.py` so it matches the `verb_to_noun` convention already used by `add_to_collection()` in `services/collection_service.py`. Updated the docstring's first line to say "Add a film" instead of "Save a film" so it stays consistent with the new name. Updated the one call site in `routes/watchlist/watchlist.py`, both the import and the function call.
**How I verified:** Searched the whole project for `save_to_watchlist` after the rename and got zero matches, so I know every call site was caught. Ran `pytest tests/ -v` and all 4 existing tests still pass, confirming the rename didn't break anything else.

## Comment 2 — Deduplication
**What I did:** Followed the same pattern `add_to_collection()` uses in `services/collection_service.py`: before creating the `WatchlistEntry`, I query for an existing entry with the same `user_id` and `film_id`, and if one exists I raise a new `AlreadyInWatchlistError` instead of silently inserting a duplicate. I also updated `routes/watchlist/watchlist.py` to catch this new error and return a 409, and to catch `FilmNotFoundError` and return a 404, matching how `routes/collection.py` handles the same two cases for the collection endpoint (this second part wasn't strictly asked for, but the watchlist route was letting both errors bubble up as unhandled 500s, which didn't match the rest of the app).
**How I verified:** Started `tests/test_watchlist.py`, following the same fixture structure as `tests/test_collection.py`. Added `test_add_to_watchlist_creates_entry` for the happy path and `test_add_to_watchlist_duplicate_raises`, which adds the same film twice and asserts the second call raises `AlreadyInWatchlistError` while only one entry ends up in the database. Both pass. Also ran `pytest tests/ -v` to confirm the existing collection tests still pass unaffected.

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
