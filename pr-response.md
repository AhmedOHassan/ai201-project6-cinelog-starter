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
**What I did:** Added `test_add_to_watchlist_nonexistent_film_raises` to `tests/test_watchlist.py`, using `test_add_to_collection_nonexistent_film_raises` in `tests/test_collection.py` as the model.
**How I verified:** Ran `pytest tests/test_watchlist.py -v` and confirmed the new test passes alongside the other two watchlist tests. Ran the full suite with `pytest tests/ -v` and all 7 tests pass (4 collection + 3 watchlist).

## Comment 4 — Default visibility
**My position:** I'm keeping `public=True` as the default, and I want to be clear that this is a deliberate choice, not something I inherited without thinking about it.

**Reasoning:** A watchlist is different from a collection. A collection is a record of what someone has already watched, which feels more personal and retrospective. A watchlist is a list of what someone wants to watch, and I think that's naturally more outward facing. CineLog is a film tracking app, and in that genre of app, public activity by default is the norm, not the exception. Letterboxd, the app CineLog is clearly closest to in spirit, defaults diaries, reviews, and lists to public, and that's part of what makes the category work: people log and browse what others are watching as a normal part of using the app. I'm following that same convention here rather than treating CineLog as a private utility where sharing is the exception.

**Tradeoff acknowledged:** The real cost of this default is that a user who doesn't think about privacy at all ends up exposing their watchlist without ever making that choice. That's a real risk, especially for someone who might not want people knowing they're planning to watch something they'd find embarrassing or that doesn't match how they present themselves publicly. I'll admit CineLog doesn't currently have anything in this codebase, like a signup notice or a friends system, that actively tells a user their watchlist is public or lets them do anything social with that visibility yet. So right now this default is really just matching the norm for the category of app CineLog is, not something the product is actively taking advantage of. I still think `public=True` is the right default to build toward that norm from, but I'd treat clearly communicating this default to users, and building the features that make public visibility actually useful, as follow-up work rather than something this PR needs to solve.

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
