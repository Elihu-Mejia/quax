# Quax Shader

This project contains a GLSL fragment shader (`quax.glsl`) that renders a procedural, glitch-style sci-fi HUD (Heads-Up Display) effect.

## Overview

The shader creates a dynamic, chaotic visual utilizing aggressive coordinate distortion and procedural noise. It simulates a digital interface that is booting up, glitching, or experiencing interference.

## Features

*   **Coordinate Distortion:** Uses tangent-based transformations and time-dependent rotation matrices to create warping and grid-like artifacts (`uv = rot(tilt) * tan(uv)`).
*   **Procedural Noise:** Generates digital block noise and background patterns using custom hash functions.
*   **HUD Elements:**
    *   **Boot Sequence:** A fade-in effect driven by a 2-second time cycle.
    *   **Target Reticle:** A dynamic element that attempts to lock onto a position, moving erratically based on sine waves.
    *   **Color Palette:** Primarily uses deep Cyan and high-intensity Red.
*   **Post-Processing:**
    *   **Scanlines:** A vertical sine-wave pattern overlay to simulate CRT screens.
    *   **Blackout:** A momentary blackout effect at the beginning of every cycle (`cycle < 0.2`).

## Uniforms

The shader requires the following uniforms to be passed from the host application:

*   `uniform float time;` - The elapsed time in seconds.
*   `uniform vec2 resolution;` - The viewport resolution in pixels (width, height).

## Technical Details

*   **Version:** GLSL 1.50 (`#version 150`).
*   **Cycle:** The animation loops based on a modulo of the time (`mod(t, 2.0)`).
*   **Math:** Heavily relies on trigonometric functions (`tan`, `sin`, `cos`) for both movement and coordinate mapping.
    *   *Note:* The rotation matrix `rot()` includes `tan(a * time)`, creating non-linear distortion rather than standard Euclidean rotation.

## Usage

This file is intended to be loaded by a GLSL-compatible renderer (such as a game engine, Shadertoy-style wrapper, or custom OpenGL/Vulkan app) that supplies the required `time` and `resolution` uniforms.