---
layout: game
title: "Marienbad"
year: 1962
month:
day:
platform: "Mainframe"
developer: "Witold Podgórski"
publisher: ""
genre: "Puzzle / abstract strategy" 
updated: 2026-02-13

youtube_longplay: ""

---

## The Game
Marienbad is an early Polish mainframe puzzle game credited to Witold Podgórski, created in 1962 as a computer implementation of a misère variant of Nim (the player who takes the last object loses). 
 The title and rules trace back to a popularized “Marienbad” matchstick game described in the Polish weekly Przekrój, itself tied to the film Last Year at Marienbad. 

The original computer program was written for an Odra-series mainframe at Elwro in Wrocław, Poland, and appears to have been used as an in-house “logical duel” rather than a commercially released product. 
 The game is historically notable because it is frequently cited as among the earliest Polish computer games, though the “first Polish computer game” label is sometimes debated due to earlier Polish computing-game experiments (e.g., tic-tac-toe on the XYZ computer). 
 
## Gameplay
The rule-set comes from the “Marienbad matchstick game” popularized in Przekrój and later formalized in mathematical references:

Initial layout: four rows (heaps) of 1, 3, 5, and 7 objects (matches/counters). 
Turn rule: players alternate; on a turn you may remove any positive number of objects, but only from one row. 
Loss condition (misère): the player who makes the last move (takes the last object) loses. 
Mathematically, this specific start position is “unfair” under optimal play: the nim-value (nim-sum) is zero, so the second player can always force a win (i.e., force the first player to take the last object). 

## Modes and Features
The original matchstick form is explicitly a two-player game. 

The computer-game form is described as a logical duel implemented on Odra hardware; the most consistent interpretation is human vs computer, though documentation of an official two-human mode (computer as an arbiter) is not currently verified. 

Output and interaction (partly unverified): Academic discussion notes that recreating play on an original transistor computer “with a teleprinter” would be substantially harder than reproducing the abstract algorithm, implying at least some historically relevant I/O involved printed output rather than a display. 

## Level / Progression Structure
There is no known level or campaign structure. Each session is a single finite match that ends when the last object is taken (triggering a loss for the last mover). 

## How to win
Official win condition (per the rules): Win by leaving your opponent with no safe move so that they are forced to take the last remaining match/counter (and therefore lose). 

Practical reality at the canonical setup (1,3,5,7): If the game begins with heaps of 1, 3, 5, and 7 and both sides play optimally, the second player has a forced win. 
 This means:

If the computer is the second mover and plays optimally, the human cannot win from perfect play conditions. 
A human win would require either (a) the computer to make a mistake, or (b) a different starting configuration / turn order (not conclusively verified for the 1962 Odra implementation). 

## Development and Historical Context
Origin in film culture and popular math puzzles
A primary, contemporary Polish source ties the “Marienbad” matchstick rules to the film’s public visibility: Przekrój describes the “Marienbad game” as a matchstick puzzle associated with the film’s well-known match-playing scene, and provides an explicit 16-match, 7–5–3–1 setup with a last-move-loses condition. 
 The mathematical reference tradition later codifies this as the “Marienbad” opening position for misère Nim. 

Implementation in early Polish mainframe computing
Modern Polish computing histories place Marienbad within the Odra mainframe ecosystem. Culture.pl describes Odra 1003 as the platform on which Podgórski “installed the Marienbad game,” explicitly connecting it to the Chinese number-game tradition (Nim). 

Technical-background sources emphasize the constraints of the era. For example, a broad history of Polish informatics lists Odra 1003 parameters (including a drum memory of 8192 39-bit words and performance around 500 additions per second) and notes limited production (42 units), reinforcing how rare such machines were and why game-like uses were exceptional rather than routine. 

Documentation loss and preservation obstacles
Academic work on game preservation uses Marienbad as a case study for “dying media.” It reports that original instruction lists (program orders) have not survived and that a functioning Odra 1003 is difficult to find for faithful recreation, making the original gameplay experience hard to reconstruct even when the abstract algorithm is understood. 

A later preservation-oriented narrative records continued interest in reconstruction: it describes a 2022 run of the Marienbad variant on an Odra 1305 and states that the game was first implemented in 1962 as Odra 1003 machine code, with Podgórski later writing a C++ version. 

## Legacy and Influence
 Marienbad is regularly treated as a landmark of early Polish computer play, but its “first Polish computer game” status is not universally exclusive. Culture.pl notes an earlier Polish tic-tac-toe programming effort on the XYZ computer (described as a likely first Polish computer game), which complicates any single-title “first” claim depending on definitions and evidentiary thresholds. 

What is more stable than “first” is Marienbad’s value as a documented example of how a mathematically solvable puzzle game migrated from film-and-magazine cultural circulation into a mainframe computing context. 

In preservation discourse, Marienbad is also used to illustrate a common historical problem: early software may be culturally important yet practically unrecoverable in its authentic hardware/software configuration, pushing historians toward reconstruction efforts and careful labeling of what is original versus recreated. 


