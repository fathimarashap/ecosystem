# Build Log: Evolving Ecosystem Simulator

## Wandering creatures that eat
- Built a canvas world with plants and creatures. Each creature wanders, finds the nearest plant inside its vision range, and eats it.
- Learned: vision decides how well a creature finds food, and a higher speed makes it overshoot plants.

## Energy, reproduction and death
- Added energy: eating gives energy, moving costs energy, a creature reproduces above 100 energy and dies at 0.
- Problem: creatures were reproducing but nobody was dying, because plants grew too fast and food was too easy.
- Dead creatures were also removed from the list instantly, so deaths were invisible.

## Making the simulation readable
- Colour creatures by energy (white = healthy, red = starving) and show a fading red ring where a creature dies.
- Learned: visualising internal state made the simulation far easier to debug.

## Evolution
- Gave creatures heritable traits (speed and vision) that mutate slightly in offspring, and started the first generation with random values.
- Made creature size show vision, and added average speed and vision to the stats line.
- Learned: mutation only changes babies, so the first generation needs variety or nothing visibly changes.

## Live graphs
- Added two Chart.js graphs: population (creatures and plants) and average traits (speed and vision).
- Bug: the graphs did not show up. The chart containers were inside the info div, and the code that updates that div's text every frame was deleting them.
- Fix: moved the charts outside the info div.
- Learned: setting `textContent` on an element replaces everything inside it.

## Sliders and Start button
- Added sliders for plant growth, living cost and mutation size so the rules can be changed while the simulation runs.
- Added a Start button so the sliders can be set before the simulation begins.

## Problems found in my own method
1. The simulation started as soon as the page loaded, so the first generations mutated at the default settings before I could move the sliders. Those traits carried through the whole run. Fix: added a Start button.
2. Speed was almost free (cost proportional to speed), so it sat at its cap in every run. Fix: made the speed cost quadratic, and vision now costs a little energy too, so both traits have a trade-off.
3. Trait numbers from those flawed runs were discarded and the experiments were re-run with the fixed code.
- Learned: check that an experiment can actually fail before trusting its results.

## Preliminary checks (read by eye, one run each)
- Control, mutation 0 (growth 0.3, cost 0.1): avg speed 2.04, which is inside the starting range (0.8 to 2.2). Traits can't evolve without mutation, so this confirmed the fixes work.
- Living cost 0.4 (growth 0.3, mutation 1): about 10 to 15 creatures, against about 25 to 30 at cost 0.1. Higher living cost gave fewer creatures. Vision and speed did not change clearly.

## Faster experiments: speed control and CSV export
- Added a simulation speed selector (1x to 50x) and a Download CSV button. A row is logged every 30 steps, along with the slider settings.
- First exported run (growth 0.3, cost 0.1, mutation 1): average speed rose from about 1.5 to about 3.2 over the run and was still climbing, so evolution was clearly happening. The population boomed, crashed when the plants ran out, then settled into oscillation.
- The same settings gave different numbers to my earlier by-eye reading, so there is real run-to-run variation. I dropped the early speed claim and moved to repeated, longer runs.
- Made runs stop automatically at a fixed length and download their CSV.
- Wrote `analyze.html`, which reads the CSVs in the browser and builds a table and scatter plots, averaging the last 3000 steps of each run.

## Experiment A: effect of food supply (plant growth)
Method: 15 runs, growth from 0.10 to 0.50, run in shuffled order. Cost 0.1 and mutation 1 fixed. One run per value. Reload, set sliders, Start, run to the fixed length, download CSV.

| Growth | Creatures | Avg speed | Avg vision |
|---|---|---|---|
| 0.10 | extinct | - | - |
| 0.13 | 7.8 | 3.91 | 145.3 |
| 0.16 | 10.2 | 3.93 | 97.3 |
| 0.17 | 10.2 | 3.94 | 123.5 |
| 0.21 | 13.4 | 3.84 | 110.8 |
| 0.24 | 15.4 | 3.87 | 102.5 |
| 0.27 | 16.9 | 3.91 | 112.4 |
| 0.30 | 18.4 | 3.91 | 99.1 |
| 0.33 | 20.5 | 3.90 | 107.1 |
| 0.36 | 23.5 | 3.86 | 81.8 |
| 0.39 | 24.1 | 3.88 | 91.6 |
| 0.42 | 26.7 | 3.86 | 76.2 |
| 0.44 | 28.7 | 3.81 | 83.5 |
| 0.47 | 32.5 | 3.65 | 81.9 |
| 0.50 | 32.8 | 3.87 | 71.8 |

Findings:
- Population grows almost linearly with food, from about 8 creatures at growth 0.13 to about 33 at 0.5. At growth 0.10 the population went extinct.
- Vision tends to be longer when food is scarce. The 8 surviving runs at growth 0.33 or below averaged about 112, and the 6 runs at 0.36 or above averaged about 81. Neighbouring runs vary a lot (0.16 gave 97, 0.17 gave 123.5), so this is a tendency, not an exact curve. A likely explanation is that seeing far pays off when plants are rare, but this is a hypothesis.
- Speed was about 3.9 in almost every run, because it was capped at 4 in the code. So speed cannot tell us anything about food in this experiment.

## Limitations
- One run per growth value, so these are patterns, not confirmed conclusions.
- The speed cap of 4 hid any response of speed to food.
- Only plant growth has been tested systematically so far. Living cost and mutation size have only preliminary checks.

## Next steps
- Raise the speed cap (4 to 8) and re-run the 15 growth values.
- Experiment B (living cost) and Experiment C (mutation size), one setting changed at a time.
- Possibly a third level of the food chain (predators).
- Deploy to GitHub Pages, write the README, record the demo video, write the report.