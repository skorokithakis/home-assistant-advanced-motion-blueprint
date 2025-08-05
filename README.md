# Advanced Motion Activated Lights Blueprint

This Home Assistant blueprint automatically controls lights based on motion detection during specific time periods - from before sunset until a configured end time.

## Features

- Turns lights on when motion is detected
- Only activates during configurable hours (from X hours before sunset until a specified end time)
- Automatically turns lights off after a configurable period of no motion
- Tracks whether lights were turned on by the automation to avoid interfering with manual control
- Supports both lights and switches

## Prerequisites

This workflow needs a helper, but Home Assistant will prompt you to create one in the blueprint UI if you don't already have one.

## Installation

1. Copy the `advanced-motion-blueprint.yaml` file to your Home Assistant blueprints folder
2. Reload automations or restart Home Assistant
3. Go to Settings → Automations & Scenes
4. Click "Create Automation"
5. Select "Advanced motion activated lights" from the blueprint list

## Configuration

When creating an automation from this blueprint, you'll need to configure:

- **Motion sensor**: The binary sensor that detects motion
- **Lights or switches**: The light or switch entities to control
- **Turn off delay**: How many minutes to wait after motion stops before turning off lights (1-60 minutes, default: 5)
- **Hours before sunset**: When to start responding to motion relative to sunset (0-4 hours, default: 1)
- **End time**: When to stop responding to motion (default: 03:00 AM)
- **Tracking helper**: The input boolean helper you created earlier

## How It Works

The automation activates lights when:
1. Motion is detected
2. The lights are currently off
3. The current time is either:
   - Between the configured hours before sunset and midnight, OR
   - Between midnight and the configured end time

The automation turns lights off when:
1. No motion has been detected for the configured delay period
2. The automation was responsible for turning the lights on (tracked via the helper)

## Example Use Cases

- **Evening hallway lighting**: Set to activate 1 hour before sunset until 2 AM
- **Bathroom night light**: Set to activate 2 hours before sunset until 6 AM
- **Garage lighting**: Set to activate at sunset until midnight with a 10-minute turn-off delay

## Notes

- The automation uses single mode, meaning only one instance can run at a time
- Manual control is preserved - if you turn lights on manually, the automation won't turn them off
- The tracking helper ensures the automation only turns off lights it turned on
