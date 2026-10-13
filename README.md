# Stargazer Skills

Agent Skills from **Stargazer**, an astronomy chat agent. They follow the open
[Agent Skills](https://agentskills.io) format, so they work in Claude Code, Codex,
Cursor, Agno and other skill-aware agents.

## `sky-tour-guide`

A live, guided tour of what's in the sky **right now**, with directions anyone can
follow in a dark backyard:

> **5. Saturn** · look **east-southeast**, **2 fists up** (20°)
> A bright, steady golden "star"; planets don't twinkle like stars.
> 🔭 A small telescope shows its rings and Titan.

Astronomy tools answer in azimuth and altitude degrees. This skill teaches the agent to:

1. Take a sky snapshot (`astro_sky_today`): darkness, Moon phase, which planets are up.
2. Get positions for the Moon, naked-eye planets and ~14 bright stars and showpieces in **one**
   `astro_positions` call, using the coordinates in `references/bright-targets.md`.
3. Keep only objects at least 15° up, skip anything you can't see without a telescope, and
   pick 5–7 stops sorted by compass bearing so you turn smoothly from north through east, south and west.
4. Turn numbers into **compass directions** and **fists at arm's length** (1 fist ≈ 10°).

### Files

```
skills/sky-tour-guide/
├── SKILL.md                     workflow, translation rules, output format
└── references/bright-targets.md 24 bright stars, clusters, nebulae and galaxies with J2000 coordinates
```

No scripts and no network access of its own. It only uses the MCP tools below.

### Requirements

The skill drives the **CycleCalcs astronomy MCP server**, which provides the `astro_sky_today` and
`astro_positions` tools: `https://www.cyclecalcs.com/mcp` (Streamable HTTP; listed on
mcpservers.org). Without an API key it allows 50 calls a day per IP address; a free RapidAPI
key raises that (send it as an `X-Api-Key` header).

### Install

```bash
# 1. Install the skill (pick your agent when prompted)
npx skills add terminator349/stargazer-skills --skill sky-tour-guide

# 2. Claude Code example: connect the MCP server it needs
claude mcp add --transport http cyclecalcs https://www.cyclecalcs.com/mcp
```

Then ask: **"I'm outside right now. Give me a tour of the sky."** Say where you are, or let
your agent use a saved location.

### Using it in Agno (Python)

```python
from agno.agent import Agent
from agno.skills import LocalSkills, Skills
from agno.tools.mcp import MCPTools

cyclecalcs = MCPTools(transport="streamable-http", url="https://www.cyclecalcs.com/mcp")
await cyclecalcs.connect()
agent = Agent(model=..., tools=[cyclecalcs], skills=Skills(loaders=[LocalSkills("skills/")]))
```

## License

MIT © 2026 Jake Croft
