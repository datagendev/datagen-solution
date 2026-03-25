# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This repository is a knowledge base and skills collection for DataGen (datagen.dev). It helps people understand what DataGen is, how to use it, and what agents they can build with it. It is NOT a code project -- there is no build system, test suite, or application code.

## What Is DataGen

DataGen is a deployment platform for Claude Code agents. Users build agents locally in Claude Code, push to GitHub, and DataGen handles deployment with webhook URLs, cron scheduling, MCP tool connections, and Slack/email communication channels. The core value proposition is "human-as-a-tool" -- agents run autonomously on external triggers and only reach humans for judgment calls.

Key primitives: one-click deploy to webhook/cron, MCP gateway for tool portability (local to production), built-in Slack and email channels with subscriber control.

See `datagen.md` for the full product description extracted from the landing page.

## Repository Structure

- `datagen.md` -- "What is DataGen" reference document (product overview, features, pricing, getting started)
- `.claude/skills/` -- Claude Code skills that use DataGen context
  - `linkedin-agent-recommender/` -- Skill that takes a LinkedIn URL, classifies the person into an archetype, and recommends a specific DataGen agent. Archetypes are in `archetypes/` subdirectory (A through G: agency owner, revops, sales rep, outbound specialist, consultant, founder, engineer).

## Working in This Repo

- Content is markdown-only. When editing, preserve the existing tone: practical, specific, grounded in real workflows rather than abstract features.
- The linkedin-agent-recommender skill follows a specific output format (see SKILL.md). When modifying archetypes, maintain the structure: who they are, manual loop, agent loop, build instructions, first-event line.
- `datagen.md` is sourced from the landing page at `DataGen/leadgen/app/src/landing-page/`. If the landing page changes upstream, this file should be updated to match.

## DataGen MCP Tools Available

The local settings grant access to DataGen MCP tools: `searchTools`, `executeTool`, `searchCustomTools`. These can be used to look up available integrations and test tool execution when building new skill content or agent recommendations.
