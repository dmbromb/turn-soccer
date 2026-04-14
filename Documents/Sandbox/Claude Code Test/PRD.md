# Product Requirements Document
## Freeze Kick — 3D Turn-Based Soccer Game

**Version:** 1.0  
**Date:** April 14, 2026  
**Author:** David Bromberg  
**Status:** In Development

---

## 1. Overview

### 1.1 Problem Statement
Traditional soccer video games require fast reflexes and real-time inputs, creating a high barrier to entry for casual players or those who want to think strategically. There is no accessible, browser-based soccer game that combines 3D visuals with turn-based tactical decision-making.

### 1.2 Product Vision
Freeze Kick is a single-file, browser-based 3D soccer game where time stops every turn and the player makes deliberate tactical choices — dribble, pass, or shoot — before watching the action play out. It captures the strategy of soccer without the twitch-speed execution.

### 1.3 Goals
- Deliver a playable, self-contained soccer experience that runs in any modern browser with no installation
- Give players meaningful decisions each turn through a clear action menu
- Provide three escalating challenge levels with AI opponents of increasing difficulty
- Keep the experience short and replayable (each level completable in under 5 minutes)

---

## 2. Target Audience

| Segment | Description |
|---------|-------------|
| Casual gamers | Players who enjoy sports games but are put off by the skill ceiling of real-time titles |
| Strategy game fans | Players who enjoy turn-based games (chess, XCOM, Fire Emblem) and want a sports variant |
| Soccer fans | Fans of the sport who want a low-friction digital experience |
| Students / office players | Anyone who wants a quick, browser-based game with no download |

---

## 3. User Stories

| # | As a… | I want to… | So that… |
|---|-------|-----------|----------|
| 1 | Player | See a clear main menu when I open the game | I know how to start |
| 2 | Player | Choose from 3 difficulty levels | I can pick a challenge appropriate to my skill |
| 3 | Player | Pick Dribble, Pass, or Shoot each turn | I have meaningful tactical options |
| 4 | Player | Choose a direction for every action | I can control where the ball goes |
| 5 | Player | Choose potency when passing | I can control how far the ball travels |
| 6 | Player | Choose a shot type when shooting | I have variety in how I try to score |
| 7 | Player | Watch an animation after I confirm my action | I can see the result play out in 3D |
| 8 | Player | See AI opponents react to my moves | The game feels dynamic and challenging |
| 9 | Player | Know when I've scored, been tackled, or run out of turns | I always understand my game state |
| 10 | Player | Progress to the next level after winning | There is a sense of advancement |
| 11 | Player | Retry a level immediately after losing | I can keep trying without friction |
| 12 | Player | Return to the main menu at any time | I can navigate the game freely |

---

## 4. Functional Requirements

### 4.1 Menu & Navigation

| ID | Requirement | Priority |
|----|-------------|----------|
| F-01 | The game must display a main menu screen on load with a title and Play button | Must |
| F-02 | The level select screen must show 3 playable levels with name, description, and turn limit | Must |
| F-03 | A WIN screen must appear when the player scores, with options to advance or select a level | Must |
| F-04 | A LOSE screen must appear on tackle or turn limit, with options to retry or select a level | Must |
| F-05 | All screens must be reachable without reloading the page | Must |

### 4.2 Gameplay — Turn System

| ID | Requirement | Priority |
|----|-------------|----------|
| F-06 | Each turn the match must freeze and the player must be prompted to choose an action | Must |
| F-07 | Input must be locked during the animation phase (RESOLVING state) | Must |
| F-08 | The turn counter must increment after each resolved turn and be visible in the HUD | Must |
| F-09 | Reaching the turn limit without scoring must trigger the LOSE screen | Must |

### 4.3 Player Actions

| ID | Requirement | Priority |
|----|-------------|----------|
| F-10 | DRIBBLE must move the ball carrier in one of 8 compass directions | Must |
| F-11 | PASS must send the ball in a chosen direction with Low / Medium / High potency | Must |
| F-12 | PASS must transfer ball control to the nearest teammate in the chosen direction | Must |
| F-13 | SHOOT must send the ball toward the AI goal with a chosen shot type (Curled / Power / Lob) | Must |
| F-14 | Each shot type must produce a visually distinct ball arc | Should |
| F-15 | The Confirm button must be disabled until all required sub-options are selected | Must |

### 4.4 AI Behavior

| ID | Requirement | Priority |
|----|-------------|----------|
| F-16 | The Goalkeeper must reposition toward the ball's X position each turn | Must |
| F-17 | The Goalkeeper must perform a dive animation when the player shoots | Should |
| F-18 | Defenders must move toward the ball carrier each turn | Must |
| F-19 | Advanced Defenders (Level 3) must attempt to predict and intercept the carrier's path | Should |
| F-20 | A tackle must trigger if any AI Defender is within 1.2 units of the ball after a turn resolves | Must |

### 4.5 Win / Lose Conditions

| ID | Requirement | Priority |
|----|-------------|----------|
| F-21 | A goal is scored when the ball crosses Z ≤ −24 within the goal width (7.32 units) | Must |
| F-22 | The player loses if tackled (AI within 1.2 units of ball carrier) | Must |
| F-23 | The player loses if the turn limit is exhausted without scoring | Must |

### 4.6 Levels

| ID | Level | Setup | Turn Limit |
|----|-------|-------|------------|
| L-01 | 1v1 Breakaway | 1 attacker vs 1 Goalkeeper | 10 |
| L-02 | 2v2 Build-Up | 2 attackers vs 1 Defender + 1 Goalkeeper | 15 |
| L-03 | 5v5 Full Attack | 5 attackers vs 4 Defenders + 1 Goalkeeper | 20 |

---

## 5. Non-Functional Requirements

| ID | Requirement | Detail |
|----|-------------|--------|
| NF-01 | Single file | The entire game must run from one `soccer.html` file with no external assets or server |
| NF-02 | No installation | Must run by opening the file in any modern browser (Chrome, Firefox, Edge, Safari) |
| NF-03 | No build tools | No npm, webpack, or bundler — Three.js loaded via CDN only |
| NF-04 | Performance | Must maintain 30+ fps on a standard laptop during animation phases |
| NF-05 | Responsiveness | Must render correctly at common desktop resolutions (1280×720 and above) |
| NF-06 | Turn animation | Each turn animation must complete within ~700ms to keep gameplay pace snappy |

---

## 6. Visual & UX Requirements

| ID | Requirement |
|----|-------------|
| V-01 | 3D low-poly / flat-shaded visual style |
| V-02 | Green pitch with white line markings and two goals |
| V-03 | Player team: blue; AI team: red. Goalkeeper visually distinguished (yellow gloves) |
| V-04 | Ball: white sphere with visible spin during animation |
| V-05 | Camera follows the ball carrier in third-person view |
| V-06 | Sky is light blue with fog for depth |
| V-07 | HUD shows current level, turn count, and turn limit at all times during gameplay |
| V-08 | Action panel is always visible at the bottom of the screen during the PLAYING state |

---

## 7. Out of Scope (v1.0)

- Multiplayer (local or online)
- Custom team or player names
- Sound effects or music
- Mobile / touch support
- Persistent save state or high scores
- Player-controlled goalkeeping
- More than 3 levels
- Difficulty settings within a level

---

## 8. Success Metrics

| Metric | Target |
|--------|--------|
| All 3 levels completable without bugs | 100% of the CLAUDE.md test checklist passes |
| Level 1 completable by a first-time player | Player scores within 10 turns on first attempt |
| No page reloads required | Full menu → play → win/lose → retry flow works without reload |
| Animation performance | No frame drops below 30fps during turn animations on a mid-range laptop |

---

## 9. Dependencies

| Dependency | Version | Source |
|------------|---------|--------|
| Three.js | r128 | `https://cdn.jsdelivr.net/npm/three@0.128.0/build/three.min.js` |
| Modern browser | Chrome 90+, Firefox 88+, Edge 90+, Safari 14+ | User's machine |

---

## 10. Open Questions

| # | Question | Owner |
|---|----------|-------|
| 1 | Should Level 3 winning return the player to the menu or show a special completion screen? | David Bromberg |
| 2 | Should the GK save count as a LOSE (block) or should the ball just rebound for a new turn? | David Bromberg |
| 3 | Is mobile/touch support in scope for a future version? | David Bromberg |
