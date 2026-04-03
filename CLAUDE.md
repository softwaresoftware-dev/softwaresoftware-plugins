# CLAUDE.md — softwaresoftware-marketplace (marketplace registry)

This is the `softwaresoftware-plugins` marketplace registry (`softwaresoftware-dev/softwaresoftware-marketplace`). Not a plugin itself — it lists all published plugins for discovery.

## Key Files

- `.claude-plugin/marketplace.json` — plugin entries with name, source, version, category, capability fields
- `capabilities/*.json` — capability contracts (e.g., notification, scheduling, send-sms)

## Version Bump Procedure

Run from the **plugin's own directory**, not here.

1. Update version in `.claude-plugin/plugin.json` + `pyproject.toml` (if Python)
2. Grep old version string to catch stragglers
3. `git commit -m "bump version to X.Y.Z" && git push && git tag vX.Y.Z && git push origin vX.Y.Z`
4. Update this repo's `marketplace.json` with new version, commit + push
5. Update Directory Layout table in `plugins/CLAUDE.md`

**Before bumping:** `make test` passes, version consistent across all files.

## Publish New Plugin

1. Plugin needs: `.claude-plugin/plugin.json` (name, description, version, author, license, keywords), at least one skill, GitHub repo with commits
2. Verify locally: `claude --plugin-dir /path/to/<plugin>`
3. Tag + push: `git tag v$(jq -r .version .claude-plugin/plugin.json) && git push origin --tags`
4. Add entry to `marketplace.json`:
   ```json
   {
     "name": "<plugin-name>",
     "source": { "source": "github", "repo": "ThatcherT/<plugin-name>" },
     "description": "<one-line>",
     "version": "<version>",
     "author": { "name": "Thatcher" },
     "homepage": "https://plugins.softwaresoftware.dev",
     "license": "MIT",
     "keywords": [],
     "category": "<development|research|utilities|provider|framework|toolkit>",
     "requires": [],
     "optional": [],
     "provides": [],
     "environment": {}
   }
   ```
5. Commit + push this repo
6. Add rows to `plugins/CLAUDE.md` (Statusboard + Directory Layout)
7. Add row to parent statusboard at `/home/thatcher/projects/softwaresoftware/projects/CLAUDE.md`

## plugins.softwaresoftware.dev

`plugins.softwaresoftware.dev` is the web face of this marketplace registry. Its content should reflect `marketplace.json` — plugin cards, versions, descriptions, install commands, and "Learn more" links to each plugin's dedicated site.

When `marketplace.json` changes (new plugin, version bump, description update), update `../staticsites/plugins.softwaresoftware.dev/` to match and deploy with `/liteframe:deploy`.

## Capability System

Plugins declare dependencies on capabilities via `requires`/`provides` fields in marketplace.json. softwaresoftware handles resolution. To add a new capability: add providers to marketplace.json with `provides` and `environment` conditions.
