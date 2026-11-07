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
- Added a Start button so the sliders can be set before the simulation begins (see the problems below for why this was needed).

## Problems found in my own method
1. The simulation started as soon as the page loaded, so the first generations mutated at the default settings before I could move the sliders. Those traits were then carried through the whole run. Fix: added a Start button, so I set the sliders first and then begin.
2. Speed was almost free (cost proportional to speed), so it sat at its cap of 4 in every run. Fix: made the speed cost quadratic, and vision now costs a little energy too, so both traits have a trade-off.
3. Earlier trait numbers came from those flawed runs, so I discarded them and re-ran the experiments with the fixed code.
- Learned: check that an experiment can actually fail before trusting its results.

## Control: mutation 0
Settings: growth 0.3, cost 0.1, mutation 0.
- Creatures 30 to 40, plants 10 to 15, avg speed 2.04, avg vision 84 to 85.
- With no mutation, traits can't evolve. Speed stayed inside the starting range (0.8 to 2.2), which confirms the fixes work and that the changes in the other runs come from evolution.

## Experiment 1: effect of food (plant growth)
Method: reload, set sliders, click Start, wait about 4 minutes, read the stats. Cost 0.1 and mutation 1 for all runs. One run per setting.

| Plant growth | Creatures | Plants   | Avg speed    | Avg vision |
|--------------|-----------|----------|--------------|------------|
| 0.1          | 9 to 11   | 10 to 15 | 3.1          | 115 to 119 |
| 0.3          | 25 to 30  | 10 to 15 | 2.8 to 2.9.  | 83 to 84   |
| 0.5          | 45 to 55  | 10 to 20 | 2.75 to 2.77 | 68 to 69   |

Findings:
- More food gave more creatures at every step.
- Scarce food produced much longer vision (about 117) than abundant food (about 68). Likely explanation: seeing far pays off when plants are rare and isn't worth its energy cost when food is everywhere. This also appeared in an earlier, less clean batch, which makes it more convincing.
- Speed changed only slightly (3.1 down to 2.75), so this is a weak trend.

## Experiment 2: living cost
Settings: growth 0.3, mutation 1, cost 0.4. One run.
- Creatures 10 to 15, plants 15 to 50, avg speed 2.8 to 3.0, avg vision 78 to 81.
- Higher living cost gave fewer creatures than cost 0.1 (about 27).
- Vision and speed didn't change clearly, so no claim about cost effects on traits.

## Limitations
- One run per setting, so these are patterns, not confirmed conclusions.
- Stats were read by eye from the screen, not exported.
- Only plant growth was tested across several levels. Mutation size above 1 and cost between 0.1 and 0.4 were not tested.

