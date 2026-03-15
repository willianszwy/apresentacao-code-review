# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a standalone HTML presentation built with **Reveal.js** (v5.2.1) about an AI-driven code review workflow using Claude Code CLI. The entire presentation is a single file: `index.html`.

There is no build system, package manager, or test infrastructure. To view the presentation, open `index.html` directly in a browser.

## Architecture

- **Single file**: `index.html` (~840 lines) contains all HTML structure, embedded CSS, and Reveal.js slide content
- **CDN dependencies**: Reveal.js, Highlight.js, and the moon theme are loaded from CDNs — no local dependencies
- **Language**: Portuguese (pt-BR)
- **Inline SVG**: The Mermaid flowchart on slide 9 is pre-rendered as inline SVG (not generated at runtime)

## Slide Structure

Slides are defined as `<section>` elements inside `<div class="slides">`. Nested `<section>` elements create vertical slide stacks.

Key custom CSS classes used throughout:
- `.card`, `.card-grid` — styled info cards
- `.badge`, `.badge-critical/.badge-medium/.badge-low` — severity badges
- `.terminal` — terminal emulator mockup UI
- `.step-item` — numbered step layout

## Making Changes

- To add/edit slides: modify `<section>` blocks in `index.html`
- To update styles: edit the `<style>` block in `<head>`
- To update the flowchart diagram: edit the inline `<svg>` in slide 9 directly (it was originally a Mermaid diagram, now pre-rendered)
- Color palette: amber `#FFE600`, blue `#5ba4f5`, green `#2ecc7a`, red `#f5677a`
