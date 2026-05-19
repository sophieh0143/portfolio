---
layout: post 
title: Documentation Checklist
permalink: /old-Doc-checklist
hide: true
show_reading_time: false
---

<style>
.oop-section {
    background: rgba(255,255,255,0.05);
    border: 2px solid rgba(255,255,255,0.1);
    border-radius: 12px;
    padding: 20px 28px;
    margin: 28px 0;
}
.oop-section h2 {
    color: #3498db;
    border-bottom: 2px solid #3498db;
    padding-bottom: 8px;
    margin-top: 0;
}
.oop-section h3 {
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
}
code {
    font-family: 'Courier New', monospace;
}

    @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;700&family=Bungee&display=swap');

    /* Global Page Overrides */
    .post {
        background-color: #0f0f0f;
        color: #f0f0f0;
        font-family: 'Orbitron', sans-serif;
        padding: 40px 20px;
        min-height: 100vh;
    }

    /* Styling for the intro text */
    .dashboard-intro {
        font-family: 'Orbitron', sans-serif;
        font-weight: 500;
        font-size: 1rem;
        color: #bbb;
        margin-bottom: 30px;
        letter-spacing: 0.5px;
        line-height: 1.8;
        border-bottom: 1px solid #333;
        padding-bottom: 25px;
    }

    .system-label {
        color: #00d4ff;
        font-weight: 700;
        text-transform: uppercase;
        display: block;
        margin-bottom: 10px;
        font-size: 1.2rem;
    }

    /* Level Highlight Tags */
    .level-tag {
        color: #ff0000;
        background: rgba(255, 0, 0, 0.1);
        padding: 2px 8px;
        border-radius: 4px;
        font-family: 'Bungee', cursive;
        font-size: 0.8rem;
    }

    h2 {
        font-family: 'Bungee', cursive;
        color: #ff0000;
        text-transform: uppercase;
        letter-spacing: 2px;
        border-left: 5px solid #ff0000;
        padding-left: 15px;
        margin-top: 40px;
    }

    /* Container for the button grid */
    .grid-container {
        display: flex; 
        flex-wrap: wrap; 
        gap: 15px;
        margin-bottom: 30px;
    }

    /* The Racing Button Style */
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

    /* Hover Effects */
    .race-button:hover {
        transform: translateY(-5px) skew(-5deg);
    }

    /* Color Variants */
    .btn-red { background: linear-gradient(45deg, #c51d1d, #ff3131); box-shadow: 0 4px 15px rgba(197, 29, 29, 0.4); }
    .btn-orange { background: linear-gradient(45deg, #a54c1f, #e67e22); box-shadow: 0 4px 15px rgba(165, 76, 31, 0.4); }
    .btn-gold { background: linear-gradient(45deg, #a58f1f, #f1c40f); box-shadow: 0 4px 15px rgba(165, 143, 31, 0.4); }
    .btn-dark { background: linear-gradient(45deg, #4a0b0b, #220505); box-shadow: 0 4px 15px rgba(0, 0, 0, 0.6); border: 1px solid #ff0000 !important;}
</style>

# 🏗️ Documentation Checklist

---

<div class="oop-section" markdown="1" id="code-comments">

## Code Comments

**What are code comments?** 

**Requirement:** JSDoc comments for classes and methods

**Assessment Method:** Code review: Comment density >10%

### Evidence

<div class="evidence-block" markdown="1">

**GameLevelTimmyfuncounter** — This is my invisible maze level. The whole level is wrapped inside one class called `GameLevelTimmyfuncounter`. When the game engine starts this level, it calls the `constructor` — which is basically the setup function that runs once at the beginning. Inside there I define what the background looks like, where Kirby spawns, where the NPC Garret goes, and where all the invisible walls are placed. At the end I hand all of that to `this.classes` which is the list the game engine reads to actually build everything on screen.

```javascript
class GameLevelTimmyfuncounter {
    constructor(gameEnv) {
        // gameEnv is the game engine passing itself in so we can use things like
        // the screen width/height and the file path to find images
        this.gameEnv = gameEnv;
        const path = gameEnv.path; // the base URL path to find image files

        // this.classes is the list the engine reads to build the level
        // each item is: what class to use + what data to give it
        this.classes = [
            { class: GameEnvBackground, data: bgData }, // builds the background image
            { class: Player, data: playerData },         // builds Kirby with all his settings
            { class: Npc, data: npcData1 },              // builds Garret the NPC
            ...wallClasses                               // builds all the invisible maze walls
        ];
    }
}
export default GameLevelTimmyfuncounter; // makes this class available to other files
```

</div>

<div class="evidence-block" markdown="1">

**GameLevelBattle** — This is my boss fight level. Same structure as above — one class wrapping the whole level. The difference is instead of an NPC, I pass in an `Enemy` class. I also built a whole damage system inside here — health bars for both Kirby and the enemy, a projectile that Kirby can shoot with the spacebar, and logic that checks if anything is hitting anything else.

```javascript
class GameLevelBattle {
    constructor(gameEnv) {
        // playerData and enemyData are objects I defined above with all the
        // sprite settings, starting positions, hitbox sizes, etc.
        this.classes = [
            { class: GameEnvBackground, data: bgData }, // the hell background image
            { class: Player, data: playerData },         // Kirby, controlled by WASD
            { class: Enemy, data: enemyData }            // the enemy, chases Kirby
        ];
        // after this I override the enemy's update() to add my own damage logic
    }
}
export default GameLevelBattle;
```

</div>

---

<div class="documentation-section" markdown="1" id="mini-lesson">

## Mini-Lesson Documentation

**What are mini-lessons (or blogs)?**

**Requirement:** Create comic/visual post with embedded runtime game demo

**Assessment Method:** Portfolio review: Mini-lesson in personal portfolio

### Evidence

<div class="evidence-block" markdown="1">

**Mini-Lessons!** — While working on both sprint 4 and 5, out team make countless blogs, lessons, and ideation documents in order to show our processes. All of these are mini-lessons, documents that can be shown to others to help them achive similar things we have. Here are a few examples of ideation documents, lessons, and blogs I have made (with my partner: Salma Zaghloul).

<h2>Ideation Documents/Repo Access</h2>
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

<h2>Mini-Lessons/Teaching Blogs</h2>
<div class="grid-container">
    <a href="https://sprintingsnails.opencodingsociety.com/tlesson" class="race-button">
        <div class="btn-red">Transition Lesson</div>
    </a>
    <a href="https://sprintingsnails.opencodingsociety.com/mlesson" class="race-button">
        <div class="btn-red">Multiplayer Lesson</div>
    </a>
</div>

<h2>Blogs</h2>
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

<h2>Extra: Our team's website!</h2>
This is where ALL of these can be found in one place.
<div class="grid-container">
    <a href="https://sprintingsnails.opencodingsociety.com/" class="race-button">
        <div class="btn-red">Homepage</div>
    </a>
</div>

</div>

---

<div class="Documentation-section" markdown="1" id="code-highlights">

## Code Highlights

**What are coding highlights?** 

**Requirement:** Annotate key code snippets in documentation (OOP, APIs, collision)

**Assessment Method:** Portfolio review: Highlighted code examples with explanations

### Evidence

<div class="evidence-block" markdown="1">

Every level I made uses the same pattern. I write each game object as a JavaScript **object literal** — which is just a set of curly braces `{}` with key-value pairs inside describing all the settings. Then I pass it into `this.classes` as a pair: the class to use, and the data to give it. The game engine loops through that list and instantiates every single one.

```javascript
// From GameLevelTimmyfuncounter.js
this.classes = [
    // each { class, data } pair = one instantiation
    { class: GameEnvBackground, data: bgData },   // engine creates a background object
    { class: Player, data: playerData },           // engine creates Kirby with my settings
    { class: Npc, data: npcData1 },                // engine creates Garret the NPC
    ...wallClasses                                 // engine creates all 6 invisible walls
    // the ... spread operator unpacks the wallClasses array into individual entries
];
```

```javascript
// From GameLevelMultiplayer.js — same exact pattern, just more objects
this.classes = [
    { class: GameEnvBackground,      data: bgData },
    { class: SplineBarrier,          data: arenaBarrierData },   // the curved arena wall
    { class: Player,                 data: playerData },          // my Kirby
    { class: NetworkSynchronizer,    data: { socket } },          // sends my position to server every 50ms
    { class: TagCollisionDetector,   data: { socket, remotePlayers, tagState, myIdRef } }, // tag detection
    { class: RemotePlayerVisualizer, data: { remotePlayers, tagState, myIdRef } }, // draws other players
    { class: TagHUD,                 data: { tagState, myIdRef } } // draws the "YOU ARE IT" banner
];
```
</div>

---

<div class="oop-section" markdown="1">

## 🎮 See It All In Action — Timmy's Fun Counter

This one level demonstrates every single OOP concept on this page at the same time. Here's what to look for when you play it:

<iframe 
    src="https://sprintingsnails.opencodingsociety.com/gamify/timmycounter.html" 
    width="100%" 
    height="500px" 
    style="border: none; border-radius: 12px; margin-top: 16px;"
    allowfullscreen>
</iframe>

</div>