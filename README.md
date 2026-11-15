# Barrilete Cosmico

Autonomous health monitoring simulator for astronauts on deep-space missions.

Team: Barrilete Cosmico, Caleta Olivia, Santa Cruz, Argentina (Escuela Industrial N 9 "15 de Noviembre").
Event: NASA International Space Apps Challenge 2026.
Challenge: software for monitoring the health of astronauts.

## Summary

On a mission to Mars, a radio message can take many minutes to reach Earth, and the answer takes just as long to come back. When an astronaut is in danger, the crew cannot wait for Mission Control.

Barrilete Cosmico is a simulator that lets an astronaut check their own health on board, with no contact with Earth. It reads simulated data from a wristband and a suit, compares every value with that astronaut's own baseline, warns of physical and mental risks, and guides the crew through emergency protocols. Earth receives the data later, after the communication delay.

This is a simulation. It uses simulated data and does not run on real hardware.

## What it does

- Wristband: LED status and readings per astronaut (pulse, stress, sleep, radiation).
- Suit: lights on the chest that show the astronaut's state, and a fainting behavior when the time to reach shelter runs out.
- Tablet: on-board view with the recommended actions.
- Radiation: dose tracking against the career limit and the per-event limit, a countdown to reach the storm shelter, and a dose curve for solar particle events.
- Heart: resting pulse, estimated VO2 max, heart rate recovery after exercise, heart rate variability, and a simple model of how the heart adapts to microgravity.
- Habitat: isolation, distance, gravity and environment modules for each astronaut.
- Friction: a tension map between crew members, with suggestions for who should work with whom.
- Planner: daily plan that takes the above into account.
- Earth view: what Mission Control knows, which is always behind the real state by the communication delay.
- Store and forward: data is stored on board in a local SQLite database and sent to Earth when the link is available.
- Live solar flares: optional load of recent flare events from NASA DONKI, which can be replayed as a warning.
- Bilingual interface: English and Spanish.

## How to run

The whole application is one self-contained HTML file.

1. Download `index.html`.
2. Open it in a current desktop browser (Chrome, Edge or Firefox).
3. Use "Skip to simulator" to go straight to the tool, or scroll the story at the top.

No installation, server or account is needed. The only feature that uses the network is the optional DONKI load. Without a connection, the app keeps working and shows the last saved data.

## How it works

Every astronaut has a personal baseline. Readings are not judged against a general average but against that person's own normal values. The on-board logic is rule-based: thresholds and simple models decide the state and the recommended action. There is no machine learning model running on board.

The radiation model follows these steps:

1. A solar event is detected or replayed (class, time, expected particle arrival).
2. The simulator computes the time left to reach the shelter and shows a countdown.
3. Dose accumulates per astronaut with an exposure factor, and the shelter reduces it.
4. The warning level follows the dose against the career and event limits.

The heart model estimates VO2 max from the ratio between maximum and resting heart rate, classifies the recovery after exercise, and applies a gradual loss of cardiac capacity in space that exercise reduces.

## Data sources

Values taken from published sources:

- Career radiation limit of 600 mSv and limit of 250 mSv per solar particle event: NASA-STD-3001, Volume 1, Crew Health.
  https://standards.nasa.gov/sites/default/files/standards/NASA/C//nasa-std-3001-vol-1-rev-c-signature.pdf
  https://www.nasa.gov/wp-content/uploads/2023/03/radiation-protection-technical-brief-ochmo.pdf
- Dose rate in transit of about 1.84 mSv per day: Zeitlin et al., Science, 2013 (Curiosity RAD).
  https://www.swri.org/press-release/swri-led-team-calculates-radiation-exposure-associated-trip-mars
- Frequency of solar radiation storms (S3 about 10 per cycle, S4 about 3 per cycle): NOAA Space Weather Prediction Center.
  https://www.swpc.noaa.gov/noaa-scales-explanation
- Solar flare events: NASA CCMC DONKI. The API moved on 30 September 2026 to https://ccmc.gsfc.nasa.gov/DONKI-API/get/ and needs no key.
  https://ccmc.gsfc.nasa.gov/tools/DONKI/
  https://ccmc.gsfc.nasa.gov/news/major-updates/
- VO2 max estimate from heart rate ratio: Uth et al., European Journal of Applied Physiology, 2004.
  https://doi.org/10.1007/s00421-003-0988-y
- Heart rate recovery, abnormal when the drop in the first minute is 12 beats per minute or less: Cole et al., New England Journal of Medicine, 1999.
  https://pubmed.ncbi.nlm.nih.gov/10536127/
- Cardiac atrophy in bed rest and spaceflight: Perhonen et al., Journal of Applied Physiology, 2001.
  https://pubmed.ncbi.nlm.nih.gov/11457776/

## Model assumptions

These values are design decisions of the team. They are not NASA data:

- Time to reach the shelter: 5 minutes.
- Daily dose cap: 5 mSv.
- Shelter dose factor: 10 percent of the outside dose.
- Loss of cardiac capacity in space: up to 12 percent, reduced by up to 60 percent with exercise. The 12 percent is inspired by Perhonen et al. (2001), a study of four astronauts over 10 days whose result did not reach statistical significance.
- Exercise target: 65 percent of heart rate reserve.
- Cardiac output at rest: 5 L/min.
- Communication delay shown in the demo: 20 minutes.

## Limitations

- All data is simulated. The application has not been tested with real astronauts, wearable devices or flight hardware.
- It is not a medical device and must not be used for diagnosis.
- The on-board logic is rule-based and uses simplified models.
- Network messages between the vehicle and Earth (MQTT and SCADA style flows) are simulated inside the page.
- The expected impact described in the project documents is a design goal and has not been measured.

## Technology

- HTML, CSS and JavaScript in a single file, no build step needed to run it.
- sql.js (SQLite compiled to WebAssembly) for on-board storage and store-and-forward.
- GSAP and ScrollTrigger for the introductory story.
- Bricolage Grotesque typeface, embedded in the file.
- Playwright and ffmpeg for testing and for producing the presentation video.

## Use of AI tools

AI assistance (Anthropic Claude) was used during development, for code, text and video production. The team defined the idea, the requirements and the design decisions, and reviewed the results.

## License

To be defined by the team before publishing the repository.
