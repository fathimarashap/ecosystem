# Build Log: Evolving Ecosystem Simulator

## Wandering creatures that eat
- Built a canvas world with plants and creatures. Each creature wanders, finds the nearest plant inside its vision range, and eats it.
- Learned: vision decides how well a creature finds food, and a higher speed makes it overshoot plants.

## Energy, reproduction and death
- Added energy: eating gives energy, moving costs energy, a creature reproduces above 100 energy and dies at 0.
- Problem: creatures were reproducing but nobody was dying, because plants grew too fast and food was too easy.
- Dead creatures were also removed from the list instantly, so deaths were invisible.

## Making the simulation readable
- Colour creatures by energy and show a fading red ring where a creature dies.
- Learned: visualising internal state made the simulation far easier to debug.

## Evolution
- Gave creatures heritable traits (speed and vision) that mutate slightly in offspring, and started the first generation with random values.
- Learned: mutation only changes babies, so the first generation needs variety or nothing visibly changes.

## Live graphs
- Added two Chart.js graphs: population and average traits.
- Bug: the graphs did not show up. The chart containers were inside the info div, and the code that updates that div's text every frame was deleting them. Fix: moved the charts outside.
- Learned: setting `textContent` on an element replaces everything inside it.

## Sliders and Start button
- Added sliders for plant growth, living cost and mutation size, and a Start button so the sliders can be set before the simulation begins.

## Problems found in my own method
1. The simulation started as soon as the page loaded, so the first generations mutated at the default settings before I could move the sliders. Fix: added a Start button.
2. Speed was almost free, so it sat at its cap in every run. Fix: made the speed cost quadratic, and vision now costs a little energy too, so both traits have a trade-off.
3. Trait numbers from those flawed runs were discarded and the experiments were re-run with the fixed code.
- Learned: check that an experiment can actually fail before trusting its results.

## Preliminary checks (read by eye, one run each)
- Control, mutation 0: average speed 2.04, inside the starting range (0.8 to 2.2). With no mutation traits cannot evolve, so this confirmed the fixes work.
- Living cost 0.4: about 10 to 15 creatures against about 25 to 30 at cost 0.1. Higher living cost gave fewer creatures.

## Faster experiments: speed control and CSV export
- Added a simulation speed selector (1x to 50x) and a CSV export with a row every 30 steps, plus the slider settings.
- The first exported run showed average speed rising from about 1.5 to about 3.2 and still climbing. The population boomed, crashed when plants ran out, then oscillated.
- The same settings gave different numbers from my earlier by-eye reading, so there is real run-to-run variation. I moved to longer, repeated runs.
- Made runs stop automatically at 30,000 steps and download their CSV. Wrote `analyze.html` to read the CSVs and plot results.

## Experiment A: effect of food supply (plant growth)
Method: 15 runs, growth from 0.10 to 0.50, run in shuffled order. Cost 0.1 and mutation 1 fixed. One run per value.

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
- Population grows almost linearly with food. At growth 0.10 it went extinct.
- Vision tends to be longer when food is scarce: the 8 surviving runs at growth 0.33 or below averaged about 112, and the 6 runs at 0.36 or above averaged about 81. Neighbouring runs vary a lot, so this is a tendency, not an exact curve. A likely explanation is that seeing far pays off when plants are rare, but this is a hypothesis.
- Speed was about 3.9 in almost every run because it was capped at 4 in the code, so this experiment cannot tell us how speed responds to food.

## From simulation to game
- Wanted the project to feel like a game, so I added "god powers": drop plants, spawn creatures, drought (removes most plants and stops regrowth for a while) and food bloom.
- Extinction no longer ends the world, because spawning new creatures restarts life.
- Learned: interactivity makes the evolution much easier to understand and demo.

## 3D version
- Rebuilt the view in 3D with Three.js: low-poly meadow, trees, bush-like plants and rounded creatures, with a rotate and zoom camera.
- Kept the simulation logic unchanged and only changed how it is drawn. Plants, creatures and death rings use instanced meshes so many objects draw quickly.
- Kept the 2D version as a fallback in case 3D did not work out.

## Environment engine and final polish
- Added a day/night cycle and four seasons. Seasons change plant growth, and the sky, lighting and ground colour follow the time and season, so visuals reflect real changes in the simulation.
- Experiment mode turns the cycle off, so experiments stay comparable.
- Added pinch-to-zoom, a speed limit on phones, a fallback message if 3D is unavailable, and an About section with the findings.
- Deployed with GitHub Pages and wrote the README.

## Limitations
- One run per growth value, so findings are patterns, not confirmed conclusions.
- Speed cap of 4 hid how speed responds to food.
- Only plant growth was tested systematically. Living cost and mutation size had only preliminary checks.
- Not yet tested on a wide range of phones.

## Future work
- Predators, with both sides evolving.
- Weather with real effects, birds, butterflies and fireflies.
- Raise the speed cap and repeat Experiment A with several runs per setting.
- Run systematic experiments on living cost and mutation size.