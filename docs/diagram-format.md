{
  "version": 1,
  "author": "SyncSense Team",
  "editor": "wokwi",
  "parts": [
    { "type": "board-esp32-devkit-v1", "id": "esp32", "top": 80, "left": 80, "rotate": 0, "attrs": {} },
    { "type": "gps-module", "id": "gps1", "top": 80, "left": 480, "rotate": 0, "attrs": { "label": "NEO-6M GPS" } },
    { "type": "ic-generic", "id": "sim800l", "top": 280, "left": 480, "attrs": { "label": "SIM800L GSM" } },
    { "type": "ic-generic", "id": "mcp1700", "top": 380, "left": 250, "attrs": { "label": "MCP1700 Regulator (3.3V)" } },
    { "type": "battery", "id": "battery1", "top": 480, "left": 250, "attrs": { "label": "Li-Po 3.7V Battery" } },
    { "type": "button", "id": "button1", "top": 50, "left": 300, "attrs": { "label": "Panic Button" } },
    { "type": "piezo", "id": "piezo1", "top": 50, "left": 150, "attrs": { "label": "Piezo Sensor" } }
  ],
  "connections": [
    ["esp32:3V3", "gps1:VCC", "red", ["v-30", "h80"]],
    ["esp32:GND.1", "gps1:GND", "black", ["h80"]],
    ["esp32:TX2", "sim800l:RX", "green", ["h60"]],
    ["esp32:RX2", "sim800l:TX", "orange", ["h70"]],
    ["esp32:GND.1", "sim800l:GND", "black", ["h60"]],
    ["battery1:positive", "sim800l:VCC", "red", ["v-30", "h40"]],
    ["battery1:positive", "mcp1700:VIN", "red", ["v-20", "h-30"]],
    ["mcp1700:VOUT", "esp32:3V3", "red", ["h-40"]],
    ["mcp1700:GND", "battery1:negative", "black", ["h-30"]],
    ["esp32:GND.2", "battery1:negative", "black", ["h-50"]],
    ["esp32:GPIO25", "button1:1.l", "blue", ["h-40"]],
    ["button1:2.l", "esp32:3V3", "red", ["h-40"]],
    ["esp32:GPIO35", "piezo1:signal", "blue", ["v-20"]],
    ["piezo1:ground", "esp32:GND.2", "black", ["v40"]],
    ["gps1:TX", "esp32:GPIO34", "green", ["h-60"]],
    ["gps1:RX", "esp32:GPIO33", "orange", ["h-60"]],
    ["esp32:GND.2", "gps1:GND", "black", ["v60"]]
  ],
  "dependencies": {}
}
---
title: diagram.json File Format
sidebar_label: diagram.json
description: Reference guide for the `diagram.json` file for Wokwi simulation projects. This file defines components, their attributes, layout positions, and the connections between them.
keywords: [diagram.json, connections, wiring, component layout, virtual hardware, ESP32, STM32, Arduino, wire placement, simulation diagram, simulation editor]
---

Each simulation project contains a diagram.json file. This file defines the components
that will be used for the simulation, their properties, and the connections between the
components.

## File structure

The diagram file is a JSON file with several sections. The basic file structure is as
follows:

```json
{
  "version": 1,
  "author": "Uri Shaked",
  "editor": "wokwi",
  "parts": [],
  "connections": []
}
```

`"version"` is always 1, `"author"` is the name of the person who created the
file, and `"editor"` is the name of the application that was used to edit the
file ("wokwi").

In addition, you can add a `"serialMonitor"` section to [configure the Serial Monitor](guides/serial-monitor#configuring-the-serial-monitor).

## Parts

The `"parts"` section defines the list of components in the simulation.
It's an array of objects with the following properties:

| Name   | Type    | Description                                     |
| ------ | ------- | ----------------------------------------------- |
| id     | string  | the unique identifier of the part (e.g. "led1") |
| type   | string  | the type of the part (e.g. "wokwi-led")         |
| left   | number  | x screen coordinate (in pixels)                 |
| top    | number  | y screen coordinate (in pixels)                 |
| attrs  | object  | part attributes (e.g. "color" for wokwi-led)    |
| rotate | number  | rotation in degress (e.g. 90)                   |
| hide   | boolean | if true, the part won't be visible              |

`id` and `type` are required, the other fields are optional.

For example, here's how you define a red LED called `"led1"` at position (x=100, y=50):

```json
{
  "id": "led1",
  "type": "wokwi-led",
  "left": 100,
  "top": 50,
  "attrs": {
    "color": "red"
  }
}
```

:::warning
Each part must have a unique "id" property. If two parts have the same "id",
the simulation may not function correctly.
:::

A partial list of part types (e.g. [wokwi-led](parts/wokwi-led)) can be found under the "Diagram Reference" section of this guide. We're currently working to expand this list. Meanwhile, some of the parts are also documented at [Wokwi Elements](https://elements.wokwi.com).

If your simulation project contains code, the diagram should include a microcontroller part that will execute your code. The following microcontrollers are currently supported:

- [`wokwi-attiny85`](parts/wokwi-attiny85) - ATtiny85
- [`wokwi-arduino-nano`](parts/wokwi-arduino-nano) - Arduino Nano
- [`wokwi-arduino-mega`](parts/wokwi-arduino-mega) - Arduino Mega 2560
- [`wokwi-arduino-uno`](parts/wokwi-arduino-uno) - Arduino Uno R3
- [`wokwi-pi-pico`](parts/wokwi-pi-pico) - Raspberry Pi Pico
- `board-esp32-devkit-c-v4` - ESP32 (official devkit)
- `wokwi-esp32-devkit-v1` - ESP32 (unofficial devkit)
- `board-esp32-c3-devkitm-1` - ESP32-C3
- `board-esp32-c3-rust-1` - ESP32-C3
- `board-esp32-c6-devkitc-1` - ESP32-C6
- `board-esp32-h2-devkitm-1` - ESP32-H2
- `board-esp32-s2-devkitm-1` - ESP32-S2
- [`board-franzininho-wifi`](parts/board-franzininho-wifi) - ESP32-S2
- `board-esp32-s3-devkitc-1` - ESP32-S3
- `board-esp32-p4-preview` - ESP32-P4
- [`board-st-nucleo-c031c6`](parts/board-st-nucleo-c031c6) - STM32 Nucleo-64 with STM32C031C6 MCU
- [`board-st-nucleo-l031k6`](parts/board-st-nucleo-l031k6) - STM32 Nucleo-32 with STM32L031K6 MCU
- `board-xiao-esp32-c3` - ESP32-C3
- `board-xiao-esp32-c6` - ESP32-C6
- `board-xiao-esp32-s3` - ESP32-S3

:::tip
Instead of manually specifying the left/top coordinates for each item, you
can drag them with the mouse to the desired position.
:::

## Connections

The `"connections"` section defines how the parts are connected. Each connection is an array with four
items:

- The source component id and pin name, separated by a colon. e.g. `partId:pinName`
- The target component id and pin name
- The color of the wire (or an empty string to hide the wire)
- A list of instructions how to place the wire, as an array of strings (optional)

For example, the following definition will connect the A (anode) pin of `led1`
to pin 13 of the `uno` part:

```json
  ["led1:A", "uno:13", "green", []],
```

You can find the name of a component pin by moving the mouse over it.

### Wire placement mini-language

Each item in the `"connections"` section can specify a list of instructions
how to draw the lines for the wire. Wires always go in straight lines, either
horizontally or vertically, and never diagonally.

There are three instructions:

- "v" followed by a number of pixels: move vertically (up/down)
- "h" followed by a number of pixels: move horizontally (left/right)
- "\*" can appear only once. All the instructions that appear before the "\*"
  apply to the source pin, and the instructions that appear after it apply
  to the target pins.

For example:

```json
["v10", "h5", "*", "v-15", "h10"]
```

The "v10" will move 10 pixels down from the source pin, then "h5" will move
five pixels the right.

The instructions that appear after the "\*" are applied in reverse order: "h10" will
move 10 pixels right of the target pin, then "v-15" will move 15 pixels up.

Finally, the simulator will connect the two ends of the wire with a combination
of horizontal and a vertical wire that cover the remaining distance, as necessary.

### Wire placement animation

If you are a visual learner, you may find the following GIF animation useful.
The animation was created by Steve Sigma.

![diagram.json wire placement mini language](diagram-format-connections.gif)
