---
name: impc-assistant
description: Coordinate IMPC research requests spanning phenotype downloads, IMPReSS parameter discovery, ES-cell coordinates, orthology, or publications. Use as the IMPC Assistant entry point or when a request combines these workflows.
---

# IMPC Assistant

Use the current application's agent to carry out the request. No custom agent
or particular model is required. Read only the relevant skill files below;
resolve these paths relative to this file, including when installed in a plugin
cache. Host skill names may have a plugin namespace; file paths are the fallback.

## Choose the workflow

| Requested result | Instructions |
| --- | --- |
| IMPReSS pages, procedure/parameter discovery, or a triplet table | [get-impress-data](../get-impress-data/SKILL.md) |
| Phenotype observations, associations, statistics, images, disease models, or data exports | [impc-data-retriever](../impc-data-retriever/SKILL.md) |
| ES-cell or targeting-vector construct coordinates | [get-es-cell-coordinates](../get-es-cell-coordinates/SKILL.md) |
| Missing MCP tools or explicit connection setup | [impc-mcp-setup](../impc-mcp-setup/SKILL.md) |
| Python runtime prerequisite | [uv](../uv/SKILL.md), only when a selected workflow needs it |

For orthology, publications, and allele/product lookups, consult the
[server inventory](../impc-mcp-setup/references/servers.md), then inspect the
connected tool's actual input schema. These are direct tool workflows, not
additional bundled specialist skills. Preserve one-to-many ortholog mappings
and species; publication matches alone do not establish a gene-phenotype claim.

## Coordinate dependent steps

1. Identify the requested deliverable and any essential missing identifiers.
   For a single-purpose request, follow the specialist skill directly.
2. Discover connected MCP tools by server identity/endpoint and operation.
   Names such as `mcp__impc_solr__solr_query` are host-dependent aliases, not
   required literal names. Inspect the actual schema before calling a tool.
   Reuse existing connections; do not install duplicate servers just because
   their names differ. If unavailable, follow the setup skill for that host.
3. Run prerequisites before dependent retrieval. For a human gene request,
   resolve mouse orthologs before querying mouse phenotypes. For a request
   combining IMPReSS pages and observations, resolve the selected triplets
   first, then pass the exact stable IDs to retrieval. Data-only requests use
   the retriever's existing triplet discovery and do not require IMPReSS MCP.
4. Pass exact gene IDs, species, allele names, full pipeline/procedure/parameter
   triplets, requested filters, source URLs, and output paths between steps.
   Distinguish numeric IMPReSS keys from stable IDs. Never reinterpret an
   ambiguous selection silently or drop a pipeline/procedure context.
5. Follow each specialist's confirmation and scientific validation rules.
   Keep construct-relative coordinates distinct from genomic coordinates.
   Report partial results when an independent branch succeeds and another
   fails; a failed query is not evidence that no data exist.
6. Combine the results with source links, files, applied filters, and material
   limitations. Retain the specialist provenance reports. For direct MCP
   lookups record the actual tool, arguments, source, and outcome alongside
   any exported result. Do not claim unexecuted steps succeeded.

## Files and execution

Keep generated data and Python environments in the user's working/output
folder, never in the installed plugin directory. Resolve bundled script paths
from the containing skill's directory and quote absolute paths when invoking
scripts from another working directory. Choose distinct output subdirectories
for multiple workflows so their README reports do not overwrite each other.

A host without filesystem or shell access cannot run the Python workflows.
Explain the missing capability and return only results that available tools
actually support. A skill is guidance, not a replacement execution runtime.

## Example routing

- "Find cornea opacity parameter pages": IMPReSS discovery only; clarify
  genuinely different parameter matches before producing the final table.
- "Download body weight observations": retrieval and its triplet discovery.
- "Find the IMPReSS page and download observations for this parameter":
  IMPReSS discovery, selected full triplets, then retrieval.
- "Get ES-cell coordinates from this GenBank file": coordinate parser;
  no MCP connection is needed for a local file.
- "Find mouse phenotypes for this human gene and related papers": orthology,
  phenotype retrieval for resolved mouse IDs, publication search, then a
  synthesis that distinguishes direct evidence from literature relevance.
