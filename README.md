# Focus card ledger

What the Top 4 Focus Card on lab.algorythmos.com/options predicted, and what the market actually did.

Every trading day:

1. **Before the open**, each card setup (4 names × call and put) is written to `predictions/` and committed. That commit is the proof the card was not changed after the fact. Predictions are never edited.
2. **After the close**, the setups are scored against IBKR 2-minute regular-hours bars. Results go to `outcomes/`, and the bars go to `bars/` with their hash.
3. **Option check (C).** For every filled setup, the option the card's size rule would pick is priced on its own 2-minute bars and written to `options/`. That option is the nearest out-of-the-money strike at or under the premium cap: $2.00 up to the 2026-10-29 session, $15.00 from 2026-10-30.
4. **Rebuild.** `scorecard.json`, `data/setups.csv`, the daily pages in `reports/daily/` and the table below are rebuilt from those files. A weekly report goes to `reports/weekly/`.

Research only. Nothing here places or suggests orders.

## Scorecard

R is measured on the stock, from entry to stop, before option costs. Each rate shows its 95% range. Sessions before 29 Oct 2026 were backfilled after the fact (`backfilled: true`), so they are not proof of a pre-open record. 19-21 Oct are scored on Webull 1-minute bars joined into 2-minute bars, because IBKR no longer had 2-minute bars for them; on days with both, the two sources give the same results (`reports/source_check.json`).

The headline tables use only cards that were really published (from 19 Oct). The rebuilt history (4 Mar – 16 Oct; 2-minute bars from 4 May) sits in its own tables, split by bar size; how it was rebuilt and how far to trust it is in [docs/backfill.md](docs/backfill.md).

<!-- scorecard:start -->
Last scored session: **2026-11-23** · 25 sessions of published cards · 158 sessions of rebuilt history.

### Published cards

| Measure | Result |
| --- | --- |
| Setups published | 199 |
| Triggered | 52% (103/199; 95% 45–59%) |
| Filled after the retest | 50% (52/103; 95% 41–60%) |
| Hit T1 before the stop | 56% (29/52; 95% 42–68%) |
| Stopped | 42% (22/52; 95% 30–56%) |
| Reached T2 | 8% (4/52; 95% 3–18%) |
| Average R, card exits | +0.54R |
| Average R, 5-minute rule | +0.45R |
| Average R, hold for T2 | +0.90R |
| Total R (card / 5-min / T2) | +28.26R / +23.62R / +46.67R |
| Option P&L, 1 contract (card / 5-min / T2) | -$318 / -$177 / +$819 over 35 trades |
| Average best / worst move | +1.64R / -0.99R |
| Median minutes to T1 | 16 |
| Bias read right | 40% (39/98; 95% 31–50%) |

### Last 20 published sessions

| Measure | Result |
| --- | --- |
| Setups published | 159 |
| Triggered | 50% (80/159; 95% 43–58%) |
| Filled after the retest | 50% (40/80; 95% 39–61%) |
| Hit T1 before the stop | 55% (22/40; 95% 40–69%) |
| Stopped | 42% (17/40; 95% 28–58%) |
| Reached T2 | 10% (4/40; 95% 4–23%) |
| Average R, card exits | +0.51R |
| Average R, 5-minute rule | +0.37R |
| Average R, hold for T2 | +0.95R |
| Total R (card / 5-min / T2) | +20.38R / +14.88R / +37.98R |
| Option P&L, 1 contract (card / 5-min / T2) | -$318 / -$177 / +$819 over 35 trades |
| Average best / worst move | +1.65R / -0.96R |
| Median minutes to T1 | 23 |
| Bias read right | 42% (33/78; 95% 32–53%) |

### Rebuilt history, by bar size

Cards re-picked from saved data with the card's own code (`tools/rebuild_cards.py`). Coarse bars miss many retest fills, so read fill and R numbers here as rough; the bias, trigger and day-range facts hold on any bar size. `reports/tier_check.json` measures the gap.

| Measure | Published cards (19 Oct – 23 Nov) | Rebuilt, 2-min bars (4 May – 16 Oct) | Rebuilt, 60-min bars (4 Mar – 1 May) |
| --- | --- | --- | --- |
| Setups published | 199 | 925 | 331 |
| Triggered | 52% (103/199; 95% 45–59%) | 51% (473/925; 95% 48–54%) | 44% (146/331; 95% 39–50%) |
| Filled after the retest | 50% (52/103; 95% 41–60%) | 56% (267/473; 95% 52–61%) | 25% (37/146; 95% 19–33%) |
| Hit T1 before the stop | 56% (29/52; 95% 42–68%) | 43% (115/267; 95% 37–49%) | 65% (24/37; 95% 49–78%) |
| Stopped | 42% (22/52; 95% 30–56%) | 51% (135/267; 95% 45–56%) | 14% (5/37; 95% 6–28%) |
| Reached T2 | 8% (4/52; 95% 3–18%) | 5% (14/267; 95% 3–9%) | 0% (0/37; 95% 0–9%) |
| Average R, card exits | +0.54R | +0.25R | +1.21R |
| Average R, 5-minute rule | +0.45R | +0.22R | – |
| Average R, hold for T2 | +0.90R | +0.18R | +0.54R |
| Total R (card / 5-min / T2) | +28.26R / +23.62R / +46.67R | +66.60R / +58.74R / +47.80R | +44.67R / – / +20.08R |
| Option P&L, 1 contract (card / 5-min / T2) | -$318 / -$177 / +$819 over 35 trades | – / – / – over 0 trades | – / – / – over 0 trades |
| Average best / worst move | +1.64R / -0.99R | +1.51R / -0.96R | +2.31R / -0.91R |
| Median minutes to T1 | 16 | 18 | – |
| Bias read right | 40% (39/98; 95% 31–50%) | 40% (181/454; 95% 36–44%) | 40% (65/161; 95% 33–48%) |

Card score vs R (information coefficient), rebuilt history: 0.051; rank vs R: 0.044.

#### Rebuilt history by bias

| Group | Setups | Bias read right | Triggered | Hit T1 before the stop | Average R, card |
| --- | --- | --- | --- | --- | --- |
| breakout | 522 | 44% (116/261; 95% 38–50%) | 51% (266/522; 95% 47–55%) | 47% (61/129; 95% 39–56%) | +0.38R |
| range | 19 | – | 32% (6/19; 95% 15–54%) | 67% (2/3; 95% 21–94%) | +0.95R |
| support | 715 | 37% (130/354; 95% 32–42%) | 48% (347/715; 95% 45–52%) | 44% (76/172; 95% 37–52%) | +0.34R |

#### Rebuilt history by where the stock opened

| Group | Setups | Bias read right | Triggered | Hit T1 before the stop | Average R, card |
| --- | --- | --- | --- | --- | --- |
| Opened between floor and ceiling | 713 | 38% (131/348; 95% 33–43%) | 46% (326/713; 95% 42–49%) | 45% (86/191; 95% 38–52%) | +0.37R |
| Opened over the ceiling | 310 | 74% (111/151; 95% 66–80%) | 54% (167/310; 95% 48–59%) | 42% (25/60; 95% 30–54%) | +0.18R |
| Opened under the floor | 233 | 3% (4/116; 95% 1–8%) | 54% (126/233; 95% 48–60%) | 53% (28/53; 95% 40–66%) | +0.57R |

#### Rebuilt history by card rank

| Group | Setups | Bias read right | Triggered | Hit T1 before the stop | Average R, card |
| --- | --- | --- | --- | --- | --- |
| 1 | 316 | 38% (60/158; 95% 31–46%) | 49% (155/316; 95% 44–55%) | 47% (34/72; 95% 36–59%) | +0.55R |
| 2 | 316 | 42% (66/156; 95% 35–50%) | 52% (165/316; 95% 47–58%) | 41% (32/78; 95% 31–52%) | +0.19R |
| 3 | 315 | 38% (58/154; 95% 30–46%) | 47% (147/315; 95% 41–52%) | 51% (38/74; 95% 40–62%) | +0.48R |
| 4 | 309 | 42% (62/147; 95% 34–50%) | 49% (152/309; 95% 44–55%) | 44% (35/80; 95% 33–55%) | +0.26R |

#### Rebuilt history by how far the level was from the close

| Group | Setups | Bias read right | Triggered | Hit T1 before the stop | Average R, card |
| --- | --- | --- | --- | --- | --- |
| 0.5-1 ATR | 226 | 38% (83/216; 95% 32–45%) | 26% (59/226; 95% 21–32%) | 53% (17/32; 95% 36–69%) | +0.52R |
| <0.5 ATR | 983 | 40% (243/611; 95% 36–44%) | 57% (557/983; 95% 54–60%) | 45% (122/271; 95% 39–51%) | +0.35R |
| >1 ATR | 47 | 55% (21/38; 95% 40–70%) | 6% (3/47; 95% 2–17%) | 0% (0/1; 95% 0–79%) | -1.32R |

### 2026-11-23

| Setup | Level → entry / stop / T1 / T2 | Result | R card | R 5-min | Option $ |
| --- | --- | --- | --- | --- | --- |
| GOOGL call (#1, support) | 272.36 → 272.47 / 271.22 / 274.97 / 283.15 | no fill | – | – | – |
| GOOGL put (#1, support) | 264.22 → 264.11 / 265.36 / 261.61 / None | no trigger | – | – | – |
| NVDA call (#2, support) | 143.66 → 143.79 / 142.38 / 146.61 / 163.0 | no fill | – | – | – |
| NVDA put (#2, support) | 124.52 → 124.39 / 125.8 / 121.57 / None | no trigger | – | – | – |
| PLTR call (#3, support) | 152.5 → 152.6 / 151.52 / 154.76 / 167.13 | no fill | – | – | – |
| PLTR put (#3, support) | 142.29 → 142.19 / 143.27 / 140.03 / 126.97 | no trigger | – | – | – |
| SPY call (#4, support) | 715.65 → 715.81 / 714.04 / 719.35 / 726.79 | target1 | +2.00R | +2.00R | +$70 |
| SPY put (#4, support) | 686.76 → 686.6 / 688.37 / 683.06 / None | no trigger | – | – | – |

<!-- scorecard:end -->

## How a setup is scored (scorer 1.1)

- **Trigger:** a 2-minute close beyond the card's level.
- **Fill:** a later bar touches the entry and closes back on the trade side of the level. This is the card's "then a dip holds it" or "then a bounce fails under it". It must happen outside the no-entry windows, stored in New York time so clock changes don't move them: 09:30-09:35 and 11:30-13:00 ET. That is before 00:35 and 02:30-04:00 Sydney while both cities are on daylight time. A close through the stop before any valid retest kills the setup.
- **Stop:** a 2-minute close through the stop. **Targets:** the bar's high or low reaches them.
- **Same-bar conflict:** if one bar both reaches T1 and closes through the stop, it is flagged and scored as a stop.
- **Exit rules, all three scored:**
    - `card`: T1, the stop, or the session close.
    - `five_min`: the card rules, but out at about 5 minutes if the trade isn't in profit.
    - `hold_t2`: T2, the stop, or the session close.
- **Option P&L (C):**
    - **Entry:** the option's 2-minute close at the stock fill bar.
    - **Exit:** the earlier of the −25% option stop and the stock exit.
    - **Basis:** IBKR last-trade bars, with no spread, slippage or fees.
- **Bias:** "hold support" is right if the day's low stayed above the floor. "Breakout watch" is right if the session closed above the ceiling.
- **Confidence (v1):** set before the open from pre-market. A setup is **high** when the pre-market already leans its way, **low** when the pre-market gaps against it, and **medium** otherwise.

## Layout

```
predictions/YYYY/DATE.jsonl     before the open, append-only
outcomes/YYYY/DATE.jsonl        after the close, linked by id + prediction sha256
options/YYYY/DATE.jsonl         option result per filled setup
bars/YYYY/DATE/SYMBOL.json      the bars each outcome was scored on
scorecard.json, data/setups.csv derived; rebuilt by `python -m ledger scorecard`
reports/daily, reports/weekly   readable pages
schema/                         required fields per record type
missed_sessions.json            NYSE sessions with no prediction or no outcome, each with the reason
tools/                          rebuild_cards.py (past cards), tier_check.py (coarse-bar error bar)
docs/backfill.md                how the rebuilt history was made and how far to trust it
```

## Missed sessions

Every NYSE session since the first one in the ledger must have predictions once 09:30 New York has passed and
outcomes once 17:00 New York has passed. `python -m ledger validate --gaps` fails for any session missing either
that is not listed in `missed_sessions.json` with the reason. The list is append-only, like `predictions/`. CI runs
the check on every push and on weekdays at 22:45 UTC (GitHub may run scheduled checks late), so a day with no push
still fails a check. The calendar is `ledger/sessions.py`. A missed session is listed, never back-filled silently;
if it is back-filled later, its predictions carry `backfilled: true`.

## Commands

```
python -m ledger predict --card options-tab/data/focus/2026-10-29.json
python -m ledger bars --name SPY --ibkr spy_2min.json
python -m ledger score --session 2026-10-29
python -m ledger options --session 2026-10-29 --manifest options_manifest.json
python -m ledger scorecard
python -m ledger report --week 2026-W44
python -m ledger validate origin/main
python -m ledger validate --gaps
python -m unittest discover tests
```
