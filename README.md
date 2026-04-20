# Reolink Notify

A Home Assistant blueprint that sends rich camera notifications to iOS and Android devices when Reolink sensors trigger. No snapshot files are saved to disk — images are fetched live via the authenticated camera proxy.

Based on [ha-reolink-rich-notifications](https://github.com/jterrace/ha-reolink-rich-notifications) by jterrace, extended with iOS support and a file-free image delivery approach.

## Features

- Supports **iOS** and **Android** independently — configure one or both
- **iOS**: single notification with a live camera attachment; tap opens the camera feed in the HA app
- **Android**: single notification with the camera image via the authenticated camera proxy
- Handles multiple sensor types per camera (motion, person, vehicle, pet) — only one notification fires per camera per retrigger window
- Configurable retrigger delay to suppress duplicate alerts (default: 20 seconds)

## Prerequisites

- [Reolink integration](https://www.home-assistant.io/integrations/reolink/) installed and configured
- [Home Assistant Companion app](https://companion.home-assistant.io/) on iOS and/or Android

## Import

[![Open your Home Assistant instance and show the blueprint import dialog with this blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheLeftMoose%2Fha-reolink-notify%2Fblob%2Fmain%2Freolink_rich_notifications.yaml)

Or import manually via **Settings → Automations → Blueprints → Import Blueprint** and paste the raw URL of `reolink_rich_notifications.yaml`.

## Inputs

## Known Limitations

- **iOS tap behavior**: Tapping a notification opens the HA companion app to the default dashboard. iOS does not support opening a specific entity or camera feed directly from a notification tap.

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| iOS Devices to Notify | Apple devices running the HA companion app | — |
| Android Devices to Notify | Android devices running the HA companion app | — |
| Trigger Sensors | Reolink binary sensors to monitor | — |
| Retrigger Delay | Minimum time between notifications for the same camera | 20 seconds |
