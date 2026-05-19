---
layout: post 
title: Testing and Verification Blog
permalink: /Testing-checklist
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

# 📝 Testing Checklist (most of this you can find everywhere in this checklist)

---

## Gameplay Testing

You can find some of the games my team and I coded, along with a google from we used for a short period of time for our peers to review our work in class in order to make imporvements to our game(s)!

<h2 style="font-family: sans-serif; color: #3498db; border-left: 4px solid #3498db; padding-left: 12px; text-transform: none; letter-spacing: 0;">A few playable levels for testing</h2>
<div class="grid-container">
    <a href="https://sprintingsnails.opencodingsociety.com/gamify/timmyworld" class="race-button">
        <div class="btn-red">Level 1 (Timmy)</div>
    </a>
    <a href="https://sprintingsnails.opencodingsociety.com/gamify/timmycounter.html" class="race-button">
        <div class="btn-red">Level 2 (Maze)</div>
    </a>
    <a href="https://sprintingsnails.opencodingsociety.com/gamify/battle" class="race-button">
        <div class="btn-red">Last Level (boss)</div>
    </a>
    <a href="https://docs.google.com/forms/d/e/1FAIpQLSfo49g4uz0ujn8IB16fkf5t2Xgaj_j35Jvi1HmGzaoMg8Ft2Q/viewform?usp=publish-editor" class="race-button">
        <div class="btn-red">Gameplay Feedback Google Form!</div>
    </a>
</div>

---

<div class="oop-section" markdown="1" id="code-highlights">

## Integration Testing

We used many documents to keep track of our work: blogs, troubleshooting guides, backup repositories, slack, messages, google slides, google forms, and more.

<h2> style="font-family: sans-serif; color: #3498db; border-left: 4px solid #3498db; padding-left: 12px; text-transform: none; letter-spacing: 0;">Ideation Documents / Repo Access</h2>
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
    <a href="https://docs.google.com/presentation/d/1d0nl5TecVl7yp6SYSlN1drLuNwqGP_9UVkKysGhV--8/edit?slide=id.p#slide=id.p" class="race-button">
        <div class="btn-red">Timmy Ideation Slide Deck</div>
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


---

## API Error Handling

<h2> style="font-family: sans-serif; color: #3498db; border-left: 4px solid #3498db; padding-left: 12px; text-transform: none; letter-spacing: 0;">Ideation Documents / Repo Access</h2>

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
     <a href="https://docs.google.com/forms/d/e/1FAIpQLSfo49g4uz0ujn8IB16fkf5t2Xgaj_j35Jvi1HmGzaoMg8Ft2Q/viewform?usp=publish-editor" class="race-button">
        <div class="btn-red">Gameplay Feedback Google Form!</div>
    </a>
</div>