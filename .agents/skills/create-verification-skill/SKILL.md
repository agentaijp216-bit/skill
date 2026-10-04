---
name: create-verification-skill
description: Use when a project lacks a reliable way for an agent to prove UI, CLI, API, service, or application behavior; create a project-local Codex skill and feature map for repeatable verification.
disable-model-invocation: true
---

# Create a verification skill

Create a project-local skill that teaches a future agent how to run the real application, exercise user-visible behavior, and capture evidence. This is adapted from Lauren Tan's pstack verification workflow for Codex.

## 1. Inspect the repository first

Answer these from the codebase and ask the user only about details the repository cannot establish:

- **Surface:** What does a user interact with: web UI, CLI/TUI, desktop, API, mobile app, or library? Choose the main surface and record other important ones.
- **Launch:** How does the project start locally? Prefer documented commands and existing scripts. Record ports, environment variables, seed data, and authentication requirements.
- **Drive:** How can an agent interact with it? Prefer existing test or automation harnesses, Playwright/Cypress, PTY helpers, curl-able routes, or a debug protocol. Then document a suitable supported method.
- **Observe:** What evidence can be captured: screenshots, terminal output, response bodies, logs, exit codes, or persisted state?
- **Isolate:** Can multiple instances run safely side by side? Record data directories, ports, profiles, and cleanup boundaries.

Do not assume a command, selector, or interface. Derive it from the repository. Do not change application code just to create the skill. If the app cannot currently launch, document the blocker and avoid writing instructions that imply it works.

## 2. Create the project-local skill

Write `.agents/skills/verify-<app>/SKILL.md` using Codex's project skill layout. Give it YAML frontmatter with `name: verify-<app>` and a description naming the project surface and when to use the skill.

Include these sections, filled with details confirmed from the repository:

- **Launch:** Exact startup command and readiness signal, plus teardown. For short-lived CLI/TUI work, explain how to build and run each drive in an isolated terminal session.
- **Doctor:** A read-only check for whether this instance is ready to drive: process, version, port ownership, and authentication as relevant.
- **Drive:** Real commands and stable UI selectors from this project. Prefer accessible labels, data attributes, prompt strings, and route paths over coordinates or tab order.
- **Evidence:** What to capture and where. Exercise real user flows, inspect observable side effects, and distinguish verified behavior from mocks or assumptions.
- **Cleanup:** Remove only processes and scratch state started by the verification run. Preserve evidence artifacts and name their location.
- **Helpers:** Include only scripts needed for repeatability, and document their exact invocation.

Keep the instructions short enough that a fresh agent can follow them without reading the entire source tree again.

## 3. Seed a feature map

Create `.agents/skills/verify-<app>/features/README.md` and one concise file for the most important user-facing features (start with three to five). For each feature, document:

- What it does from the user's point of view.
- How to reach it.
- How to exercise it using the documented harness.
- What observable result proves success.
- Relevant alternate, failure, cancel, empty, or persistence paths.

Keep the map in the repository so it can be updated alongside behavior changes. Use the feature-map example bundled under `references/feature-map-example/` as a shape guide, not as application facts.

## 4. Verify the generated instructions

Use existing project commands to verify the new skill once, if the application can be run safely in the current environment. Launch it, run the doctor check, exercise one mapped feature, save the promised evidence, and clean up only what this run created. Do not install tools or access accounts without authorization. If verification cannot be performed, state why and leave the generated skill marked as unverified.

When the application changes later, update the feature map and skill instructions in the same change where practical.
