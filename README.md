About the project:
RED DUST PROTOCOL is a Mars outpost survival game for the Build a Junior Astronaut Trainer challenge. You command four astronauts for 30 sols (Martian days) and keep six systems alive at once: oxygen, water, food, power, morale and hull strength. Every sol you assign your crew to jobs, and random hazards force hard trade-offs.

It is a single HTML file. There is nothing to install, no server and no API key, and it runs fully offline.

Why it fits the challenge
Challenge requirement	How the game meets it
Run a lunar or Martian outpost	You manage a Mars base on Jezero Crater for 30 sols
Balance competing demands	Four astronauts, six jobs. Fixing one meter always costs another
Internal state management	One state object tracks six meters, crew, sick timers, a dust-storm timer and the current hazard
Conditional logic	Threshold rules (power below 25, morale below 25, hull below 30), a dust multiplier, sick-crew limits and win/lose checks
No external API needed	All game logic is in the file. NASA photos are an optional extra and never affect play
Features
Three mission levels: Cadet, Commander and No Margin, with different resource drain and hazard frequency.
Built-in mission guide (the "Guide" button on the title screen and during play).
Synthesized sound effects: generated in the browser with the Web Audio API, so there are no audio files. A speaker button mutes everything.
Leave-mission warning: going back to the main menu ends the mission, because progress can't be resumed.

To host it, put it on any static host (GitHub Pages, Netlify, Vercel) or run a local server:

python3 -m http.server 8000
# then open http://localhost:8000/red-dust-protocol.html
How to play
Pick a level on the title screen. Cadet is the best first run.
Each sol, use the − and + buttons to assign your 4 crew to jobs.
Read the "next sol" number under each meter. A red number means that resource is shrinking.
Press END SOL. If a hazard strikes, choose how to respond.
Survive Sol 30 to win. If any meter reaches 0, the mission fails.

The full rules, strategy tips and tables are in the in-game Mission Guide.

The in-game mission guide

Rules at a glance

Meters (all drain every sol, capped at 100):

Meter	Starts at	Base drain per sol	Failure at 0
Oxygen	90	10	Suffocation
Water	90	8	Dehydration
Food	90	8	Starvation
Power	90	9	Total blackout
Morale	80	4	Crew mutiny
Hull	100	3	Hull collapse

Jobs (up to 3 crew each): Life Support +14 oxygen, Water Recycler +12 water, Greenhouse +12 food, Solar Array +14 power, Rest and Recreation +10 morale, Hull Repair +10 hull. The gain is per astronaut assigned.

Conditions: power below 25 slows every job to 60%. Morale below 25 slows it to 75%. Hull below 30 leaks an extra 4 oxygen per sol. A dust storm cuts solar output to 40%. Sick astronauts can't work.

How it works:
The whole game is one file with three parts: HTML, CSS and JavaScript. The JavaScript is organised in labelled sections:
Section (search for this comment)	What it holds
CONFIG	Level settings (DIFF), base drain (BASE_USE), the six meters (METERS) and the six jobs (TASKS)
ILLUSTRATIONS	Original SVG scenes for every hazard, the base, and the win and lose screens
IN-GAME GUIDE	The tabbed guide. Its tables read from the same settings as the game, so they stay in sync
STATE	The state object and the per-sol projection (project())
EVENTS	The 11 hazards, their choices and the weighted random draw (rollEvent())
UI	Rendering of meters, crew jobs, alerts and the log
GAME FLOW	startGame(), endSol(), hazard handling and finish()

One sol, in order:
project() works out the net change per meter: base drain, plus each job's gain, times the efficiency penalties.
endSol() applies it, counts down the dust and sickness timers and advances the day.
If any meter is at 0, the mission ends. If the sol passes 30, the player wins.
Otherwise rollEvent() may draw a hazard by weighted random choice, using the level's chance and each hazard's earliest sol. The same hazard never strikes twice in a row.
The player picks a choice. Its apply() function changes the state, and the win and loss checks run again.
Customising
Balance: edit DIFF (drain multiplier, hazard chance, win bonus), BASE_USE and each job's gain in TASKS.
Add a hazard:
Add an object to the EVENTS array with id, min (earliest sol), w (weight), icon, title, text, fact and two choices. Each choice has a label, a desc and an apply() function that changes the state.
Add a matching drawing as ART.yourId in ILLUSTRATIONS. The game needs one for every hazard.
Optionally add an entry in NASA_INFO (photo search term and NASA links), FX_TINT (screen tint) and EV_SND (sound).
NASA content and privacy
With internet, the game asks NASA's public Image and Video Library (images-api.nasa.gov) for photos matching each hazard. No account or key is needed.
If the request fails, or the "Show real NASA photos" checkbox on the title screen is off, the game uses its own drawn scenes. Gameplay is identical either way.
Each photo shows its title and credit. A "NASA Photos You've Seen" list with links to the original photos is at the bottom of the sol page and on the end screen.
The game stores one thing in your browser: whether sound is muted. It has no tracking, no cookies and no accounts.
Tech
Plain HTML, CSS and JavaScript. No framework, build step or dependencies.
Scenes are inline SVG. Sound uses the Web Audio API.
Built and tested in Chromium. It should work in current Chrome, Edge, Firefox and Safari, but those have not been tested.
Team
Team name: 404 brain not found
Members: Gitanjali,Parth,Dhruv
Hackathon: NASA SPACE APP CHALLENGE 2026

Credits and disclaimer:
This is an unofficial student project. It is not affiliated with or endorsed by NASA. Photos are courtesy of the NASA Image and Video Library and are credited where they appear. All illustrations in the game are original drawings. NASA's logo is not used, and the NASA name appears only to credit photos and facts and to label links.
