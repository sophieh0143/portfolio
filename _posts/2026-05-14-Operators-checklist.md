---
layout: post 
title: Operators Checklist
permalink: /Operators-checklist
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

# 🏗️ Operators Checklist

---

<div class="oop-section" markdown="1" id="mathematical-physics">

## Mathematical & Physics Calculations

**What are mathematical operators?** Math operators are just the basic math symbols we use in code, like `+`, `-`, `*`, and `/`. In a game, you need them constantly to handle physics, like finding the middle of a player's sprite, calculating how fast they are moving, sizing things correctly on the screen, or figuring out the distance between two players to see if someone got tagged.

**Requirement:** Implement standard math operators to determine real-time vector coordinates, bounding boxes, or physics scales.

**Assessment Method:** Code review — presence and deployment of `+`, `-`, `*`, or `/` across calculations.

### Evidence

<div class="evidence-block" markdown="1">

**TagCollisionDetector Proximity Calculations** — To check if a tag actually happened, I used math operators to run a distance formula. The code subtracts the player positions (`-`) to find the distance between them, and then multiplies them (`*`) to square the numbers.

```javascript
// Subtracting center points (-) to see how far apart the players are on the X and Y grid
const dx = local.cx - remote.cx;
const dy = local.cy - remote.cy;

// Multiplying numbers (*) to square them for the distance formula
const dist = Math.sqrt(dx * dx + dy * dy);

// Multiplying the hit radius (*) to see if the distance is small enough for a tag
if (dist < this.hitRadius * 2) {
    this.socket.emit("tag", { taggedId: sid });
}
```
**RemotePlayerVisualizer Coordinate Scaling** — I also used multiplication and division (*, /) to scale down my player sprites so they fit nicely on the map instead of taking up the whole screen.

```javascript
// Dividing the image width (/) by 13 because there are 13 frames in the animation row
this.frameWidth = 569 / 13;

// Multiplying the width and height (*) by a scale factor to make the character bigger or smaller
const drawWidth = this.frameWidth * this.SCALE_FACTOR;
const drawHeight = this.frameHeight * this.SCALE_FACTOR;
```
</div>

<div class="oop-section" markdown="1">

</div>

---

## String Operators

**What are string operations?** String operations are ways to mash text together. You can do this by adding text together with a + sign (concatenation) or by using backticks and putting variables inside ${} (template literals). This is super helpful for making text on the screen look dynamic, or building file paths for images.

**Requirement:** Use string concatenation or template literals to merge parameters for file paths or user interface readouts.

**Assessment Method:** Code review — verified presence of + concatenation strings or `...${}...` literals.

### Evidence

<div class="evidence-block" markdown="1">

**TagHUD Screen Text Assembly** — Inside the TagHUD class, I used both template literals and the plus sign to build strings. I used a template literal to smoothly change the background box alpha color, and a + sign to put the countdown timer text together on the screen.

```javascript
// Using backticks and ${} to pass a changing number into the color string
ctx.strokeStyle = `rgba(255, 30, 30, ${0.4 + 0.5 * pulse})`;

// Using the + sign to combine an emoji, the seconds variable, and a "s" for seconds
const graceText = `🛡️ grace period: ` + secondsLeft + `s`;
ctx.fillText(graceText, graceX, graceY);
```
</div>

<div class="oop-section" markdown="1">

</div>

---

---

## Boolean Expressions

What are boolean expressions? Compound conditions are checks that combine more than one true or false question into a single line using logic operators like AND (&&), OR (||), and NOT (!). They are super important for setting up rules, like making sure a player can only trigger a tag score if they are the tagger AND their target isn't protected by a spawn shield.

Requirement: Deploy compound boolean expressions using logical operators (&&, ||, !) to handle game logic states.

Assessment Method: Code review — checking our conditional blocks for operational &&, ||, or ! tokens.

### Evidence

<div class="evidence-block" markdown="1">

**Compound Collision and Tag Checks** — To make sure players don't randomly trigger tag scores whenever they just pass by each other, I have a compound if-statement. It checks if my ID doesn't exist (!), OR if the server says I am not the current hunter (||), and stops the collision logic from firing if either is true.

```javascript
const myId = this.myIdRef.value;

// Using NOT (!) and OR (||) to double check our game rules:
// If myId is missing, OR if the server taggerId does NOT match my player ID, stop right here!
if (!myId || this.tagStateRef.taggerId !== myId) {
    return; 
}
```
---

</div>

<div class="oop-section" markdown="1">

</div>

---

<div class="oop-section" markdown="1">

## Game runners add later
<iframe 
    src="https://sprintingsnails.opencodingsociety.com/gamify/timmycounter.html" 
    width="100%" 
    height="500px" 
    style="border: none; border-radius: 12px; margin-top: 16px;"
    allowfullscreen>
</iframe>

## Multiplayer
<iframe 
    src="https://sprintingsnails.opencodingsociety.com/gamify/multiplayer" 
    width="100%" 
    height="500px" 
    style="border: none; border-radius: 12px; margin-top: 16px;"
    allowfullscreen>
</iframe>


<div class="evidence-block" markdown="1">


