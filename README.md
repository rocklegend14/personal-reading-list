# Personal Reading List
A single webpage for saving the books, articles, and links you want to get back to. Entries persist in the browser's localStorage, accessed through a small async api layer (list, add, remove) that mimics a real REST backend including latency and failures, so the loading and error states are genuine, not just mocked up visuals.

## How data is stored
- Storage key: `reading-list:v1:items`, a JSON array in `localStorage`.
- Persistence is per browser/per device, not shared across devices there's no server. To back it with a real API, replace the three functions in the api object (`list`, `add`, `remove`) with `fetch()` calls nothing else in the app needs to change, since every read/write already goes through that layer and every call site already handles the pending/success/error outcomes.

## Reviewing the three states
A Review tools button sits in the bottom-right corner of the page. Open it to reach every state on demand, without editing any code:

| State | How to see it |
|---|---|
| **Loading** |	Happens automatically on every page load (the list is always fetched, never assumed). To inspect it for longer, turn on **Simulate slow connection** in Review tools, then click **Reload list** - the skeleton rows and spinner stay up. Adding or removing an item also shows a spinner on that specific control while its (simulated) request is in flight. |
| **Empty** |	Click **Empty the list** in Review tools. The app clears storage, reloads (showing the loading state first), and lands on the empty state: an explanation of what the list is for plus an **"Add your first book"** button that focuses the form. A brand new visitor with nothing saved yet sees this automatically. |
| **Error** |	Turn on **Simulate network error** in Review tools, then trigger any action: **Reload list** (or refresh the page) shows a full error screen in place of the list; adding or removing an item instead shows an inline red banner, so the existing list stays visible. Every error message names the action that failed (e.g. "add 'X'") and tells you the toggle is the cause and how to stop it. Turn the toggle back off to return to normal. |

Add 3 sample entries is a convenience for going from empty → populated without typing.

## What each state says, specifically

- **Empty**: "Nothing on your list yet. This is where you'll keep the books and articles you want to come back to. Add the first one above, or use the button below," with an "Add your first book" call to action.
- **Loading**: an explicit "Loading your reading list…" caption plus skeleton rows, so it reads as fetching in progress rather than a blank page.
- **Error**: names what failed (e.g. "add 'The Overstory'") and what to do (turn off the simulated error toggle, or, for a real storage failure, that the browser may be blocking storage) with a "Try again" button that re-runs the same request.
