---
layout: post 
title: Data Types Checklist
permalink: /old-DataTypes-checklist
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

# 🏗️ Data Types Checklist

---

<div class="oop-section" markdown="1" id="writing-classes">

## Numbers
**What is a class?** In JavaScript, the number data type handles both integers (whole numbers) and floating-point numbers (decimals). Numbers are used across games to handle rendering measurements, run canvas-coordinate physics equations, track game clocks, and control sprite scales.

**Requirement:** Implement numeric primitive variables to control positioning scales, animations, or timing cycles.

**Assessment Method:** Code review — verified declaration and usage of quantitative values.

### Evidence

<div class="evidence-block" markdown="1">

**GameLevelTimmyfuncounter** — This is my invisible maze level. The whole level is wrapped inside one class called `GameLevelTimmyfuncounter`. When the game engine starts this level, it calls the `constructor` — which is basically the setup function that runs once at the beginning. Inside there I define what the background looks like, where Kirby spawns, where the NPC Garret goes, and where all the invisible walls are placed. At the end I hand all of that to `this.classes` which is the list the game engine reads to actually build everything on screen.

```javascript
<div class="evidence-block" markdown="1">

**RemotePlayerVisualizer Constant Configs** — Inside the multiplayer engine, I use numeric integers and decimal values to define sprite dimension rules. `SCALE_FACTOR` acts as a multiplier, while `frameWidth` divides a raw image width asset evenly across a 13-column sheet grid.

```javascript
class RemotePlayerVisualizer extends GameObject {
    constructor(data = null, gameEnv = null) {
        super(gameEnv);
        this.SCALE_FACTOR = 3.5;       // Decimal number (Floating-Point) for vector rescaling
        this.frameWidth = 569 / 13;    // Mathematical number division producing structural coordinates
        this.frameHeight = 36;         // Whole integer specifying native frame pixel height
    }
}
export default GameLevelTimmyfuncounter; // makes this class available to other files
```

**TagHUD Expiration Tracking** — For the player's tag status immunity window, I store the exact moment they changed roles using a high-precision millisecond timestamp. This numeric timestamp is then used in a subtraction formula to calculate remaining immunity.

```javascript
// gracePeriod stores an integer threshold of 2000 milliseconds
const gracePeriod = 2000; 

// timeSinceIt calculates a numeric difference via timestamp math
const timeSinceIt = now - (this.tagStateRef.becameItAt ?? 0); 

// secondsLeft converts milliseconds to an isolated countdown integer
const secondsLeft = Math.ceil((gracePeriod - timeSinceIt) / 1000);
```
</div>

<div class="oop-section" markdown="1">

</div>

---

## Strings

**What is a string data type?** A string represents textual data wrapped in quotes (', ", or `). Strings are vital for passing static file paths to asset managers, emitting targeted event signals through network sockets, and rendering UI warnings to players.

**Requirement:** Utilize string parameters for asset loading references or network event keys.

**Assessment Method:** Code review — direct integration of textual variables or parameters.

### Evidence

<div class="evidence-block" markdown="1">

**Socket Endpoint Events** — My multiplayer script relies entirely on matching string labels to transmit updates between the backend and frontend. Network triggers like "connect", "move", "tag", and "player_left" are string arguments that tell WebSockets exactly how to process the network packets.

```javascript
// The string "player_left" dictates the precise socket callback routing path
socket.on("player_left", (data) => {
    delete remotePlayers[data.sid]; // sids themselves are long unique hash strings
});

// The string "tag" identifies the message type being sent to the server
this.socket.emit("tag", { taggedId: sid });
```
**Background Asset Loading Paths** — Strings are used to assemble the image locations for the game canvas. By concatenating gameEnv.path (a directory string) with an absolute location path string, the engine can correctly fetch assets.
```Javascript
const bgData = {
    name: "custom_bg", // String identification label
    src: path + "/images/projects/tag-game-multiplayer/Arena.png", // Assembled image location path string
    pixels: { height: 720, width: 1280 }
};
```
</div>

<div class="oop-section" markdown="1">

</div>

---

## Booleans

**What is a boolean data type?** A boolean is a binary flag that can only ever be true or false. Booleans control the conditional logic inside a game engine, functioning as simple toggle switches to track states like whether a sprite asset has finished downloading, or if a player is currently marked as "IT".

**Requirement:** Incorporate boolean state flags to flip execution loops or conditional branches.

**Assessment Method:** Code review — logical evaluation of flags holding exclusively true/false values.

### Evidence

<div class="evidence-block" markdown="1">

**TagHUD Alternating Visibility Checks** — Inside TagHUD, the boolean flag variable isIt evaluates whether your personal client ID matches the server-tracked hunter ID. This instantly flips your display state between active hazard warnings and a safe display state.

```javascript
// From GameLevelTimmyfuncounter.js
// isIt evaluates directly to a true or false primitive flag
const isIt = this.tagStateRef.taggerId === this.myIdRef.value;

if (isIt) {
    // Executes only when the boolean state is true
} else {
    // Executes only when the boolean state is false
}
```
**Dynamic Sprite Switch Flags** — When shifting characters using the dropdown menu inside GameLevelMultiplayer, the code sets a boolean property spriteReady to false. This locks drawing loops until an .onload callback switches it to true.

```javascript
player.spriteReady = false; // Turn off drawing safety flag

player.spriteSheet.onload = () => {
    player.spriteReady = true; // Turn flag on when the image is fully downloaded
    player.resize();
};
```
</div>

<div class="oop-section" markdown="1">

</div>

---

## Arrays

**What is an array?** An array is an ordered list of items stored inside square brackets []. Each item in an array has a numbered index starting at 0. Arrays are highly useful for managing linear collections of items, such as a predefined set of selectable player models, coordinate lists, or level layers.

**Requirement:** Implement structured array configurations to manage lists of data elements.

**Assessment Method:** Code review — definition and loop traversal of index-based [] arrays.

### Evidence

<div class="evidence-block" markdown="1">

**Character Option Array (spriteOptions)** — Centralizing data choices in an array lets the selection menu programmatically read, switch, and apply configurations based on index matching without repeating blocks of code.

```javascript
const spriteOptions = [
    { label: "Boy", src: path + "/images/projects/characters/boysprite.png", ... },          // Index 0
    { label: "Scuba Diver", src: path + "/images/projects/characters/scubadiver.png", ... }, // Index 1
    { label: "Astro", src: path + "/images/projects/characters/astro.png", ... },            // Index 2
    { label: "Kirby", src: path + "/images/projects/characters/kirby.png", ... }             // Index 3
];
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

## Objects (JSON)

**What is an object?** An object is a structural entity enclosed in curly braces {} that maps configurations via explicit key: value properties. JavaScript objects share syntax rules with JSON (JavaScript Object Notation), which acts as a worldwide data transfer format to package complex runtime attributes across internet servers.

**Requirement:** Group parameters into unified object configurations or stream socket parameters inside JSON blocks.

**Assessment Method:** Code review — verified declaration of nested key-value entities.

### Evidence

<div class="evidence-block" markdown="1">

**Multiplayer Entity Configuration Models** — Level parameters use multi-layered key-value maps to collect character sprite scales, starting coordinate configurations, animation tracking speeds, and map key codes under a single object variable identifier.

```javascript
const playerData = {
    id: 'playerData',
    src: '/images/gamebuilder/sprites/kirby.png',
    SCALE_FACTOR: 10,
    INIT_POSITION: { x: width * 0.5, y: height * 0.5 }, // Nested secondary value object
    hitbox: { widthPercentage: 0.2, heightPercentage: 0.2 },
    keypress: { up: 87, left: 65, down: 83, right: 68 } // Mapped key values configuration object
};
```
Real-time JSON Socket Payloads — When interacting with our network server backend, client data must be bundled into structured object maps before transmission to ensure proper communication over network sockets.

```javascript
// Bundling tracking coordinates into a JSON footprint object structure
this.socket.emit("move", {
    x: this.playerInstance.position?.x ?? this.playerInstance.x,
    y: this.playerInstance.position?.y ?? this.playerInstance.y
});
```
</div>

</div>

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


