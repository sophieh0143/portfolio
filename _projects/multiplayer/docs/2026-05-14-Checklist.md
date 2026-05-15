---
layout: post
codemirror: true
title: Checklist
permalink: /portfolio-checklist
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
    <a href="{{site.baseurl}}/OOP-checklist#writing-classes" class="cloud-btn">
        <span>Writing Classes</span>
    </a>
    <a href="{{site.baseurl}}/OOP-checklist#methods-params" class="cloud-btn">
        <span>Methods & Parameters</span>
    </a>
    <a href="{{site.baseurl}}/OOP-checklist#instantiation" class="cloud-btn">
        <span>Instantiation & Objects</span>
    </a>
    <a href="{{site.baseurl}}/OOP-checklist#inheritance" class="cloud-btn">
        <span>Inheritance (Basic)</span>
    </a>
    <a href="{{site.baseurl}}/OOP-checklist#overriding" class="cloud-btn">
        <span>Method Overriding</span>
    </a>
    <a href="{{site.baseurl}}/OOP-checklist#super-calls" class="cloud-btn">
        <span>Constructor Chaining</span>
    </a>
</div>

---

## ⚙️ Control Structures
> **The logic driving the game.** Click to see how the game handles loops, conditions, and complex state logic.

<div class="cloud-grid">
    <a href="{{site.baseurl}}/ControlStructures-checklist#iteration" class="cloud-btn">
        <span>Iteration (Loops)</span>
    </a>
    <a href="{{site.baseurl}}/ControlStructures-checklist#conditionals" class="cloud-btn">
        <span>Conditionals</span>
    </a>
    <a href="{{site.baseurl}}/ControlStructures-checklist#nested-conditions" class="cloud-btn">
        <span>Nested Conditions</span>
    </a>
</div>