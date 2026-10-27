# Mafia / After Dark

A minimal, mobile-first Mafia game for 5-20 people sitting together and passing one phone. No account, server, external library or build step is needed to play.

## Play

1. Enter one unique player name per line. Choose the Mafia count (or auto), Doctor, Detective and Cutie.
2. Pass the phone to each named player. Reveal your own role, then hide before handing it over.
3. Each night, every living player opens a private turn. Mafia vote for a target, Doctor protects, Detective investigates, Cutie mutes one other player for the following day, and Town make a decoy selection.
4. Discuss out loud. Pass the phone for private votes. The app resolves eliminations and checks victory automatically.
5. At the end, review the game and rematch with the same players.

## House rules

- Town wins at zero Mafia. Mafia wins when living Mafia equal or outnumber living Town, including Doctor, Detective and Cutie.
- Mafia know their teammates and cannot target them. A unique top target is selected; a tied night target is chosen randomly.
- Doctor may protect themself or the same player on consecutive nights. Night protection and kill resolve together, after all turns.
- Detective checks one other living player: Mafia or not Mafia. The result is private and survives a tab lock or reload until the turn is completed.
- Day votes use plurality, not an absolute majority. No self-votes. No elimination is a ballot option. A tie gets one runoff between tied options; a second tie causes no elimination.
- Eliminated roles become public. Eliminated players cannot act, speak or vote.
- Timing may reveal clues. Keep all private turns roughly equal in length.

Classic rules reference: https://www.houseofmafia.com/en/rules

One-phone app pattern reviewed: https://play.google.com/store/apps/details?id=com.awaytix.mafia

The tie, protection and secret-ballot rules above are this app's chosen variant. No assets or code were copied from these references.

Cutie is a Town role based on the requested house rule. They choose one other living player each night. That player stays silent and cannot vote during the next day, including a runoff, but can still be voted out. Their night power is unaffected. The mute expires next night; repeat targets are allowed. A simultaneous night elimination does not undo the choice. Cutie is enabled by default and can be switched off in setup.

There is no discussion timer or separate discussion screen. Talk as long as you like, then use the direct private-voting button on the morning screen. Existing saved games remain resumable without adding a new role midgame; start a new game to deal Cutie.

## Privacy and persistence

Game progress, roles and votes stay in this browser's localStorage. No analytics or game-data requests are sent. The interface masks secrets on tab visibility changes, blur and page exit. This is a casual trust-based party game, not a tamper-proof system: browser developer tools can inspect the saved state, and screenshots cannot be prevented by a web page.

Reload returns to a covered resume screen. Clearing site data removes progress. Network access is needed to initially load the page; offline reload is not guaranteed.

## Run locally

Open `index.html` in a modern browser, or serve this folder with `python -m http.server 8080` and open `http://localhost:8080`.

The entire app is in `index.html`, with a pure `Rules` object for line-ups, win conditions, vote counting and night resolution. Random dealing uses the Web Crypto API with rejection sampling and Fisher-Yates shuffle.

This repository is a browser web app, not an Android APK/Gradle project. An Android package would require a separate wrapper and build setup.

## Verification

Tested in Chromium with a 390px mobile viewport. Fourteen engine checks cover 5-20 player line-ups, invalid parity, victory, protection, dead-role exclusion, night ties, day ties, no-elimination and invalid votes. Browser integration covers role dealing, night actions, investigation, day discussion, private voting, game-over, rematch, reload and privacy locks. Horizontal overflow checked at 320, 390, 768 and 1440px. No browser script errors observed in the tested flow.

Automated tests can be run from the companion `test.cjs` using Node.js and Playwright:

```sh
npm install --no-save @playwright/test
npx playwright install chromium
node test.cjs
```
