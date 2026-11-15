# Yahtzee Scorer Handover

Updated: 2026-11-15

## Current App

- Live app: https://cdunhill.github.io/yahtzee-scorer-2/
- Repository: https://github.com/CDunhill/yahtzee-scorer-2
- Deployment branch: `main`
- Latest published commit: `2b5f012` (`Restore plain text email report layout`)
- App version shown in the page: Yahtzee Scorer 2.5
- The app is currently a single-file static site in `index.html`, hosted by GitHub Pages.

## Implemented

- Email report includes all score categories and calculated rows, including subtotals, upper bonus, lower total, and grand total. It uses the original plain-text per-player layout; the newer ASCII table borders were removed.
- Score keypad order is `1` through `9`, `CLEAR`, `0`, `ENTER`. `CLEAR` immediately empties the selected cell and closes the keypad; entering zero displays `/` in the score sheet.
- Physical keyboard scoring is supported while the keypad is open: number keys enter digits, Enter commits, Space clears, and Escape closes the keypad.
- Quick Restart lets the user choose the starting player and rotates the existing player order.
- Settings include Light, Dark, and Forest themes; a default email; a Google Sheets URL/ID placeholder; and Clear Game and Reload App. Theme and email preferences are stored locally on each device. Google Sheets is not integrated.
- A positive Yahtzee-category score triggers full-screen confetti and the animated `YAHTZEE!` letters. Reduced-motion preferences suppress the animation.
- The finish/email flow has a confirmation dialog.

## Email Decision

The app currently uses `mailto:`, so the report is plain text. HTML email was deferred because it needs a server-side sender; `mailto:` cannot reliably set an HTML message body. The user prefers the original plain-text report for now, without ASCII table borders.

If revisiting HTML email, the discussed option was a serverless endpoint (for example, Cloudflare Worker) connected to a transactional email provider (for example, Resend). GitHub Pages can remain the frontend and keep the same app URL. Account creation, sender-domain verification, and secret configuration must be done by the user. Never put an email API key in the public app or ask the user to share it in chat. Pricing reviewed on 2026-11-15 indicated free tiers may be sufficient for light personal use, subject to provider limits and current terms.

## Validation And Known Gaps

- Embedded JavaScript syntax, editor diagnostics, and `git diff --check` have passed after recent changes.
- Browser checks covered score clearing, zero display, restart ordering, settings persistence, complete email rows, keyboard entry, and the celebration on desktop/mobile-sized viewports.
- The animation fix uses a consistent transform-function sequence to improve iOS WebKit interpolation. It has not been physically tested on an iPhone or Android device; confirm the effect on the user's iPhone Chrome after Pages deploys.
- Android is expected to work in a modern browser, but has not been device-tested.
- Email opening depends on the device having a `mailto:` handler configured.
- The optional turn-tracking idea remains a design suggestion only: manual skip/rejoin behavior in round-robin order.

## Working Notes

- Source of truth: `index.html`.
- Pushes to `main` update the existing GitHub Pages app after deployment completes.
- Do not commit credentials, provider API keys, or other secrets.