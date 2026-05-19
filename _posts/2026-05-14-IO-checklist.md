---
layout: post 
title: Input/Output Checklist
permalink: /IO-checklist
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
</style>

# 🏗️ Input/Output Checklist

---

<div class="oop-section" markdown="1" id="writing-classes">

## Keyboard Input (Event Listeners & WASD Controls)

**What is keyboard input?** Keyboard input is how the game listens to what keys you are pressing so it can move your character around. We use JavaScript event listeners to watch for `keydown` and `keyup` events, mapping controls like WASD, Arrow keys, or Spacebar to change the player's velocity.

**Requirement:** Set up keyboard event handlers that correctly detect and respond to player key inputs.

### Evidence

<div class="evidence-block" markdown="1">

**WASD Tracking Inside Player Class** — The game uses an array or object to keep track of which keys are currently held down. When you press 'W' (key code 87) or 'A' (key code 65), the event listener flips a flag so the update loop knows to move the player.

```javascript
// Listening for keys being pressed down
window.addEventListener('keydown', (e) => {
    if (e.keyCode === 87) this.pressedKeys['up'] = true;    // W Key
    if (e.keyCode === 65) this.pressedKeys['left'] = true;  // A Key
    if (e.keyCode === 83) this.pressedKeys['down'] = true;  // S Key
    if (e.keyCode === 68) this.pressedKeys['right'] = true; // D Key
});

// Listening for when you let go of the keys
window.addEventListener('keyup', (e) => {
    if (e.keyCode === 87) this.pressedKeys['up'] = false;
    // ... maps the rest of the keys back to false so the character stops moving
});
```

## Canvas Rendering

**What is canvas rendering?** Canvas rendering is how the game actually displays graphics on your screen. It uses the HTML5 Canvas API context (ctx) to constantly clear the screen and draw game elements like backgrounds, custom platforms, and animated player frames onto specific X and Y coordinates.

**Requirement:** Implement custom draw() methods that use the Canvas API to render graphics onto the game window.

### Evidence

<div class="evidence-block" markdown="1">

RemotePlayerVisualizer Draw Implementation — This code takes the character sprite image sheet and crops out the correct animation frame, then draws it onto the canvas using ctx.drawImage().

```javascript
draw() {
    if (!this.spriteImage || !this.remotePlayersRef) return;

    // Looping through all network players and painting them onto the canvas context
    for (let id in this.remotePlayersRef) {
        const p = this.remotePlayersRef[id];
        
        this.gameEnv.ctx.drawImage(
            this.spriteImage,
            this.frameX * this.frameWidth, 0, // Where to crop from the sprite sheet
            this.frameWidth, this.frameHeight, // Size of the crop
            p.x, p.y,                          // X and Y positions on the game screen
            this.frameWidth * this.SCALE_FACTOR, 
            this.frameHeight * this.SCALE_FACTOR
        );
    }
}
```
</div>

<div class="oop-section" markdown="1">

</div>

---

## GameEnv Configuration

**What is GameEnv configuration?** GameEnv is basically a global settings object that keeps track of core engine rules—like how big the game window is, the difficulty levels, game speeds, and which assets are currently loaded into the scene.

**Requirement:** Use a central configuration file or setup method (GameEnv.create()) to define universal game rules.

### Evidence

<div class="evidence-block" markdown="1">

**GameSetup.js Global Rules Setup** — This initializes our canvas dimensions and defines basic setup rules so that all game levels stay uniform.

```javascript
const GameSetup = {
    canvasId: "gameCanvas",
    width: 1280,   // Sets the core width of the game window
    height: 720,   // Sets the core height of the game window
    difficulty: "normal",
    gameSettings: {
        gravity: 0.5,
        maxPlayers: 8
    },
    
    create() {
        // Appends settings directly to our global GameEnv core object
        GameEnv.width = this.width;
        GameEnv.height = this.height;
        GameEnv.gravity = this.gameSettings.gravity;
    }
};
```
</div>

<div class="oop-section" markdown="1">

</div>

---

## API Integration

**What is API integration and async Input/Output?** API integration lets our game talk to a backend database server. We use asynchronous JavaScript (async/await and fetch) to do things in the background—like using a GET request to pull the top scores for a leaderboard, or a POST request to upload a new high score when a game ends without freezing up the screen gameplay.

**Requirement:** Use async/await and fetch with proper error handling to grab or send game data to an external backend server.

### Evidence

<div class="evidence-block" markdown="1">

**Asynchronous Leaderboard Fetch Actions** — This code uses an async function to save scores to our python flask backend server, utilizing a try/catch block to handle network errors gracefully.

```javascript
// async tells JavaScript to run this function in the background
async function saveHighScore(playerName, scoreValue) {
    const url = "[https://yourgamebackend.com/api/leaderboard](https://yourgamebackend.com/api/leaderboard)";
    
    try {
        // await pauses this function until the POST request finishes sending over the web
        const response = await fetch(url, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ name: playerName, score: scoreValue })
        });
        
        if (!response.ok) {
            throw new Error("Server rejected our score packet!");
        }
    } catch (error) {
        // Error handling block prevents the game loop from crashing if the server is offline
        console.error("Failed to connect to Leaderboard API:", error);
    }
}
```
**SplineBarrier Coordinates List** — Bounding walls are defined by a sequenced list array. The engine parses this collection in order to build line meshes across custom levels.

```javascript
const arenaBarrierData = {
    id: 'arena-wall',
    visible: false,
    splinePoints: [ // Array holding sequential coordinate parameters
        { x: 100, y: 235 }, // Index 0
        { x: 115, y: 165 }, // Index 1
        { x: 155, y: 110 }  // Index 2
    ]
};
```
</div>

<div class="oop-section" markdown="1">

</div>

---

## JSON Parsing

**What is JSON parsing?** JSON parsing is how we translate the raw strings that data servers send back into real JavaScript objects that our game can read and use. We use JSON.parse() or automatically use .json() on fetch responses, and then use object destructuring (const { name, score } = player) to cleanly unpack the data.

**Requirement:** Parse JSON data packets from web responses and unpack properties into usable game variables.

### Evidence

<div class="evidence-block" markdown="1">

**Parsing and Unpacking Server Score Lists** — When data comes back from our leaderboard API, it arrives as raw JSON text. We turn it into an object and use destructuring to pull values straight out of it for the UI menu.

```javascript
async function loadLeaderboardData() {
    const response = await fetch("[https://yourgamebackend.com/api/leaderboard](https://yourgamebackend.com/api/leaderboard)");
    
    // Unpacking raw JSON text string into a clean, searchable array list object
    const scoresArray = await response.json(); 
    
    scoresArray.forEach(entry => {
        // Object Destructuring: Unpacks variables directly from the entry map
        const { name, score, date } = entry; 
        
        console.log(`Player: ${name} got a score of ${score} on ${date}`);
        // Code then displays these extracted variables directly onto our high score menu text...
    });
}
```

---

<div class="oop-section" markdown="1">

## Timmy Fun Counter
<iframe 
    src="https://sprintingsnails.opencodingsociety.com/gamify/timmycounter.html" 
    width="100%" 
    height="500px" 
    style="border: none; border-radius: 12px; margin-top: 16px;"
    allowfullscreen>
</iframe>

## Multiplayer
</div>



