---
layout: post 
title: Documentation Checklist
permalink: /Doc-checklist
hide: true
show_reading_time: false
---

<div>
    <div class="grid-container">
    <a href="https://pages.opencodingsociety.com/navigation/courses/csse" class="race-button">
        <div class="btn-red">Open Coding Society Homepage</div>
    </a>
</div>

<div>
    <div class="grid-container">
    <a href="/portfolio/portfolio-checklist" class="race-button">
        <div class="btn-red">Home</div>
    </a>
</div>

<style>
.oop-section, .cs-section {
    background: rgba(255,255,255,0.05);
    border: 2px solid rgba(255,255,255,0.1);
    border-radius: 12px;
    padding: 20px 28px;
    margin: 28px 0;
}
.oop-section h2, .cs-section h2 {
    color: #3498db;
    border-bottom: 2px solid #3498db;
    padding-bottom: 8px;
    margin-top: 0;
}
.oop-section h3, .cs-section h3 {
    color: #f0c040;
    margin-top: 20px;
}
.evidence-block {
    background: rgba(0,0,0,0.3);
    border-left: 4px solid #3498db;
    border-radius: 0 8px 8px 0;
    padding: 12px 16px;
    margin: 12px 0;
    font-size: 0.95em;
}
.gamerunner-note {
    background: rgba(52, 152, 219, 0.1);
    border: 2px dashed #3498db;
    border-radius: 8px;
    padding: 12px 16px;
    margin: 16px 0;
    font-size: 0.95em;
    color: #a0d4f5;
}
pre {
    background: #1a1a2e;
    border-radius: 8px;
    padding: 14px;
    overflow-x: auto;
    font-size: 0.88em;
    color: #e8e8e8 !important;
}
code {
    font-family: 'Courier New', monospace;
    color: #e8e8e8 !important;
}

@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;700&family=Bungee&display=swap');

.grid-container {
    display: flex; 
    flex-wrap: wrap; 
    gap: 15px;
    margin-bottom: 30px;
}

.race-button {
    display: inline-block;
    text-decoration: none;
    transition: transform 0.2s, box-shadow 0.2s;
}

.race-button div {
    padding: 15px 25px;
    border-radius: 8px;
    font-weight: bold;
    text-transform: uppercase;
    font-size: 0.9rem;
    font-family: 'Bungee', cursive;
    border: 2px solid rgba(255,255,255,0.1);
    display: flex;
    align-items: center;
    justify-content: center;
    min-width: 220px;
    text-align: center;
    color: white;
}

.race-button:hover {
    transform: translateY(-5px) skew(-5deg);
}

.btn-red { background: linear-gradient(45deg, #c51d1d, #ff3131); box-shadow: 0 4px 15px rgba(197, 29, 29, 0.4); }
</style>

# 📝 Documentation Checklist

---

<div class="oop-section" markdown="1" id="code-comments">

## Code Comments

**What are code comments?** Code comments are notes you write inside your code to explain what's happening. They don't actually run — the computer ignores them — but they're really important for anyone reading your code (including yourself two weeks later). JSDoc is a specific style of comment using `/** */` that describes what a class or method does, what parameters it takes, and what it returns. The requirement is that at least 10% of your code lines are comments, meaning roughly 1 comment for every 10 lines of code.

**Requirement:** JSDoc comments for classes and methods

**Assessment Method:** Code review: Comment density >10%

### Evidence

<div class="evidence-block" markdown="1">

**TagCollisionDetector — JSDoc style comments** — This class has comments on basically every important line. The `/** */` block at the top describes the whole class, and then every parameter and step inside the methods is explained. This is what comment density over 10% actually looks like in practice.

```javascript
/**
 * TagCollisionDetector — runs every frame to check if the local player
 * is close enough to tag a remote player.
 * Extends GameObject so the engine calls update() automatically each frame.
 */
class TagCollisionDetector extends GameObject {

    /**
     * @param {Object} data - contains socket, remotePlayers, tagState, myIdRef
     * @param {Object} gameEnv - the game engine environment
     */
    constructor(data = null, gameEnv = null) {
        super(gameEnv); // call GameObject's constructor first — required before using "this"

        this.socket = data.socket;               // WebSocket connection to the server
        this.remotePlayersRef = data.remotePlayers; // reference to all connected players
        this.tagStateRef = data.tagState;        // tracks who is currently "it"
        this.myIdRef = data.myIdRef;             // my own socket ID
        this.hitRadius = 30;                     // tag range in pixels
        this.tagCooldownUntil = 0;               // timestamp: don't tag until this time passes
    }

    /**
     * Calculates the center point of a bounding box.
     * Sprites are positioned by their top-left corner, so we need this
     * to get an accurate distance measurement between two players.
     * @param {number} x - left edge of the box
     * @param {number} y - top edge of the box
     * @param {number} w - width of the box
     * @param {number} h - height of the box
     * @returns {{ cx: number, cy: number }} center coordinates
     */
    _getCenter(x, y, w, h) {
        return { cx: x + w / 2, cy: y + h / 2 }; // halfway across = center
    }

    /**
     * Called every frame by the game engine.
     * Checks if this player is "it" and if so, measures distance to every
     * remote player. If anyone is within hitRadius * 2 pixels, sends a tag.
     */
    update() {
        const myId = this.myIdRef.value;
        if (!myId || this.tagStateRef.taggerId !== myId) return; // not "it", do nothing

        const now = Date.now();
        if (now < this.tagCooldownUntil) return; // still on cooldown

        // loop through every connected player and check distance
        for (const sid in this.remotePlayersRef) {
            const rp = this.remotePlayersRef[sid];
            const local = this._getCenter(px, py, pw, ph);
            const remote = this._getCenter(rp.x, rp.y, sw, sh);

            const dx = local.cx - remote.cx; // horizontal gap
            const dy = local.cy - remote.cy; // vertical gap
            const dist = Math.sqrt(dx * dx + dy * dy); // Pythagorean distance formula

            if (dist < this.hitRadius * 2) {
                this.socket.emit("tag", { taggedId: sid }); // tell the server: tag this player
                break; // only tag one person per frame
            }
        }
    }
}
```

</div>

<div class="evidence-block" markdown="1">

**RemotePlayerVisualizer — inline comments throughout** — Every step of the draw loop is explained with a comment. Someone who has never seen this code before could read through it and understand exactly what's happening at each line.

```javascript
/**
 * RemotePlayerVisualizer — draws every other connected player on screen.
 * Runs draw() each frame to show their current position and tag status.
 */
class RemotePlayerVisualizer extends GameObject {

    constructor(data = null, gameEnv = null) {
        super(gameEnv); // sets up gameEnv, ctx, etc. from the base class

        this.remotePlayersRef = data?.remotePlayers || {}; // all players except me
        this.tagStateRef = data.tagState; // who is "it" right now
        this.myIdRef = data.myIdRef;      // my socket ID so I can skip drawing myself
        this.SCALE_FACTOR = 3.5;          // multiplier — makes sprites bigger on screen
        this.frameWidth = 569 / 13;       // sprite sheet is 569px wide with 13 frames
        this.frameHeight = 36;            // each frame is 36px tall
    }

    /**
     * Draws all remote players onto the canvas.
     * Called automatically every frame by the game engine.
     */
    draw() {
        if (!this.spriteImage || !this.remotePlayersRef) return; // nothing to draw yet

        for (const sid in this.remotePlayersRef) {
            const p = this.remotePlayersRef[sid]; // one player's position data
            const isIt = this.tagStateRef.taggerId === sid; // are they the tagger?

            const drawWidth = this.frameWidth * this.SCALE_FACTOR;   // scaled sprite width
            const drawHeight = this.frameHeight * this.SCALE_FACTOR; // scaled sprite height

            // draw the Kirby sprite at this player's position
            this.gameEnv.ctx.drawImage(
                this.spriteImage,
                0, 0,                          // crop from top-left of sprite sheet
                this.frameWidth, this.frameHeight, // crop size
                p.x, p.y,                      // where on screen to draw them
                drawWidth, drawHeight          // how big to draw them
            );

            // if they're "it", draw a red overlay so everyone can see
            if (isIt) {
                this.gameEnv.ctx.fillStyle = 'rgba(255, 0, 0, 0.4)';
                this.gameEnv.ctx.fillRect(p.x, p.y, drawWidth, drawHeight);
            }
        }
    }
}
```

</div>

</div>

---

<div class="oop-section" markdown="1" id="mini-lesson">

## Mini-Lesson Documentation

**What are mini-lessons?** Mini-lessons are blog posts or documents where you teach someone else how to do something you built. The idea is if you can explain it clearly enough for someone else to follow along, you actually understand it. Ours include embedded game demos, code walkthroughs, and step-by-step guides for building things like multiplayer connections and level transitions.

**Requirement:** Create comic/visual post with embedded runtime game demo

**Assessment Method:** Portfolio review: Mini-lesson in personal portfolio

### Evidence

<div class="evidence-block" markdown="1">

**Mini-Lessons and Blogs** — Throughout sprints 4 and 5 my partner Salma Zaghloul and I made a bunch of blogs, lessons, and ideation documents to show our process and teach others what we built.

<h2 style="font-family: sans-serif; color: #3498db; border-left: 4px solid #3498db; padding-left: 12px; text-transform: none; letter-spacing: 0;">Ideation Documents / Repo Access</h2>
<div class="grid-container">
    <a href="https://github.com/Salma-Zag/Tri2team" class="race-button">
        <div class="btn-red">Frontend Repo (for multiplayer game)</div>
    </a>
    <a href="https://github.com/TDWolff/flask" class="race-button">
        <div class="btn-red">Backend Repo (for multiplayer game)</div>
    </a>
    <a href="https://pages.opencodingsociety.com/2025/01/15/CSSE_game_over_IPYNB_2_.html" class="race-button">
        <div class="btn-red">Reference Document</div>
    </a>
    <a href="https://docs.google.com/document/d/1ZCdYbzZ6DTbXBwnawsfMCg9TeT4FzVzlmvFETTRZnL8/edit?tab=t.0" class="race-button">
        <div class="btn-red">Multiplayer Ideation Doc</div>
    </a>
</div>

<h2 style="font-family: sans-serif; color: #3498db; border-left: 4px solid #3498db; padding-left: 12px; text-transform: none; letter-spacing: 0;">Mini-Lessons / Teaching Blogs</h2>
<div class="grid-container">
    <a href="https://sprintingsnails.opencodingsociety.com/tlesson" class="race-button">
        <div class="btn-red">Transition Lesson</div>
    </a>
    <a href="https://sprintingsnails.opencodingsociety.com/mlesson" class="race-button">
        <div class="btn-red">Multiplayer Lesson</div>
    </a>
</div>

<h2 style="font-family: sans-serif; color: #3498db; border-left: 4px solid #3498db; padding-left: 12px; text-transform: none; letter-spacing: 0;">Blogs</h2>
<div class="grid-container">
    <a href="https://sprintingsnails.opencodingsociety.com/Aoneblog" class="race-button">
        <div class="btn-red">Timmy's Maze Blog</div>
    </a>
    <a href="https://sprintingsnails.opencodingsociety.com/Atwoblog" class="race-button">
        <div class="btn-red">Timmy's Maze Interaction Blog</div>
    </a>
    <a href="https://sprintingsnails.opencodingsociety.com/Athreeblog" class="race-button">
        <div class="btn-red">Timmy's Levels Gamerunners</div>
    </a>
</div>

<h2 style="font-family: sans-serif; color: #3498db; border-left: 4px solid #3498db; padding-left: 12px; text-transform: none; letter-spacing: 0;">Our Team's Website</h2>
This is where all of these live in one place.
<div class="grid-container">
    <a href="https://sprintingsnails.opencodingsociety.com/" class="race-button">
        <div class="btn-red">Homepage</div>
    </a>
</div>

</div>

</div>

---

<div class="oop-section" markdown="1" id="code-highlights">

## Code Highlights

**What are code highlights?** Code highlights are when you pick the most important or interesting parts of your project and explain them clearly — not just "here's the code" but actually walking through what each piece does and why it matters. The rubric wants to see OOP hierarchy, API calls, and collision logic specifically called out and explained.

**Requirement:** Annotate key code snippets in documentation (OOP, APIs, collision)

**Assessment Method:** Portfolio review: Highlighted code examples with explanations

### Evidence

<div class="evidence-block" markdown="1">

**OOP Highlight — The this.classes pattern** — Every single level I built uses the same core OOP pattern. I define game objects as object literals and pass them into `this.classes`. The engine then loops through and instantiates every one. This is data-driven design — instead of hardcoding objects, I describe them as data and let the engine do the work.

```javascript
// This is the pattern I used in EVERY level I made.
// Each entry is: { class: WhatToBuild, data: HowToBuildIt }
// The engine reads this list and calls new WhatToBuild(data, gameEnv) for each one.

this.classes = [
    { class: GameEnvBackground, data: bgData },      // background image
    { class: Player, data: playerData },              // Kirby — WASD controlled
    { class: Npc, data: npcData1 },                   // Garret the NPC
    ...wallClasses                                    // all 6 invisible maze walls
    // ^ the spread operator (...) unpacks an array into individual list entries
];

// The multiplayer level has more objects but the exact same pattern:
this.classes = [
    { class: Player,                 data: playerData },
    { class: NetworkSynchronizer,    data: { socket } },          // sends position to server
    { class: TagCollisionDetector,   data: { socket, remotePlayers, tagState, myIdRef } },
    { class: RemotePlayerVisualizer, data: { remotePlayers, tagState, myIdRef } },
    { class: TagHUD,                 data: { tagState, myIdRef } }
];
```

</div>

<div class="evidence-block" markdown="1">

**Collision Highlight — Tag detection using the distance formula** — This is probably the coolest piece of math in my whole project. To check if two players are touching in the multiplayer game, I use the actual Pythagorean theorem — the same formula from geometry class. I get the center point of both sprites, find the difference in x and y, then calculate the straight-line distance between them.

```javascript
// Step 1: get the center of each sprite
// (x/y is the top-left corner, so we add half the width/height to find the middle)
const local  = this._getCenter(px, py, pw, ph);      // my center point
const remote = this._getCenter(rp.x, rp.y, sw, sh);  // their center point

// Step 2: find the gap between the two centers
const dx = local.cx - remote.cx; // horizontal distance
const dy = local.cy - remote.cy; // vertical distance

// Step 3: Pythagorean theorem — a² + b² = c²
// dx² + dy² = dist²  →  dist = √(dx² + dy²)
const dist = Math.sqrt(dx * dx + dy * dy);

// Step 4: if the distance is small enough, that means they're overlapping — tag!
if (dist < this.hitRadius * 2) {
    this.socket.emit("tag", { taggedId: sid }); // send tag event to server
}
```

</div>

<div class="evidence-block" markdown="1">

**API Highlight — Saving scores to localStorage** — For the maze level leaderboard I used the browser's built-in localStorage API to save and load scores. It's not a network API but it's still API integration — I'm calling `localStorage.getItem()` and `localStorage.setItem()` to persist data between sessions. I also use `JSON.parse()` and `JSON.stringify()` to convert between strings and objects since localStorage only stores text.

```javascript
saveToLeaderboard(steps) {
    // localStorage.getItem returns a string (or null if nothing saved yet)
    // JSON.parse converts that string back into a JavaScript array
    let scores = JSON.parse(localStorage.getItem("mazeScores")) || [];

    // add this run's score with a timestamp
    scores.push({ steps: steps, date: new Date().toLocaleTimeString() });

    // sort ascending — fewer steps = better score
    scores.sort((a, b) => a.steps - b.steps);

    // trim to top 5 only
    scores = scores.slice(0, 5);

    // JSON.stringify converts the array back to a string so localStorage can store it
    localStorage.setItem("mazeScores", JSON.stringify(scores));

    // then update the display
    this.updateLeaderboardDisplay();
}
```

</div>

</div>

---

<div class="oop-section" markdown="1">

## 🎮 See It All In Action — Timmy's Fun Counter

<iframe 
    src="https://sprintingsnails.opencodingsociety.com/gamify/timmycounter.html" 
    width="100%" 
    height="500px" 
    style="border: none; border-radius: 12px; margin-top: 16px;"
    allowfullscreen>
</iframe>

<iframe 
    src="https://sprintingsnails.opencodingsociety.com/gamify/multiplayer" 
    width="100%" 
    height="500px" 
    style="border: none; border-radius: 12px; margin-top: 16px;"
    allowfullscreen>
</iframe>

</div>