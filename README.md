# Tempo

A complete mobile-first workout tracker. Plain ES modules, CSS, and IndexedDB; no runtime dependencies or build step. The app lives in `tempo/`, and deployable assets are in `tempo/dist/`.

## Run locally

```sh
npm run dev
```

Open http://localhost:4173. For phone installation and service workers, deploy `tempo/dist/` to HTTPS. On iPhone, use Safari → Share → Add to Home Screen. On Android, use the browser's Install app / Add to Home screen action.

## Features

- 33 exact starting exercise references; unknown reps remain null, with no invented workout history.
- Workout player with decimal or zero weights, reps, warm-up labels, notes, per-exercise conventions, previous performance, and set edit/delete/undo.
- Editable routines, planned sets/targets/rest, exercise picker, exercise replacement and ordering.
- Saved workout and rest timers based on timestamps; explicit training pauses excluded.
- Real history, editable sets/notes/duration, exercise comparisons and weight charts.
- Pounds by default; kilograms convert against canonical pounds without doubling dumbbell loads.
- Light/dark/system appearance, reduced motion support, safe area spacing, numeric input, phone navigation.
- Installable PWA with a cached local application shell and offline logging.
- JSON backup/restoration and completed-set CSV export.

## Data and offline behavior

Data is stored in IndexedDB in this browser on this device. A synchronous local recovery journal protects the latest edits during refresh. There is no account sync or analytics. Clearing browser storage or using another browser/device does not carry workouts over. Use Settings → Export JSON backup regularly, and restore through Settings on another device.

Starting references never count as workouts or personal records. The two editable starter routines are suggestions with their own planned sets. Weight conventions start unspecified except Weighted Pull Ups, which use added external load. Updating a convention changes its label and does not rescale any logged weight.

Timers are reconciled on visibility changes and reopening. Browsers may suspend sounds, vibration, and notifications in the background; no background or lock-screen delivery is guaranteed. Notifications are requested only when the preference is explicitly enabled and saved. iPhone notification support may require home-screen installation.

Wait for Settings to report that offline logging is ready after the initial load. The service worker caches the app itself; workout data remains in IndexedDB. A hosting service with private authentication may require an initial sign-in while online.

## Verification

```sh
npm test
npm run check
npm --prefix tempo install
npm run test:browser
```

The browser verification uses the installed Google Chrome at `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`. Run the local server first. Test output and screenshots go to the ignored `tempo/test-results/` directory. To use another Chrome installation, update the executable path in `tempo/tests/browser.mjs`.

The browser checks cover logging, set editing/deleting/undo, decimal and zero loads, unknown reps, timers and pauses, refresh restoration, routines and history, unit conversion, JSON export/import, offline reload and logging, and horizontal overflow at 320–430px widths. Simulated clock jumps check timer arithmetic; actual operating-system background alerts remain browser-dependent.
