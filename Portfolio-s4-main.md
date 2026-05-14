---
layout: post 
title: Sprint 4 Assessments 
permalink: /portfolio-s4-main
hide: true
show_reading_time: false
---

<style>
    @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&family=Bungee&family=JetBrains+Mono:wght@400;700&display=swap');

    /* Global Aesthetic */
    .post {
        background-color: #0d0d0f;
        color: #e0e0e0; 
        font-family: 'JetBrains Mono', monospace;
        padding: 40px 20px;
    }

    /* Top Navigation Bar for your Blogs & Checklist */
    .sprint-nav {
        display: flex;
        gap: 10px;
        margin-bottom: 40px;
        border-bottom: 2px solid #ff3131;
        padding-bottom: 20px;
    }

    .nav-item {
        flex: 1;
        background: rgba(255, 49, 49, 0.1);
        border: 1px solid #ff3131;
        color: white;
        text-align: center;
        padding: 10px;
        text-decoration: none;
        font-family: 'Orbitron';
        font-size: 0.8rem;
        text-transform: uppercase;
        transition: 0.3s;
    }

    .nav-item:hover {
        background: #ff3131;
        box-shadow: 0 0 15px #ff3131;
    }

    /* Headers */
    h2 {
        font-family: 'Bungee', cursive;
        color: #ff3131;
        text-transform: uppercase;
        letter-spacing: 2px;
        margin-top: 50px;
        font-size: 1.4rem;
        display: flex;
        align-items: center;
    }

    h2::before {
        content: "> ";
        margin-right: 10px;
    }

    /* Grid for Buttons */
    .grid-container {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
        gap: 15px;
        margin: 20px 0;
    }

    /* High-Tech Button Style */
    .race-button {
        text-decoration: none !important;
    }

    .btn-content {
        padding: 15px;
        border-radius: 4px;
        font-family: 'Orbitron', sans-serif;
        font-weight: 700;
        text-transform: uppercase;
        font-size: 0.85rem;
        text-align: center;
        color: white;
        transition: 0.2s;
        border: 1px solid rgba(255,255,255,0.1);
        clip-path: polygon(10% 0, 100% 0, 90% 100%, 0% 100%); /* Angled sides */
    }

    .race-button:hover .btn-content {
        transform: translateY(-3px);
        filter: brightness(1.2);
    }

    /* Color Palette */
    .btn-red { background: #c51d1d; box-shadow: 4px 4px 0px #5e0b0b; }
    .btn-orange { background: #d35400; box-shadow: 4px 4px 0px #873600; }
    .btn-dark { background: #1a1a1a; border: 1px solid #ff3131; box-shadow: 4px 4px 0px #000; }
    .btn-gold { background: #b8860b; box-shadow: 4px 4px 0px #7a5a07; }

    /* Checklist Section */
    .checklist-container {
        background: #16161a;
        border: 1px solid #333;
        padding: 20px;
        margin-top: 20px;
    }

    .check-row {
        display: flex;
        align-items: center;
        margin-bottom: 10px;
        font-size: 0.9rem;
    }

    .check-row input { margin-right: 10px; accent-color: #ff3131; }

</style>

<div class="sprint-nav">
    <a href="{{site.baseurl}}/portfolio-s4-main" class="nav-item">Sprint 4 Blog</a>
    <a href="{{site.baseurl}}/portfolio-s5-main" class="nav-item">Sprint 5 Blog</a>
    <a href="#checklist" class="nav-item">My Checklist</a>
</div>

<h2>Timmy's FunWorld - Game Levels</h2>
<div class="grid-container">
    <a href="https://sprintingsnails.opencodingsociety.com/gamify/timmyworld" class="race-button">
        <div class="btn-content btn-red">Level 1</div>
    </a>
    <a href="https://sprintingsnails.opencodingsociety.com/gamify/timmycounter.html" class="race-button">
        <div class="btn-content btn-red">Level 2</div>
    </a>
    <a href="https://sprintingsnails.opencodingsociety.com/gamify/timmyhooray" class="race-button">
        <div class="btn-content btn-red">Level 3</div>
    </a>
    <a href="https://sprintingsnails.opencodingsociety.com/gamify/battle" class="race-button">
        <div class="btn-content btn-red">Level 4</div>
    </a>
</div>

<h2>Issue & Development Blogs</h2>
<div class="grid-container">
    <a href="{{site.baseurl}}/Aoneblog" class="race-button">
        <div class="btn-content btn-orange">Adventure Blog</div>
    </a>
    <a href="{{site.baseurl}}/Atwoblog" class="race-button">
        <div class="btn-inner btn-orange">Maze Blog (Level 2)</div>
    </a>
</div>

<h2>Game Runners</h2> 
<p style="font-size: 0.8rem; color: #888;">Try out the levels below! Get the code and implement it into your gamebuilder.</p>

<div class="grid-container">
    <a href="{{site.baseurl}}/Aonerunner" class="race-button">
        <div class="btn-content btn-red">A1 Runner</div>
    </a>
    <a href="{{site.baseurl}}/Atworunner" class="race-button">
        <div class="btn-content btn-orange">A2 Runner</div>
    </a>
    <a href="{{site.baseurl}}/Athreerunner" class="race-button">
        <div class="btn-content btn-gold">A3 Crash Landing</div>
    </a>
</div>

<h2>Developer Tools</h2>
<div class="grid-container">
    <a href="{{site.baseurl}}/runners" class="race-button">
        <div class="btn-content btn-dark">Game Runner Examples</div>
    </a>
</div>

<h2 id="checklist">Mission Checklist</h2>
<div class="checklist-container">
    <div class="check-row"><input type="checkbox"> Complete Sprint 4 Blog documentation</div>
    <div class="check-row"><input type="checkbox"> Link all game levels and runners</div>
    <div class="check-row"><input type="checkbox"> Verify all external links work</div>
    <div class="check-row"><input type="checkbox"> Update Sprint 5 Plan</div>
</div>