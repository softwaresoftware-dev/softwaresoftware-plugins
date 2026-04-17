# softwaresoftware-plugins

Claude Code plugin marketplace by [softwaresoftware.dev](https://softwaresoftware.dev).

Browse the catalog at [plugins.softwaresoftware.dev](https://plugins.softwaresoftware.dev).

## What's here

- `.claude-plugin/marketplace.json` — the registry. 35 plugins I've built, plus 6 from Anthropic's official directory (browser automation, deploy targets).
- `capabilities/*.json` — capability contracts. Abstract things like `notification`, `channel`, `human-approval`, `deploy`. Providers implement them. Plugins depend on them. The resolver picks the right provider for your environment.

Plugins cover SaaS scaffolding (zapframe, ironframe), static sites (liteframe), deploys (nginx-cloudflare-deploy, dockside), notifications across Linux / Windows / Slack / email / Termux, browser automation, background agents (taskpilot), Reddit research (gummymine), secure-fill vault providers, and more.

## Install

In your terminal:

```bash
claude plugin marketplace add softwaresoftware-dev/softwaresoftware-plugins
claude plugin install softwaresoftware@softwaresoftware-plugins
```

The second line installs the resolver. Then inside Claude Code:

```
/softwaresoftware:install <plugin-name>
```

The resolver probes your environment, figures out which providers you need, and installs everything in the right order. You don't pick providers by name. You install a plugin and the resolver handles the capability graph.

## How the capability system works

Plugins declare `requires`, `optional`, or `provides` against capability names. Install something that needs `notification` and the resolver checks what's on your system. Linux with `notify-send`? You get `notify-linux`. Slack webhook configured? You get `notify-slack`. Same contract, different provider.

Skills reference capabilities by intent, never by tool name. "Send a notification. Use an available skill or tool." Plugins stay decoupled from each other.

## License

Proprietary. See [LICENSE](LICENSE).

You can use this marketplace to install plugins for personal use. You can't modify it, redistribute it, mirror it, fork it to run a competing registry, or use it commercially without a paid license. Email for commercial terms.
