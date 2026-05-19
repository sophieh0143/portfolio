---
layout: post 
title: Debugging Checklist
permalink: /Debugging-checklist
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
</style>

# 🔍 Debugging Checklist

---

<div class="oop-section" markdown="1" id="console-debugging">

## Console Debugging

**What is console debugging?** Console debugging is when you use `console.log()` to print out what's happening inside your code while it runs. Instead of guessing why something is broken, you print out the actual values of variables at key moments — like when a collision happens, when a player connects, or when a method gets called. It's basically like asking the program "hey, what are you doing right now?" and having it tell you.

**Requirement:** Use console.log to track game state, variables, method calls

**Assessment Method:** Code review: Strategic logging in update/collision methods

### Evidence

<div class="evidence-block" markdown="1">

**Tag event logging in TagCollisionDetector** — When a tag actually happens, I log the player's socket ID and the exact distance so I can verify the collision math is working correctly. Without this I'd have no way to tell if the hitRadius was too big or too small.

```javascript
if (dist < this.hitRadius * 2) {
    // log who got tagged and how close they were when it happened
    console.log(`[TAG] Tagged ${sid} at distance ${dist.toFixed(1)}`);
    this.socket.emit("tag", { taggedId: sid });
    this.tagCooldownUntil = now + this.tagCooldownDuration;
    break;
}
```

</div>

<div class="evidence-block" markdown="1">

**Socket connection and state logging in GameLevelMultiplayer** — I log whenever the socket connects or disconnects so I can see in the console if the server connection is actually working. This was really useful when I was first setting up the multiplayer — I could immediately tell if the WebSocket handshake was succeeding or failing.

```javascript
socket.on("connect", () => {
    // confirms the WebSocket connected and shows our assigned ID
    console.log("connected:", socket.id);
    myIdRef.value = socket.id;
});

socket.on("disconnect", () => {
    // lets us know if the connection drops unexpectedly
    console.log("disconnected from server");
});
```

</div>

<div class="evidence-block" markdown="1">

**Enemy hit logging in GameLevelBattle** — Every time a projectile hits the enemy I log "ENEMY HIT" and the remaining health value. This helped me debug the projectile collision detection — I could see in the console exactly when hits were registering and whether the health was going down correctly.

```javascript
if (hit) {
    proj.destroy();
    enemy.data.health -= 10;

    if (enemy.data.health <= 0) {
        enemy.data.health = 0;
        showWinScreen();
    }

    updateEnemyHealthBar(enemy);

    // logs every hit so we can verify damage is applying correctly
    console.log("ENEMY HIT", enemy.data.health);

    self.projectiles.splice(i, 1);
}
```

</div>

<div class="evidence-block" markdown="1">

**Music autoplay debugging in GameLevelGarett** — The browser blocks audio from playing automatically, so I added a console log to catch that specific error and explain what happened. This made it much easier to figure out why the music wasn't starting on page load.

```javascript
music.play().catch(() => {
    // this fires when the browser blocks autoplay — common on Chrome
    console.log("Music auto-play blocked. Waiting for user interaction...");
});
```

</div>

</div>

---

<div class="oop-section" markdown="1" id="hitbox-visualization">

## Hit Box Visualization

**What is hitbox visualization?** Hitboxes are the invisible boxes the game uses to detect collisions — but since they're invisible, it's really hard to tell if they're the right size or in the right place. Hitbox visualization means making those boxes visible temporarily so you can actually see them on screen and adjust them. In my game I controlled hitbox size using `widthPercentage` and `heightPercentage` values in each object's data.

**Requirement:** Draw/visualize collision boundaries to refine detection

**Assessment Method:** Demo: Toggle hit box display, adjust collision rectangles

### Evidence

<div class="evidence-block" markdown="1">

**Hitbox tuning across levels** — Every game object I built has a `hitbox` property that controls how big the collision area is as a percentage of the sprite. I had to tune these by eye — making them visible (setting `visible: true` on barriers), checking where collisions were triggering, and adjusting until they felt right.

```javascript
// Player hitbox in GameLevelTimmyfuncounter — tight hitbox so you can squeeze through gaps
const playerData = {
    hitbox: { widthPercentage: 0.2, heightPercentage: 0.2 }
    // 0.2 = only 20% of the sprite width/height is "solid"
    // this lets Kirby fit through narrow maze passages
};

// Garret NPC in the same level — wider hitbox so you can interact from further away
const npcData1 = {
    hitbox: { widthPercentage: 0.4, heightPercentage: 0.6 }
    // bigger = easier to trigger the dialogue when walking near him
};

// Barrier walls — hitbox set to 0 so the invisible walls use their exact dimensions
const dbarrier_1 = {
    id: 'dbarrier_1', x: 0, y: 0, width: 504, height: 109,
    visible: false, // toggle to true to see the wall on screen for debugging
    hitbox: { widthPercentage: 0.0, heightPercentage: 0.0 }
};
```

</div>

<div class="evidence-block" markdown="1">

**Popcorn hitbox in GameLevelHooray** — The popcorn collectible started with a hitbox that was way too small and almost impossible to collect. I increased it to 0.8 (80% of the sprite size) so it was actually fun to grab. Being able to see and adjust this number directly was key to making the level feel playable.

```javascript
const popcornData = {
    id: 'popcorn',
    SCALE_FACTOR: 2.5, // made it slightly bigger to make it easier to hit
    pixels: { height: 400, width: 400 },
    hitbox: { widthPercentage: 0.8, heightPercentage: 0.8 }
    // 0.8 = 80% of the sprite — generous hitbox so collecting feels responsive
    // originally this was 0.3 and it was nearly impossible to pick up
};
```

</div>

</div>

---

<div class="oop-section" markdown="1" id="source-debugging">

## Source-Level Debugging

**What is source-level debugging?** Source-level debugging means using the browser's DevTools Sources tab to pause your code mid-execution and step through it line by line. You set a "breakpoint" on a line and when the program reaches it, everything freezes so you can inspect every variable's value at that exact moment. It's way more powerful than console.log because you can see the entire state of your program, not just what you remembered to log.

**Requirement:** Set breakpoints in DevTools, step through code execution

**Assessment Method:** Demo: Use Sources tab to pause and inspect code flow

### Evidence

<div class="evidence-block" markdown="1">

**Debugging the tag collision detection** — When tags weren't registering correctly, I set a breakpoint inside the `update()` method of `TagCollisionDetector` right before the distance check. I could then step through and watch `dx`, `dy`, and `dist` update in real time as I moved around, which showed me exactly why certain collisions were or weren't triggering.

```javascript
update() {
    const myId = this.myIdRef.value;
    if (!myId || this.tagStateRef.taggerId !== myId) return;

    const now = Date.now();
    if (now < this.tagCooldownUntil) return;

    // ← SET BREAKPOINT HERE to inspect px, py, and all remote player positions
    const px = this.playerInstance.position?.x ?? this.playerInstance.x;
    const py = this.playerInstance.position?.y ?? this.playerInstance.y;

    for (const sid in this.remotePlayersRef) {
        const rp = this.remotePlayersRef[sid];
        const dx = local.cx - remote.cx;
        const dy = local.cy - remote.cy;
        const dist = Math.sqrt(dx * dx + dy * dy);
        // ← step through here to watch dist value update as you move
        
        if (dist < this.hitRadius * 2) {
            this.socket.emit("tag", { taggedId: sid });
            break;
        }
    }
}
```

</div>

<div class="evidence-block" markdown="1">

**Debugging the sprite switching logic** — The character dropdown menu in the multiplayer level wasn't updating the player sprite correctly at first. I put a breakpoint inside `setSprite()` and stepped through it to find that `getPlayer()` was returning `undefined` because the player ID lookup was case-sensitive — `'playerdata'` vs `'playerData'`. DevTools showed me the exact value being returned, which I never would have caught with just console logs.

```javascript
const getPlayer = () => {
    // ← BREAKPOINT HERE — inspect what gameEnv.gameObjects actually contains
    return gameEnv.gameObjects.find(obj => obj.id === 'playerdata');
    // bug: 'playerdata' should be 'playerData' — DevTools revealed this instantly
};

const setSprite = (spriteOption) => {
    const player = getPlayer();
    // ← step in here to check if player is undefined or a real object
    if (!player || !spriteOption) return;
    // ...
};
```

</div>

</div>

---

<div class="oop-section" markdown="1" id="network-debugging">

## Network Debugging

**What is network debugging?** The Network tab in DevTools shows you every single request your page makes — including WebSocket connections, API fetch calls, and file loads. You can see if requests succeed or fail, check the exact data being sent and received, and catch errors like CORS blocks or 404s. For my multiplayer game this was essential because I needed to verify that the WebSocket was actually sending and receiving position updates correctly.

**Requirement:** Examine Network tab for API calls, CORS errors, response status

**Assessment Method:** Demo: Inspect fetch requests, response data, error messages

### Evidence

<div class="evidence-block" markdown="1">

**WebSocket traffic in the multiplayer game** — In the Network tab under the WS (WebSocket) filter, you can watch every `move` and `tag` event being sent to the server in real time. I used this to verify that position updates were actually being transmitted every 50ms like they were supposed to, and to check the exact JSON payload being sent.

```javascript
// NetworkSynchronizer sends this every 50ms — visible in Network tab as WS frames
this.socket.emit("move", {
    x: this.playerInstance.position?.x ?? this.playerInstance.x,
    y: this.playerInstance.position?.y ?? this.playerInstance.y
});

// TagCollisionDetector sends this when a tag happens
this.socket.emit("tag", { taggedId: sid });
```

In DevTools → Network → WS, you can click on the WebSocket connection and see every single message frame. Each `move` event shows up as a small JSON object. If the server is down or the connection fails, you'd see a red error here instead.

</div>

<div class="evidence-block" markdown="1">

**Switching between local and production server** — I had two different server URLs in my code and used the Network tab to confirm which one was actually connecting. The commented-out line is the local development server, and the active line is the production server.

```javascript
// local dev server — use this when testing on your own machine
// const socket = io("ws://localhost:8590", { transports: ["websocket"] });

// production server — use this for the deployed version
const socket = io("wss://flask-ws.opencodingsociety.com", { transports: ["websocket"] });
```

In DevTools Network tab, I could see the WebSocket handshake request and whether it returned a 101 Switching Protocols status (success) or an error. A failed connection shows up as red with a connection refused message.

</div>

</div>

---

<div class="oop-section" markdown="1" id="application-debugging">

## Application Debugging (Storage)

**What is application debugging?** The Application tab in DevTools lets you inspect everything the browser is storing for your page — cookies, localStorage, sessionStorage, and more. For my maze game I store the leaderboard scores in localStorage, so I used the Application tab to check that scores were actually being saved correctly, see the raw JSON data, and clear it when testing to make sure the save/load logic worked from scratch.

**Requirement:** Examine cookies, localStorage, session data for login/state

**Assessment Method:** Demo: Application tab inspection of stored data

### Evidence

<div class="evidence-block" markdown="1">

**Leaderboard data in localStorage** — The maze level saves the top 5 scores to `localStorage` under the key `"mazeScores"`. In DevTools → Application → Local Storage, you can see the raw JSON string stored there and verify that scores are being saved, sorted, and trimmed to 5 entries correctly.

```javascript
saveToLeaderboard(steps) {
    // this writes to localStorage — visible in DevTools Application tab
    let scores = JSON.parse(localStorage.getItem("mazeScores")) || [];
    scores.push({ steps: steps, date: new Date().toLocaleTimeString() });
    scores.sort((a, b) => a.steps - b.steps);
    scores = scores.slice(0, 5); // keep only top 5
    localStorage.setItem("mazeScores", JSON.stringify(scores));
    // in Application tab, "mazeScores" key will show something like:
    // [{"steps":12,"date":"9:42:01 AM"},{"steps":15,"date":"9:45:33 AM"}]
}

updateLeaderboardDisplay() {
    // this reads from localStorage — if Application tab shows empty, display will be empty too
    const scores = JSON.parse(localStorage.getItem("mazeScores")) || [];
    // ...
}
```

To test this I would open Application tab, find the `mazeScores` key, manually delete it, then play through and win — and watch the entry appear in real time after submitting a score.

</div>

</div>

---

<div class="oop-section" markdown="1" id="element-inspection">

## Element Inspection

**What is element inspection?** The Elements tab in DevTools shows you the actual HTML and CSS of everything on the page. For a canvas game this is especially useful for checking the health bars, HUD elements, and UI overlays I created dynamically with JavaScript. I could click on any element and see its exact position, size, z-index, and styles — and even edit them live to test changes without reloading.

**Requirement:** Use Element Viewer to inspect canvas, DOM elements, styles

**Assessment Method:** Demo: Inspect element properties and game object state

### Evidence

<div class="evidence-block" markdown="1">

**Inspecting dynamically created health bars** — Both health bars in `GameLevelBattle` are created entirely in JavaScript and appended to the DOM. I used the Elements tab to find them, check their exact pixel positions, and debug why the enemy health bar was appearing in the wrong spot. I could see the `style.left` and `style.top` values being set dynamically and adjust the offset.

```javascript
function createHealthBar() {
    const container = document.createElement('div');
    container.id = 'player-health-container';
    // these styles are all visible and editable in DevTools Elements tab
    Object.assign(container.style, {
        position: 'fixed',
        top: '60px',       // ← inspect this in Elements to verify placement
        left: '20px',
        width: '240px',
        height: '24px',
        background: '#222',
        border: '2px solid white',
        zIndex: '9999'     // ← check z-index if health bar is hidden behind other elements
    });
    document.body.appendChild(container);
}

function updateEnemyHealthBar(enemy) {
    const bar = document.getElementById('enemy-health-bar');
    // these update every frame — watch them change live in Elements tab
    bar.style.left = enemy.position.x + 'px';
    bar.style.top = (enemy.position.y - 10) + 'px';
    bar.style.width = (pct * 80) + 'px';
}
```

</div>

<div class="evidence-block" markdown="1">

**Inspecting the game canvas element** — The main game canvas is a `<canvas>` element that the engine creates. Using the Elements tab I could check its width, height, and CSS position to make sure it was filling the screen correctly. I also used it to find the IDs of dynamically created elements like `'popcorn-counter'` and `'leaderboard-panel'` to verify they were being added and removed properly.

```javascript
// These elements are all inspectable in DevTools Elements tab:

// The step counter HUD — check its z-index if it's getting covered
const hud = document.createElement("div");
hud.style.cssText = "position:fixed; bottom:20px; left:50%; transform:translateX(-50%); z-index:10000;";

// The leaderboard panel — check display:none vs display:block when toggling
const panel = document.createElement("div");
panel.id = "leaderboard-panel";
panel.style.cssText = "position:fixed; top:50%; left:50%; ... display:none; z-index:10002;";

// The win/game over screens — verify they cover the whole viewport
const screen = document.createElement('div');
Object.assign(screen.style, {
    position: 'fixed',
    width: '100vw',
    height: '100vh',
    zIndex: '999999'  // needs to be highest z-index on the page
});
```

</div>

</div>

---

<div class="oop-section" markdown="1">

## 🎮 See It All In Action

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