# moab-one-vibe-coder

**Vibe code your hardware projects with MOAB-ONE** — describe what you want to build, and your AI agent designs the complete implementation: MCU selection, peripheral wiring, jumper configuration, and working Arduino firmware.

## What is this?

An [Agent Skill](https://agentskills.io) that turns any AI coding agent into a hardware prototyping assistant for the [MOAB-ONE](https://github.com/knu2/moab-one-vibe-coder) universal carrier board. Just describe your project idea in plain language and the agent will:

1. **Select the right MCU board** — from Feather, Nano, Pico, XIAO/QT Py, or RPi Zero
2. **Map peripherals to connectors** — Stemma QT, Qwiic, Grove, mikroBUS, Arduino Shield
3. **Configure jumpers and power** — exact jumper positions for your setup
4. **Generate complete Arduino firmware** — with correct pin definitions and library includes
5. **Guide physical assembly and testing** — step-by-step build and verification

## Install

This skill follows the open [Agent Skills](https://agentskills.io) specification and works across multiple AI coding agents.

### Claude Code

```bash
npx @anthropic-ai/skills add github:knu2/moab-one-vibe-coder
```

### GitHub Copilot (VS Code Agent Mode)

```bash
# Option A: Use the skills CLI
npx @anthropic-ai/skills add github:knu2/moab-one-vibe-coder

# Option B: Manual install
git clone https://github.com/knu2/moab-one-vibe-coder.git /tmp/moab-skill
cp -r /tmp/moab-skill/moab-one-vibe-coder .agents/skills/
```

Then verify in VS Code: open Copilot Chat → Agent mode → type `/skills` → confirm `moab-one-vibe-coder` appears.

### Cursor

```bash
git clone https://github.com/knu2/moab-one-vibe-coder.git /tmp/moab-skill
mkdir -p .cursor/skills
cp -r /tmp/moab-skill/moab-one-vibe-coder .cursor/skills/
```

### Codex / Other Agents

```bash
# Clone and copy into your agent's skill directory
git clone https://github.com/knu2/moab-one-vibe-coder.git /tmp/moab-skill
cp -r /tmp/moab-skill/moab-one-vibe-coder <your-agent-skills-directory>/
```

### Supported Agents

This skill works with any agent that supports the [Agent Skills](https://agentskills.io) specification:
- Claude Code
- GitHub Copilot (VS Code)
- Cursor
- Codex
- Gemini CLI
- Any agent that reads SKILL.md files

## Usage

After installing, just describe your project to the agent:

> "I want to build a weather station that measures temperature, humidity, and barometric pressure and displays results on a small OLED screen"

> "Help me build a robot car with 4 DC motors and an IR line follower sensor"

> "I need a Bluetooth-connected environmental monitor that sends data to my phone"

The agent will automatically activate the skill and produce a complete project plan with hardware selection, wiring diagram, Arduino firmware, assembly instructions, and a testing checklist.

## What is MOAB-ONE?

MOAB-ONE is a universal carrier and expansion board for rapid embedded prototyping. It's a motherboard for microcontrollers that lets you plug in any popular MCU development board and connect sensors, actuators, displays, and communication modules without breadboard wiring.

**Supported MCU form factors:**
- Adafruit Feather
- Arduino Nano
- Raspberry Pi Pico
- Seeed Studio XIAO / Adafruit QT Py
- Raspberry Pi Zero

**Peripheral interfaces:**
- 4× mikroBUS™ sockets (1800+ Click boards)
- Arduino Uno Shield socket
- Stemma QT / Qwiic (I2C, JST-SH)
- Stemma / Gravity (I2C, UART, Analog, JST-PH)
- Grove (multi-signal)
- Display header (TFT-LCD, OLED)
- 30×13 prototyping area

## Skill Contents

```
moab-one-vibe-coder/
├── SKILL.md                          # Main agent instructions
├── references/
│   ├── pin-mappings.md               # MCU pin maps for all 5 form factors
│   ├── peripherals-and-connectors.md # All connector types & jumper configs
│   ├── power-configurations.md       # Power source options & jumper settings
│   ├── on-board-peripherals.md       # LEDs, buttons, pot, buzzer details
│   └── example-projects.md           # 3 fully worked project examples
└── assets/
    └── project-plan-template.md      # Output template for project plans
```

## License

Apache-2.0 — see [LICENSE](LICENSE)

## Author

Kenneth Pinpin ([@knu2](https://github.com/knu2))
