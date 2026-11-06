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

## Sliders
- Added sliders for plant growth, living cost and mutation size so the rules can be changed while the simulation runs.

## First experiment: plant growth
- High plant growth: creature count rises and average speed rises quickly, while average vision falls.
- Low plant growth: the opposite pattern.
- Average speed and vision looked like they moved together on the graph, but that needs checking against the actual numbers, because the two lines use separate axes.

## Experiment results
()