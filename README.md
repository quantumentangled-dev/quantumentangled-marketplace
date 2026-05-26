# quantumentangled marketplace

Curated Claude Code plugins from Quantum Entangled — agents, skills, and workflows that just work.

## Add the marketplace

From inside Claude Code:

```
/plugin marketplace add quantumentangled-dev/quantumentangled-marketplace
```

Or non-interactively:

```bash
claude plugin marketplace add quantumentangled-dev/quantumentangled-marketplace
```

## Install plugins

```
/plugin install <plugin-name>@quantumentangled
```

## Available plugins

| Plugin | Install | What it does |
| :--- | :--- | :--- |
| [`threejs-skills`](https://github.com/quantumentangled-dev/threejs-skills) | `/plugin install threejs-skills@quantumentangled --scope project` | Ten Three.js skills (fundamentals, geometry, materials, lighting, textures, animation, loaders, shaders, postprocessing, interaction) auto-loaded by Claude Code when working with Three.js. |
| [`ai-docs`](https://github.com/quantumentangled-dev/ai-docs-plugin) | `/plugin install ai-docs@quantumentangled` | Author AI-friendly HTML decks, reports, and dashboards via natural language; one-shot publish to GitHub Pages with a paired `.md` source for AI consumers. |

## More

- Marketplace authoring docs: <https://code.claude.com/docs/en/plugin-marketplaces>
- Discover & install plugins: <https://code.claude.com/docs/en/discover-plugins>
