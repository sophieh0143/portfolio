---
layout: post 
title: Object Oriented Programming Checklist
permalink: /OOP-checklist
hide: true
show_reading_time: false
---

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
.cs-section h2 {
    color: #2ecc71;
    border-bottom: 2px solid #2ecc71;
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
.cs-section .evidence-block {
    border-left: 4px solid #2ecc71;
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
    color: #b82323 !important;
}
code {
    font-family: 'Courier New', monospace;
    color: #e8e8e8 !important;
}
</style>

# 🏗️ Object-Oriented Programming Checklist

---

<div class="oop-section" markdown="1" id="writing-classes">

## Writing Classes

**What is a class?** A class is basically a blueprint for making objects. Instead of copy-pasting the same code a hundred times, you write it once in a class and then the game engine just uses that blueprint whenever it needs one. I like to think of it like a cookie cutter — you make the shape once and stamp out as many cookies as you want. In this game engine, literally everything you see on screen — the background, Kirby, the NPC, the walls — all came from a class.

**Requirement:** Create minimum 2 custom character classes extending base classes (Player.js, NPC.js, Enemy.js)

**Assessment Method:** Code review — class definitions using `extends` keyword

### Evidence

<div class="evidence-block" markdown="1">

**GameLevelTimmyfuncounter** — This is my invisible maze level. The whole thing is wrapped inside one class called `GameLevelTimmyfuncounter`. When the game engine loads this level, it runs the `constructor` first — that's basically the setup function that only runs once at the very beginning. Inside there I set up the background, where Kirby spawns, where the NPC Garret goes, and where all the invisible walls are. Then I hand everything to `this.classes`, which is the list the engine reads to actually put things on screen.

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

**GameLevelBattle** — This is my boss fight level. Same idea as above — one class wraps the whole level. The difference is instead of an NPC, I bring in an `Enemy` class. I also built a whole damage system inside here — health bars for both Kirby and the enemy, a projectile Kirby can shoot with spacebar, and logic that checks if anything is hitting anything else.

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

<div class="evidence-block" markdown="1">

**RemotePlayerVisualizer** — This is one of four custom classes I wrote from scratch for the multiplayer tag game. Its whole job is drawing the other players on screen. Every frame it loops through everyone connected, draws a Kirby sprite at their position, and if someone is "it" it slaps a red overlay on them plus a floating "IT" label above their head. It extends `GameObject` directly — meaning I'm not using a pre-built character template, I'm making my own thing on top of the engine's base class.

```javascript
class RemotePlayerVisualizer extends GameObject {
    // extends GameObject means this class inherits all the basic
    // game object stuff (like being tracked by the engine each frame)
    constructor(data = null, gameEnv = null) {
        super(gameEnv); // calls GameObject's constructor first to set up the basics
        this.remotePlayersRef = data?.remotePlayers || {}; // reference to the list of other players
        this.tagStateRef = data.tagState; // tracks who is currently "it"
        this.myIdRef = data.myIdRef;      // my own socket ID so I know which player is me
        this.SCALE_FACTOR = 3.5;          // how big to draw the sprites
    }

    draw() {
        // this runs every single frame to redraw all remote players
        for (const sid in this.remotePlayersRef) {
            const p = this.remotePlayersRef[sid]; // p = one remote player's x/y position
            const isIt = this.tagStateRef.taggerId === sid; // is this player "it"?

            // draws the kirby sprite at their current position
            // if isIt is true, also draws a red box over them and "IT" text above
        }
    }
}
```

</div>

<div class="evidence-block" markdown="1">

**TagCollisionDetector** — Another custom class I wrote for the multiplayer game. This one runs quietly in the background every single frame. Its job is just to ask: am I close enough to tag someone? It uses the distance formula from math class — square root of dx squared plus dy squared — to check how far away every other player is. If I'm "it" and someone's within 30 pixels, it fires a "tag" message to the server.

```javascript
class TagCollisionDetector extends GameObject {
    constructor(data = null, gameEnv = null) {
        super(gameEnv); // sets up the base game object
        this.socket = data.socket;               // the connection to the server
        this.remotePlayersRef = data.remotePlayers; // list of other players
        this.tagStateRef = data.tagState;        // who is currently "it"
        this.hitRadius = 30;                     // how close = close enough to tag (in pixels)
    }

    update() {
        // this runs every frame
        // first checks: am I "it"? if not, do nothing
        // then calculates distance between me and each other player
        // if distance < hitRadius * 2, we're touching — send the tag!
        if (dist < this.hitRadius * 2) {
            this.socket.emit("tag", { taggedId: sid }); // tells the server to tag that player
        }
    }
}
```

</div>

</div>

---

<div class="oop-section" markdown="1" id="methods-params">

## Methods & Parameters

**What are methods and parameters?** A method is just a function that lives inside a class — it's what the class knows how to *do*. Parameters are the inputs you pass into that method so it can actually do its job. Like if you had a `makeSandwich(bread, filling)` method, `bread` and `filling` are the parameters. In game code, methods handle stuff like checking if two things are touching, updating the score, or moving sprites around the screen.

**Requirement:** Implement methods with parameters and return values (e.g., `collisionHandler(other, direction)`)

**Assessment Method:** Code review — method signatures with 2+ parameters

### Evidence

<div class="evidence-block" markdown="1">

**`isColliding(player, popcorn)`** — This method lives inside `GameLevelHooray`. It takes two parameters — `player` (Kirby) and `popcorn` (the collectible). It uses `getBoundingClientRect()` to get the exact pixel box of each one on screen, then checks if those boxes overlap. If they do, it returns `true`. If not, `false`. That answer is what the collision loop uses to decide whether to collect the popcorn.

```javascript
isColliding(player, popcorn) {
    // getBoundingClientRect() returns an object like:
    // { top: 100, bottom: 150, left: 200, right: 250 }
    // basically the box around the sprite on screen
    const rect1 = player.canvas.getBoundingClientRect();
    const rect2 = popcorn.canvas.getBoundingClientRect();

    // this checks all four sides — if ANY side means they can't be touching,
    // the ! at the front flips it, so we return true only when they ARE overlapping
    return !(rect1.right < rect2.left ||   // kirby is fully to the left
             rect1.left > rect2.right ||   // kirby is fully to the right
             rect1.bottom < rect2.top ||   // kirby is fully above
             rect1.top > rect2.bottom);    // kirby is fully below
}
```

</div>

<div class="evidence-block" markdown="1">

**`collectPopcorn(popcorn)`** — This runs every time `isColliding` comes back true. It takes the `popcorn` object as a parameter so it can move it. It bumps the counter up, updates the number shown on screen, then picks a completely random new spot and teleports the popcorn there. Once you've grabbed all 10, it just hides the popcorn instead of respawning it.

```javascript
collectPopcorn(popcorn) {
    this.popcornCount++; // add 1 to the count
    document.getElementById('popcorn-value').innerText = this.popcornCount; // update the UI

    if (this.popcornCount >= 10) {
        // all 10 collected — hide the popcorn sprite
        popcorn.canvas.style.display = "none";
    } else {
        // pick a random x between 75 and (screen width - 75)
        const newX = Math.random() * (window.innerWidth - 150) + 75;
        // pick a random y between 75 and (screen height - 75)
        const newY = Math.random() * (window.innerHeight - 150) + 75;
        // move the popcorn to the new spot
        popcorn.x = newX;
        popcorn.y = newY;
    }
}
```

</div>

<div class="evidence-block" markdown="1">

**`_getCenter(x, y, w, h)`** — This is inside `TagCollisionDetector`. It takes four parameters — x position, y position, width, and height — and returns the center point of that box. I need this because a sprite's x/y is its top-left corner, not the middle, so I have to calculate the center before measuring distance between two players.

```javascript
_getCenter(x, y, w, h) {
    // x + w/2 = halfway across the width = center x
    // y + h/2 = halfway down the height = center y
    return { cx: x + w / 2, cy: y + h / 2 };
}

// used like this:
const local = this._getCenter(px, py, pw, ph);   // my center
const remote = this._getCenter(rp.x, rp.y, sw, sh); // their center
const dx = local.cx - remote.cx; // horizontal distance
const dy = local.cy - remote.cy; // vertical distance
const dist = Math.sqrt(dx * dx + dy * dy); // actual distance (Pythagorean theorem!)
```

</div>

<div class="evidence-block" markdown="1">

**`saveToLeaderboard(steps)`** — This is in `GameLevelTimmyfuncounter`. When you finish the maze it gets called with however many steps you took. It pulls the existing scores out of `localStorage` (basically the browser's built-in save file), adds your new score, sorts everyone from fewest to most steps, keeps only the top 5, then saves it all back.

```javascript
saveToLeaderboard(steps) {
    // localStorage stores data as a string, so JSON.parse converts it back to an array
    // if nothing is saved yet, use an empty array instead
    let scores = JSON.parse(localStorage.getItem("mazeScores")) || [];

    // add the new score with a timestamp
    scores.push({ steps: steps, date: new Date().toLocaleTimeString() });

    // sort from lowest to highest steps (a.steps - b.steps = ascending order)
    scores.sort((a, b) => a.steps - b.steps);

    // only keep the top 5
    scores = scores.slice(0, 5);

    // JSON.stringify converts the array back to a string so localStorage can save it
    localStorage.setItem("mazeScores", JSON.stringify(scores));
}
```

</div>

</div>

---

<div class="oop-section" markdown="1" id="instantiation">

## Instantiation & Objects

**What is instantiation?** Instantiation is when you take a class blueprint and actually create a real working object from it. Like, `Player` is just a description — instantiation is the moment the engine goes "okay, make me one of those" and the actual Kirby appears on screen. In this engine, instantiation happens through `this.classes` — every item in that list gets turned into a real object when the level loads.

**Requirement:** Instantiate game objects in GameLevel configuration using Object Literals

**Assessment Method:** Code review — GameLevel setup objects

### Evidence

<div class="evidence-block" markdown="1">

Every level I made uses the same pattern. I write each game object as a JavaScript **object literal** — curly braces `{}` with key-value pairs describing all its settings. Then I pass it into `this.classes` as a pair: the class to use, and the data to give it. The engine loops through that list and creates every single one.

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

</div>

---

<div class="oop-section" markdown="1" id="inheritance">

## Inheritance (Basic)

**What is inheritance?** Inheritance is when one class automatically gets all the features of another class just by using the `extends` keyword. It's like how a golden retriever is still a dog — it has all the normal dog stuff plus its own golden retriever things on top. In this engine, `Player` extends `Character`, which extends `GameObject`. So `Player` gets everything both of those have, and then adds its own keyboard controls on top. I actually wrote four new classes that extend `GameObject` myself for the multiplayer level.

**Requirement:** Create class hierarchy with 2+ levels (e.g., `GameObject → Character → Player`)

**Assessment Method:** Code review — `extends` keyword, inheritance chain

### Evidence

<div class="evidence-block" markdown="1">

The game engine already has a whole family of classes that build on each other. In my multiplayer level I added four new branches to that family tree myself.

```
GameObject  ← the base class everything inherits from
  └── Character  ← adds movement, sprites, animation
        ├── Player      ← adds keyboard controls; used in all my levels
        ├── Npc         ← adds dialogue; used in Timmyfuncounter, Garett, Hooray
        └── Enemy       ← adds AI behavior; used in GameLevelBattle
  └── RemotePlayerVisualizer  ← MY custom class, extends GameObject directly
  └── TagCollisionDetector    ← MY custom class, extends GameObject directly
  └── TagHUD                  ← MY custom class, extends GameObject directly
  └── NetworkSynchronizer     ← MY custom class, extends GameObject directly
```

```javascript
// The extends keyword is what creates the inheritance relationship
// RemotePlayerVisualizer gets everything GameObject has for free
// then adds its own draw() logic on top
class RemotePlayerVisualizer extends GameObject { ... }
class TagHUD extends GameObject { ... }
class TagCollisionDetector extends GameObject { ... }
class NetworkSynchronizer extends GameObject { ... }

// and when I import Player and Enemy I'm using the existing 2-level chain:
import Player from './essentials/Player.js'; // Player extends Character extends GameObject
import Enemy from './essentials/Enemy.js';   // Enemy extends Character extends GameObject
```

</div>

</div>

---

<div class="oop-section" markdown="1" id="overriding">

## Method Overriding

**What is method overriding?** When a class inherits from a parent, it gets all the parent's methods automatically. But sometimes you need that method to do something different in your specific class. So you write your own version with the same name — that replaces the parent's version. It's like if the base `Character` class had a generic `update()` that just moves the character, but you override it in `Enemy` to also make it chase the player around.

**Requirement:** Override parent methods (`update()`, `draw()`, `handleCollision()`)

**Assessment Method:** Code review — polymorphic implementations

### Evidence

<div class="evidence-block" markdown="1">

In my battle level, I grabbed the enemy's built-in `update()` and replaced it with my own version at runtime. I still call the original first so the enemy keeps chasing Kirby normally, but then I add my own stuff on top — checking if they're overlapping, dealing damage with a cooldown so you don't lose all your HP in one second, and triggering game over if health hits zero.

```javascript
// save the original update so we can still call it
const originalUpdate = enemy.update?.bind(enemy);

// now replace update with our own function
enemy.update = function () {
    if (originalUpdate) originalUpdate(); // step 1: run the original (keeps enemy moving)

    // step 2: check if Kirby's position overlaps with the enemy's position
    // p = player position, e = enemy position
    const touching =
        p.x < e.x + 100 &&  // kirby's left edge is before enemy's right edge
        p.x + 50 > e.x &&   // kirby's right edge is past enemy's left edge
        p.y < e.y + 100 &&  // kirby's top is above enemy's bottom
        p.y + 50 > e.y;     // kirby's bottom is below enemy's top
        // all four = true means they're overlapping

    if (touching) {
        const now = Date.now(); // current time in milliseconds
        // only deal damage if it's been 500ms since the last hit (the cooldown)
        if (now - enemy.data.lastHit > 500) {
            enemy.data.lastHit = now;      // record the time of this hit
            player.data.health -= 10;      // take 10 HP from the player
            updateHealthBar(player.data.health); // update the health bar on screen
        }
    }
};
```

</div>

<div class="evidence-block" markdown="1">

In my multiplayer level, every custom class I wrote overrides both `update()` and `draw()` from `GameObject`. `TagHUD` is a good example — it overrides `draw()` to paint a pulsing red border around the whole screen when you're "it", plus a "YOU ARE IT" banner at the top. The `Math.sin()` is what makes it pulse in and out smoothly.

```javascript
class TagHUD extends GameObject {

    // overrides draw() — runs every frame to repaint the HUD
    draw() {
        const isIt = this.tagStateRef.taggerId === this.myIdRef.value; // am I "it"?
        const ctx = this.gameEnv.ctx; // the canvas drawing context
        const now = Date.now();

        if (isIt) {
            // Math.sin oscillates between -1 and 1 over time
            // 0.5 + 0.5 * that = oscillates between 0 and 1 = pulsing effect
            const pulse = 0.5 + 0.5 * Math.sin(now / 300);
            ctx.strokeStyle = `rgba(255, 30, 30, ${0.4 + 0.5 * pulse})`; // red, pulsing opacity
            ctx.lineWidth = 18;
            ctx.strokeRect(0, 0, W, H); // draws the red border around the whole screen
            // also draws the "YOU ARE IT" text banner at the top
        }
    }

    // overrides update() — runs every frame to check if tag status changed
    update() {
        const isIt = this.tagStateRef.taggerId === this.myIdRef.value;
        if (isIt && !this._wasIt) {
            this._flashStart = Date.now(); // just became "it" — start the flash effect
        }
        this._wasIt = isIt; // remember for next frame
        this.draw();        // trigger the draw
    }
}
```

</div>

</div>

---

<div class="oop-section" markdown="1" id="super-calls">

## Constructor Chaining

**What is constructor chaining?** Every class has a `constructor` — the setup function that runs when an object gets created. When you use `extends`, your class has a parent with its own constructor too. `super()` is how you call the parent's constructor before doing your own setup. You literally have to call `super()` first or JavaScript throws an error — you can't use `this` until the parent has set up its stuff. I think of it like making sure the foundation of a house is solid before you start putting up walls.

**Requirement:** Use `super()` to chain constructors

**Assessment Method:** Code review — `super(data, gameEnv)` calls

### Evidence

<div class="evidence-block" markdown="1">

Every custom class I wrote for the multiplayer level calls `super(gameEnv)` as the very first line. This runs `GameObject`'s constructor first, which sets up `this.gameEnv` and registers the object with the engine. Only then can I safely start setting my own properties.

```javascript
class RemotePlayerVisualizer extends GameObject {
    constructor(data = null, gameEnv = null) {
        super(gameEnv); // ← MUST be first! calls GameObject's constructor
                        // GameObject sets up this.gameEnv, this.gameEnv.ctx, etc.

        // now I can safely use "this" to set my own stuff
        this.remotePlayersRef = data?.remotePlayers || {}; // the ?. means "if data exists"
        this.tagStateRef = data.tagState;
        this.myIdRef = data.myIdRef;
    }
}

class TagCollisionDetector extends GameObject {
    constructor(data = null, gameEnv = null) {
        super(gameEnv); // same deal — GameObject constructor runs first

        // then my setup
        this.socket = data.socket;
        this.hitRadius = 30;
    }
}
```

For the regular game levels, the engine handles the chaining automatically. When it reads `{ class: Player, data: playerData }` from `this.classes`, it calls `new Player(playerData, gameEnv)` which triggers the whole chain:

```javascript
// What happens behind the scenes when the engine builds Kirby:

// 1. engine calls: new Player(playerData, gameEnv)
// 2. inside Player's constructor: super(playerData, gameEnv)
//    → goes up to Character's constructor
// 3. inside Character's constructor: super(playerData, gameEnv)
//    → goes up to GameObject's constructor
// 4. GameObject sets up the basics (canvas, position, etc.)
// 5. control returns to Character (adds animation, movement)
// 6. control returns to Player (adds keyboard input)
// 7. Kirby is fully built and ready to go!

this.classes = [
    { class: Player, data: playerData }, // triggers the full chain above
    { class: Npc, data: npcData1 },      // same chain for Garret
    { class: Enemy, data: enemyData }    // same chain for the enemy
];
```

</div>

</div>

---

<div class="oop-section" markdown="1">

## 🎮 See It All In Action — Timmy's Fun Counter

This one level shows every single OOP concept on this page at the same time. Here's what to look for when you play it:

**Writing Classes** — The whole level you're playing right now is wrapped inside `GameLevelTimmyfuncounter`, a class I wrote myself. Everything on screen came from that class's constructor.

**Methods & Parameters** — The step counter at the bottom is powered by a keydown method that takes the key event as a parameter and updates the count. The leaderboard button calls `saveToLeaderboard(steps)` with your step count when you win.

**Instantiation & Objects** — Every single thing you see — the background, Kirby, Garret the NPC, all the invisible walls — was instantiated from the `this.classes` list when the level loaded.

**Inheritance** — Kirby is a `Player`, which extends `Character`, which extends `GameObject`. Garret is an `Npc`, which also extends `Character`. They look and act totally different but they're built from the same base chain.

**Method Overriding** — Garret's `interact()` method is a custom override. Instead of the default NPC behavior, when you walk into him he teleports across the screen and changes his dialogue. That's a parent method replaced with my own logic.

**Constructor Chaining** — The moment this level loaded and Kirby appeared on screen, `Player → Character → GameObject` all ran their constructors in order through `super()` calls, passing `playerData` up the chain the whole way.

<iframe 
    src="https://sprintingsnails.opencodingsociety.com/gamify/timmycounter.html" 
    width="100%" 
    height="500px" 
    style="border: none; border-radius: 12px; margin-top: 16px;"
    allowfullscreen>
</iframe>

</div>