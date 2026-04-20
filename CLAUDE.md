# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

This repository clones and extends the [`ha-reolink-rich-notifications`](https://github.com/jterrace/ha-reolink-rich-notifications/blob/main/ha_reolink_rich_notifications.yaml) blueprint by jterrace. **Scope is strictly Reolink camera notifications** — no other device types or automation categories.

The upstream blueprint:
- Triggers on Reolink binary sensor `off → on` transitions
- Identifies the associated camera entity and captures a snapshot (preferring fluent/clear quality)
- Sends an initial text notification then a follow-up with the snapshot attached (Android mobile app)
- Runs in `parallel` mode (max 100) to handle simultaneous camera triggers
- Deduplicates via a configurable retrigger delay (default 20 s) per camera

Work in this repo should extend that baseline — e.g. adding iOS support, additional notification channels, doorbell-specific inputs, or configurable snapshot quality — without changing the core notification scope.

## Home Assistant Blueprint Development

Blueprints live in `blueprints/` (by convention) as `.yaml` files. They use the `blueprint:` key at the top level with `input:` for user-configurable fields.

**Always invoke the `home-assistant-best-practices` skill before writing or editing any automation, blueprint, or script YAML.** The skill enforces native HA constructs over templates and correct automation modes.

Key rules from that skill:

- Prefer `numeric_state` conditions/triggers over `{{ states(...) | float > X }}` templates
- Use `mode: restart` for motion-based lights (not `single`)
- Use `entity_id` in targets, not `device_id` (breaks on device re-add)
- Use `wait_for_trigger` instead of `wait_template` for event-driven waits
- For Reolink/ZHA button events: use `event` trigger with `device_ieee`

Detailed pattern references are in `.agents/skills/home-assistant-best-practices/references/`:
- `automation-patterns.md` — triggers, conditions, modes, wait actions
- `helper-selection.md` — when to use built-in helpers vs template sensors
- `template-guidelines.md` — when templates ARE appropriate
- `device-control.md` — service calls, Zigbee patterns, entity vs device targeting
- `safe-refactoring.md` — impact analysis before renaming entities or replacing helpers
- `examples.yaml` — compound examples

## Testing Blueprints

Blueprints cannot be unit tested locally. Validate by:
1. Importing the blueprint into a local HA dev instance (`Settings → Automations → Blueprints → Import`)
2. Creating an automation from the blueprint and checking for validation errors in the HA UI
3. Reviewing the automation trace after a test trigger
