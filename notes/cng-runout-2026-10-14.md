# CNG run-out findings — VICTORIS, 14 Oct 2026

## Switch to petrol
- Trip: `trips/VICTORIS/2026-10-14_1791988647102.csv` (+ `.json`)
- Car's fuel status changed CNG -> PETROL at **20:22:08 IST** (t_ms 1791989528536), ~40 km/h.
- App log `logs/app-20261014.log` line 145, **20:22:49 IST**: "CNG run-out detected (VICTORIS), tank anchor reset to empty" (~40 s late).
- Trip: 8.6 km on CNG, 0.7 km on petrol.

## CNG pressure (`cng_p_bar`, last CSV column, logged only since 14 Oct, every ~30 s)
- Morning trip `2026-10-14_1791950005006`: 85.5 -> 80.9 bar.
- Evening: 40.5 bar at 20:07 -> **35.3 bar at 20:21:51**, falling ~0.35 bar/min, then switched to petrol.
- On petrol afterwards it stayed at 35.4–35.8 bar.
- **Conclusion: tank is effectively empty at ~35 bar.** That pressure should be 0%.

## Gauge problem
- The app's CNG % showed **7–8%** when it switched to petrol. 0% needs to move up to ~35 bar,
  or the gauge needs rescaling so the old 7–8% reading becomes 0%.
- The gauge code is in the app source, not in this repo. This repo only has uploaded logs/trips/settings.

## Fill history (why capacity can't be computed yet)
- `summary/fills.json` has only one fill: 6.27 kg full, 27 Sep 2026 19:35 IST.
- Trips since then used ~20.4 kg CNG (sum of `cngRawKg`), so ~3 refills weren't logged.
- `settings/current.json`: `cng_capacity_kg_victoris` = 6.5 (unverified), `cng_runouts_VICTORIS` = 1791989569163.

## Next steps (for the local session with the app source)
1. Log today's fill in the app as **full**, with the kg amount. Kg from 0% (run-out, ~35 bar) to full = real usable capacity.
2. Note the pressure right after filling (first `cng_p_bar` on the next trip) = full pressure.
3. In the app source, make the CNG % gauge read 0% at ~35 bar (or at the run-out anchor) and 100% at full pressure.
   Then set `cng_capacity_kg_victoris` to the measured usable kg.
