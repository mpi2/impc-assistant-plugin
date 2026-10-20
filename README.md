# IMPC Assistant plugin

An installable collection of IMPC research workflows for desktop AI agents.
Includes six skills and four hosted MCP connections. Your application supplies
the agent and model; this repository supplies IMPC instructions and scripts.

## What it does

- Retrieve phenotype observations, associations, statistics, images, and disease-model data.
- Find IMPReSS pipeline/procedure/parameter identifiers and links.
- Extract ES-cell or targeting-vector construct coordinates from GenBank files.
- Use connected tools for publications, human–mouse orthology, and allele products.
- Coordinate multi-step requests and preserve source identifiers and provenance.

## Install from GitHub

The GitHub repository is `impc/impc-assistant-plugin`. A private repository
requires Git access for each recipient. The
marketplace identifier is `impc-private`; that name does not enforce privacy.
Repository access settings determine who can download the package.

### Codex desktop

Register the marketplace using the Codex CLI, or ask Codex to do this for you:

```sh
codex plugin marketplace add impc/impc-assistant-plugin
```

Restart the desktop app, open Plugins, choose the IMPC Assistant marketplace,
and install `impc-assistant`. Start a new chat to load its skills and connections.
If Terminal says `codex: command not found`, ask the desktop agent to locate its
bundled CLI and register the repository; the CLI is not always on your PATH.

### Claude Code

Run these slash commands in Claude Code:

```text
/plugin marketplace add impc/impc-assistant-plugin
/plugin install impc-assistant@impc-private
```

Reload or restart as requested. You can explicitly invoke the entry skill with
`/impc-assistant:impc-assistant`.

### Cursor

`plugins/impc-assistant/` is a portable Agent Plugins package. Use your supported
Cursor plugin import/distribution workflow with that directory. The marketplace
files here target Codex and Claude Code; they do not establish a verified Cursor
GitHub installation flow. For a pilot, a technical helper can copy the bundled
skill directories into a project's `.cursor/skills/` and use `impc-mcp-setup`
to configure the existing IMPC endpoints, preserving any existing settings.

## First test

Start a chat in a writable test folder, outside this repository, and ask:

> Use IMPC Assistant to find cornea opacity IMPReSS parameter pages.

Expect a live query and a table of identifiers and links, or a clarification
when multiple parameters match. Then try:

> Use IMPC Assistant to retrieve at most 10 phenotype associations for Pax6 and save them as CSV.

Check actual tool execution, the CSV, and the provenance README. Six skills and
four MCP connections should be discoverable. Do not treat a textual claim that
the plugin works as proof of a successful connection or query.

## Requirements and limitations

- A desktop agent with skill support, filesystem/shell access for Python tasks,
  and permission to connect to the IMPC services.
- The data retrieval workflow requires uv and impc-api; skills check prerequisites
  when used. Installation of the plugin alone does not install those dependencies.
- Result files and Python environments belong in the user's working/output folder.
- Workspace policies may restrict plugins or MCP connections. GitHub hosting does
  not bypass these policies or publish the plugin to a public plugin directory.
- Tool discovery has been observed in the local Codex installation. Full live
  workflow testing across Codex, Claude Code, and Cursor is still required.

## Repository layout

```text
.agents/plugins/marketplace.json   # Codex marketplace
.claude-plugin/marketplace.json    # Claude Code marketplace
plugins/impc-assistant/
  plugin.json                     # Portable manifest
  mcp.json                        # Four Streamable HTTP connections
  .claude-plugin/plugin.json      # Claude Code compatibility manifest
  .mcp.json                       # Claude Code connection configuration
  assets/icon.svg
  skills/                         # Six skills and supporting resources
LICENSE
```

Both marketplace entries point to the same plugin directory. Keep hidden files
when copying the repository. No credentials, Git history from another repository,
Python environments, or generated research data are part of the package.

## Maintain and release

Edit the plugin under `plugins/impc-assistant/` in this repository. Keep versions
in its root and Claude manifests aligned, and keep its two MCP configuration
files consistent. Refresh the marketplace and update/reinstall the plugin through
each client's supported workflow after publishing changes; users should not
assume an already running chat has loaded a new release.

This repository is a distribution copy of the original development package.
If you continue editing that original, explicitly sync and review those changes
here before your next release; the two directories are not linked automatically.

The code is distributed under Apache-2.0; retain LICENSE and the uv skill's NOTICE.

## References

- [Codex plugin packaging](https://developers.openai.com/plugins/build/plugins)
- [Claude Code marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- [Cursor plugin format](https://cursor.com/docs/reference/plugins)

## Claude Code trailing-slash connection workaround

Version 0.1.1 adds `?client=claude` after the trailing slash in the Claude-specific
`.mcp.json` URLs, for example:

```text
https://www.ebi.ac.uk/mi/impc/mcp/solr/?client=claude
```

On 2026-10-20, all four IMPC endpoints returned HTTP 200 with valid MCP initialize
results for both the canonical slash URL and this query-string form. Removing the
slash returned HTTP 301. This workaround keeps the slash before the query string;
it does not change the service or add authentication. Portable `mcp.json` keeps
the canonical URLs.

Claude Code itself was unavailable in the validation environment, so successful
reconnection there is not yet verified. After publishing this release, refresh
the marketplace and update the installed plugin through Claude Code's plugin
manager, then start a new session and check `/mcp`. If it still fails, capture
the effective endpoint and connection error, removing any credentials before
sharing. Do not edit the installed plugin cache: those edits are replaced by updates.

A similar URL-normalization issue and the query-string workaround are reported
for Claude web connectors; that report alone does not establish a Claude Code bug:
https://github.com/anthropics/claude-ai-mcp/issues/234
