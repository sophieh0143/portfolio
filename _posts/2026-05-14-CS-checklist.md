---
layout: post 
title: Control Structures Checklist
permalink: /ControlStructures-checklist
hide: true
show_reading_time: false
---

<style>
.cs-section {
    background: rgba(255,255,255,0.05);
    border: 2px solid rgba(255,255,255,0.1);
    border-radius: 12px;
    padding: 20px 28px;
    margin: 28px 0;
}
.cs-section h2 {
    color: #2ecc71;
    border-bottom: 2px solid #2ecc71;
    padding-bottom: 8px;
    margin-top: 0;
}
.cs-section h3 {
    color: #f0c040;
    margin-top: 20px;
}
.evidence-block {
    background: rgba(0,0,0,0.3);
    border-left: 4px solid #2ecc71;
    border-radius: 0 8px 8px 0;
    padding: 12px 16px;
    margin: 12px 0;
    font-size: 0.95em;
}
.gamerunner-note {
    background: rgba(46, 204, 113, 0.1);
    border: 2px dashed #2ecc71;
    border-radius: 8px;
    padding: 12px 16px;
    margin: 16px 0;
    font-size: 0.95em;
    color: #a0f5c0;
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
</style>

# ⚙️ Control Structures Checklist

---

<div class="cs-section" markdown="1" id="iteration">

## Iteration (Loops)

**What is iteration?** Iteration means repeating a block of code multiple times — instead of writing the same thing 10 times, you write it once and tell the computer to loop through it. There are different kinds of loops: `for` loops run a set number of times, `forEach` loops go through every item in an array, and `while` loops keep going until a condition becomes false. In games, loops are everywhere — looping through all the players, looping through all the walls, looping through every frame of animation.

**Requirement:** Use loops for game object arrays, animation frames

**Assessment Method:** Code review — `for`, `forEach`, `while` loops

### Evidence

<div class="evidence-block" markdown="1">

**`for...in` loop in RemotePlayerVisualizer** — Every frame, this loop goes through every player currently connected to the multiplayer server and draws their Kirby sprite on screen. `for (const sid in this.remotePlayersRef)` means "for each socket ID in the remote players list, do this." If 5 people are connected, this runs 5 times. If 1 person is connected, it runs once.

```javascript
draw() {
    // for...in loops through every key in an object
    // sid = the socket ID (like a unique name) for each remote player
    for (const sid in this.remotePlayersRef) {
        const p = this.remotePlayersRef[sid]; // grab that player's data (x, y position)
        const isIt = this.tagStateRef.taggerId === sid; // are they "it"?

        // draw their kirby sprite at their position
        ctx.drawImage(
            this.spriteImage,
            0, 0,
            this.frameWidth, this.frameHeight, // which part of the sprite sheet to use
            p.x, p.y,                          // where on screen to draw them
            drawWidth, drawHeight              // how big to draw them
        );

        // if they're "it", draw a red overlay on top of their sprite
        if (isIt) {
            ctx.fillStyle = 'red';
            ctx.fillRect(p.x, p.y, drawWidth, drawHeight);
        }
    }
}
```

</div>

<div class="evidence-block" markdown="1">

**`.map()` loop to build maze walls** — In `GameLevelTimmyfuncounter`, I define the maze as an array of wall shapes, then use `.map()` to loop through each one and convert it into a proper `Barrier` class entry. `.map()` is like `forEach` but it gives back a new array — perfect for transforming one list into another.

```javascript
// first define all the walls as simple objects with position and size
const mazeWalls = [
    { x: 0, y: 0, width: width, height: 20 },           // top border
    { x: 0, y: height - 20, width: width, height: 20 }, // bottom border
    { x: width * 0.2, y: 0, width: 20, height: height * 0.6 }, // wall 1
    { x: width * 0.4, y: height * 0.4, width: 20, height: height * 0.6 }, // wall 2
    { x: width * 0.6, y: 0, width: 20, height: height * 0.6 }, // wall 3
    { x: width * 0.8, y: height * 0.4, width: 20, height: height * 0.6 }  // wall 4
];

// .map() loops through each wall object and converts it into a Barrier class entry
// wall = one item from the array above, on each loop
const wallClasses = mazeWalls.map(wall => ({
    class: Barrier,
    data: {
        id: "wall_" + Math.random(), // give each wall a unique ID
        x: wall.x,
        y: wall.y,
        width: wall.width,
        height: wall.height,
        visible: false // invisible walls!
    }
}));
// wallClasses is now an array of 6 Barrier entries ready to go into this.classes
```

</div>

<div class="evidence-block" markdown="1">

**`forEach` loop over leaderboard scores** — In `updateLeaderboardDisplay()`, I loop through the saved scores array and build an HTML string for each one. `.forEach` is the cleanest way to loop through an array when you just want to do something with each item and don't need a new array back.

```javascript
updateLeaderboardDisplay() {
    const scores = JSON.parse(localStorage.getItem("mazeScores")) || [];
    let html = "<h2>🏆 Top 5 Efficiencies</h2>";

    // forEach gives you each item + its index (position in the array)
    // s = one score object { steps, date }, i = 0, 1, 2, 3, 4
    scores.forEach((s, i) => {
        // build an HTML paragraph for each score
        html += `<p>${i + 1}. <b>${s.steps} steps</b> <br><small>${s.date}</small></p>`;
        //         ^ i+1 so it shows 1, 2, 3... instead of 0, 1, 2...
    });

    panel.innerHTML = html; // put all the paragraphs into the leaderboard panel
}
```

</div>

</div>

---

<div class="cs-section" markdown="1" id="conditionals">

## Conditionals

**What are conditionals?** Conditionals let your code make decisions. The most basic one is `if/else` — "if this is true, do this, otherwise do that." Without conditionals, your game would do the same thing no matter what. Conditionals are what make the game respond differently depending on what's happening — if you touched an enemy, if you ran out of steps, if you're the one who's "it."

**Requirement:** Implement collision detection, state transitions

**Assessment Method:** Code review — `if/else`, nested conditions

### Evidence

<div class="evidence-block" markdown="1">

**Step counter fail state** — In `GameLevelTimmyfuncounter`, every time you press a movement key the step counter goes up. There are two conditionals checking it: one that turns the counter red when you're getting close to the limit, and one that ends the game if you go over.

```javascript
document.addEventListener("keydown", (e) => {
    const movementKeys = [87, 65, 83, 68]; // W, A, S, D key codes

    if (movementKeys.includes(e.keyCode)) { // only count if it's a movement key
        window.currentSteps++;
        stepCounterEl.textContent = `Steps: ${window.currentSteps} / ${STEP_GOAL}`;

        // first conditional: are we past 80% of the step limit?
        if (window.currentSteps > STEP_GOAL * 0.8) {
            stepCounterEl.style.color = "red";        // turn counter red as a warning
            stepCounterEl.style.borderColor = "red";
        }

        // second conditional: did we go over the limit completely?
        if (window.currentSteps > STEP_GOAL) {
            alert("TOO MANY STEPS! Game Over."); // game over message
            window.location.reload();            // restart the page
        }
    }
});
```

</div>

<div class="evidence-block" markdown="1">

**Tag grace period check** — In `TagCollisionDetector`, before allowing a tag there are multiple conditionals stacked up as "gates." You have to pass all of them before a tag can happen — you must be "it", the grace period must be over, and the cooldown must be done.

```javascript
update() {
    const myId = this.myIdRef.value;

    // gate 1: am I even "it"? if not, stop here and do nothing
    if (!myId || this.tagStateRef.taggerId !== myId) return;

    const now = Date.now();

    // gate 2: is the grace period over? (2 seconds after becoming "it" you can't tag)
    const timeSinceIt = now - (this.tagStateRef.becameItAt ?? 0);
    if (timeSinceIt < 2000) return; // still in grace period, stop here

    // gate 3: is the cooldown over? (can't spam tags)
    if (now < this.tagCooldownUntil) return; // still on cooldown, stop here

    // all gates passed — now check distance to each player
    for (const sid in this.remotePlayersRef) {
        const dist = Math.sqrt(dx * dx + dy * dy);
        if (dist < this.hitRadius * 2) {
            this.socket.emit("tag", { taggedId: sid }); // tag them!
            break; // stop after first tag
        }
    }
}
```

</div>

</div>

---

<div class="cs-section" markdown="1" id="nested-conditions">

## Nested Conditions

**What are nested conditions?** Nested conditions are just `if` statements inside other `if` statements. You use them when there are multiple things that all have to be true at the same time, or when different combinations of conditions lead to different outcomes. Think of it like a decision tree — you check one thing, and depending on the answer you check something else inside that.

**Requirement:** Complex game logic (e.g., power-up + collision + direction)

**Assessment Method:** Code review — multi-level conditionals

### Evidence

<div class="evidence-block" markdown="1">

**Enemy damage system in GameLevelBattle** — This has three levels of nesting: first check if they're touching, then check if the cooldown is over, then apply damage and check if health hit zero.

```javascript
enemy.update = function () {
    // level 1: are Kirby and the enemy overlapping?
    if (touching) {
        const now = Date.now();

        // level 2: has it been long enough since the last hit? (cooldown)
        if (now - enemy.data.lastHit > 500) {
            enemy.data.lastHit = now;
            player.data.health -= 10; // deal damage

            // level 3: did that damage kill the player?
            if (player.data.health <= 0) {
                player.data.health = 0;  // don't go below 0
                updateHealthBar(0);
                showGameOver();          // show the game over screen
            } else {
                updateHealthBar(player.data.health); // just update the bar normally
            }
        }
    }
};
```

</div>

<div class="evidence-block" markdown="1">

**TagHUD grace period + flash** — In the multiplayer HUD, there are nested conditionals to handle whether to show the grace period countdown, and separately whether to show the flash effect — both only apply when you're "it."

```javascript
draw() {
    const isIt = this.tagStateRef.taggerId === this.myIdRef.value;

    // level 1: am I "it"?
    if (isIt) {
        // draw the red border...

        // level 2: is there an active flash effect right now?
        if (this._flashStart) {
            const elapsed = now - this._flashStart;

            // level 3: is the flash still within its duration?
            if (elapsed < 600) {
                const alpha = 0.45 * (1 - elapsed / 600); // fade out over 600ms
                ctx.fillStyle = `rgba(255, 0, 0, ${alpha})`;
                ctx.fillRect(0, 0, W, H); // red flash over the whole screen
            } else {
                this._flashStart = null; // flash is done, clear it
            }
        }

        // level 2 (separate branch): are we still in the grace period?
        if (inGrace) {
            const secondsLeft = Math.ceil((2000 - timeSinceIt) / 1000);
            // show the grace period countdown timer
        }
    }
}
```

</div>

</div>

---

<div class="cs-section" markdown="1">

## 🎮 See It All In Action

<!-- PUT YOUR GAME RUNNER / IFRAME HERE -->
<!-- Suggested: Timmy's Fun Counter (/gamify/timmycounter) -->
<!-- WHY: The step counter uses iteration (keydown listener counting keys), -->
<!-- conditionals (warning at 80%, game over at 100%), and nested conditions -->
<!-- (Garret's interact checks teleported THEN checks win/lose inside that). -->
<!-- All three control structures are visible while playing. -->

<iframe 
    src="https://sprintingsnails.opencodingsociety.com/gamify/timmycounter.html" 
    width="100%" 
    height="500px" 
    style="border: none; border-radius: 12px; margin-top: 16px;"
    allowfullscreen>
</iframe>
</div>