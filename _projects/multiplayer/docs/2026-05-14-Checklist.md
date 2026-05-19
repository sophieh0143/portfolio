---
layout: post
codemirror: true
title: Checklist
permalink: /portfolio-checklist
hide: true
show_reading_time: false
author: Sophie Haas
---

# Progress and Code Updates Checklist

<style>
    /* Prevent the main container from clipping the puffy cloud tops */
    .cloud-grid {
        display: flex;
        flex-wrap: wrap;
        gap: 60px 30px; /* Increased vertical gap for the lumps */
        padding: 60px 20px;
        justify-content: center;
        background-color: transparent;
    }

    .cloud-btn {
        text-decoration: none !important;
        position: relative;
        width: 210px;
        height: 70px;
        background: #ffffff !important; /* Force white background */
        border-radius: 100px;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: all 0.3s ease;
        /* Using filter: drop-shadow instead of box-shadow to outline the bumps too */
        filter: drop-shadow(8px 8px 0px rgba(0,0,0,0.15));
        cursor: pointer;
        border: none;
        margin-top: 40px; /* Give space for the bumps above */
    }

    /* The Text */
    .cloud-btn span {
        color: #333333 !important;
        font-family: "Arial Black", Gadget, sans-serif;
        font-weight: bold;
        font-size: 12px;
        text-align: center;
        z-index: 10; /* Ensure text is above all cloud bumps */
        text-transform: uppercase;
        letter-spacing: 0.5px;
        position: relative;
    }

    /* Left Lump */
    .cloud-btn::before {
        content: '';
        position: absolute;
        background: #ffffff !important; /* Force white */
        width: 90px;
        height: 90px;
        border-radius: 50%;
        top: -45px;
        left: 25px;
        z-index: 1;
    }

    /* Right Lump */
    .cloud-btn::after {
        content: '';
        position: absolute;
        background: #ffffff !important; /* Force white */
        width: 110px;
        height: 110px;
        border-radius: 50%;
        top: -60px;
        right: 20px;
        z-index: 1;
    }

    /* Hover animation */
    .cloud-btn:hover {
        transform: translateY(-10px);
        filter: drop-shadow(12px 12px 0px rgba(52, 152, 219, 0.3));
    }

    .cloud-btn:hover span {
        color: #3498db !important;
    }
</style>

## 🏗️ Object-Oriented Programming
> **The foundation of the game engine.** Click a button to see class architecture and inheritance.

<div class="cloud-grid">
    <a href="/portfolio/OOP-checklist#writing-classes" class="cloud-btn">
        <span>Writing Classes</span>
    </a>
    <a href="/portfolio/OOP-checklist#methods-params" class="cloud-btn">
        <span>Methods & Parameters</span>
    </a>
    <a href="/portfolio/OOP-checklist#instantiation" class="cloud-btn">
        <span>Instantiation & Objects</span>
    </a>
    <a href="/portfolio/OOP-checklist#inheritance" class="cloud-btn">
        <span>Inheritance (Basic)</span>
    </a>
    <a href="/portfolio/OOP-checklist#overriding" class="cloud-btn">
        <span>Method Overriding</span>
    </a>
    <a href="/portfolio/OOP-checklist#super-calls" class="cloud-btn">
        <span>Constructor Chaining</span>
    </a>
</div>

---

## ⚙️ Control Structures
> **The logic driving the game.** Click to see how the game handles loops, conditions, and complex state logic.

<div class="cloud-grid">
    <a href="/portfolio/ControlStructures-checklist#iteration" class="cloud-btn">
        <span>Iteration (Loops)</span>
    </a>
    <a href="/portfolio/ControlStructures-checklist#conditionals" class="cloud-btn">
        <span>Conditionals</span>
    </a>
    <a href="/portfolio/ControlStructures-checklist#nested-conditions" class="cloud-btn">
        <span>Nested Conditions</span>
    </a>
</div>

---

## 📊 Data Types
> **The variables managing game state.** Click a button to review how positions, paths, and level parameters are stored.

<div class="cloud-grid">
    <a href="/portfolio/DataTypes-checklist#numbers" class="cloud-btn">
        <span>Numbers</span>
    </a>
    <a href="/portfolio/DataTypes-checklist#strings" class="cloud-btn">
        <span>Strings</span>
    </a>
    <a href="/portfolio/DataTypes-checklist#booleans" class="cloud-btn">
        <span>Booleans</span>
    </a>
    <a href="/portfolio/DataTypes-checklist#arrays" class="cloud-btn">
        <span>Arrays</span>
    </a>
    <a href="/portfolio/DataTypes-checklist#objects-json" class="cloud-btn">
        <span>Objects (JSON)</span>
    </a>
</div>

---

## ⚡ Operators
> **The math and logic expressions.** Click a button to review physics equations, path concatenations, and compound conditions.

<div class="cloud-grid">
    <a href="/portfolio/Operators-checklist#mathematical" class="cloud-btn">
        <span>Mathematical</span>
    </a>
    <a href="/portfolio/Operators-checklist#string-ops" class="cloud-btn">
        <span>String Operations</span>
    </a>
    <a href="/portfolio/Operators-checklist#boolean-expressions" class="cloud-btn">
        <span>Boolean Expressions</span>
    </a>
</div>

---

## 🔌 Input/Output & API
> **The connections inside and outside the game.** Click a button to review hardware controls, canvas rendering configurations, and backend API integration.

<div class="cloud-grid">
    <a href="/portfolio/IO-checklist#keyboard-input" class="cloud-btn">
        <span>Keyboard Input</span>
    </a>
    <a href="/portfolio/IO-checklist#canvas-rendering" class="cloud-btn">
        <span>Canvas Rendering</span>
    </a>
    <a href="/portfolio/IO-checklist#gameenv-config" class="cloud-btn">
        <span>GameEnv Config</span>
    </a>
    <a href="/portfolio/IO-checklist#api-integration" class="cloud-btn">
        <span>API Integration</span>
    </a>
    <a href="/portfolio/IO-checklist#async-io" class="cloud-btn">
        <span>Asynchronous I/O</span>
    </a>
    <a href="/portfolio/IO-checklist#json-parsing" class="cloud-btn">
        <span>JSON Parsing</span>
    </a>
</div>

---

## 📝 Documentation
> **The guide to understanding the workspace.** Click a button to view code documentation standards, lesson summaries, and architectural highlights.

<div class="cloud-grid">
    <a href="/portfolio/Doc-checklist#code-comments" class="cloud-btn">
        <span>Code Comments</span>
    </a>
    <a href="/portfolio/Doc-checklist#mini-lesson" class="cloud-btn">
        <span>Mini-Lesson Docs</span>
    </a>
    <a href="/portfolio/Doc-checklist#code-highlights" class="cloud-btn">
        <span>Code Highlights</span>
    </a>
</div>

---

## 🔍 Debugging
> **The inspection tools under the hood.** Click a button to track down performance runtime issues, network data, and visual hitbox perimeters.

<div class="cloud-grid">
    <a href="/portfolio/Debugging-checklist#console-debugging" class="cloud-btn">
        <span>Console Debugging</span>
    </a>
    <a href="/portfolio/Debugging-checklist#hitbox-visualization" class="cloud-btn">
        <span>Hit Box Visuals</span>
    </a>
    <a href="/portfolio/Debugging-checklist#source-debugging" class="cloud-btn">
        <span>Source Debugging</span>
    </a>
    <a href="/portfolio/Debugging-checklist#network-debugging" class="cloud-btn">
        <span>Network Debugging</span>
    </a>
    <a href="/portfolio/Debugging-checklist#application-debugging" class="cloud-btn">
        <span>Application Storage</span>
    </a>
    <a href="/portfolio/Debugging-checklist#element-inspection" class="cloud-btn">
        <span>Element Inspection</span>
    </a>
</div>

---

## 🧪 Testing & Verification
> **The stability checkpoints.** Click a button to review direct gameplay loops, live API verification, and structural try/catch edge case handling.

<div class="cloud-grid">
    <a href="/portfolio/Testing-checklist#gameplay-testing" class="cloud-btn">
        <span>Gameplay Testing</span>
    </a>
    <a href="/portfolio/Testing-checklist#integration-testing" class="cloud-btn">
        <span>Integration Testing</span>
    </a>
    <a href="/portfolio/Testing-checklist#api-error-handling" class="cloud-btn">
        <span>API Error Handling</span>
    </a>
</div>