# CLAUDE.md — claude-plugins (marketplace registry)

This is the `nov-plugins` marketplace registry (`ThatcherT/nov-plugins`). Not a plugin itself — it lists all published plugins for discovery.

## Key Files

- `.claude-plugin/marketplace.json` — plugin entries with name, source, version, category, capability fields
- `capabilities/*.json` — capability contracts (e.g., notification, scheduling)

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
     "homepage": "https://plugins.nov.solutions",
     "license": "MIT",
     "keywords": [],
     "category": "<development|research|utilities|provider|framework>",
     "requires": [],
     "optional": [],
     "provides": [],
     "environment": {}
   }
   ```
5. Commit + push this repo
6. Add rows to `plugins/CLAUDE.md` (Statusboard + Directory Layout)
7. Add row to parent statusboard at `/home/thatcher/projects/nov/projects/CLAUDE.md`

## Capability Contracts

Contracts in `capabilities/*.json` define MCP tool signatures that providers must implement. Each has `name`, `version`, and `tools` array.

To add a new capability: create `capabilities/<name>.json`, then add providers to marketplace.json with `provides: ["<name>"]` and `environment` conditions for auto-selection.
