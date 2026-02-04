# USPTO Open Data Portal Doc Mirror

A lightweight bash tool that mirrors the official [USPTO Open Data Portal](https://data.uspto.gov) API documentation locally, with automatic change detection, changelog generation, and optional knowledge-graph integration.

## Why This Exists

- **Offline Access**: Read USPTO API specs without internet
- **Change Tracking**: Know when USPTO updates their API specifications
- **Version History**: Maintain a changelog of documentation changes
- **LLM Context**: Feed complete, up-to-date API specs into your AI workflows
- **Graph Integration**: Optionally record every change in your knowledge graph

## What Gets Mirrored

The USPTO ODP site is a JavaScript SPA — direct page fetches return empty HTML shells. This mirror uses a hybrid approach:

| Source | Files | Description |
|--------|-------|-------------|
| OpenAPI/Swagger specs | 8 YAML files | Complete API definitions for all 38 endpoints |
| USPTO PDFs | 2 files | PEDS-to-ODP mapping + Query spec |
| Web pages | 2 files | Getting started + syntax examples (may be JS shells) |

### API Coverage (38 endpoints)

| Group | Endpoints | Spec File |
|-------|-----------|-----------|
| Patent Applications | 13 | swagger.yaml |
| Bulk Datasets | 3 | swagger.yaml |
| Petition Decisions | 3 | swagger.yaml |
| PTAB Proceedings | 4 | trial-proceedings.yaml |
| PTAB Decisions | 5 | trial-decisions.yaml |
| PTAB Documents | 5 | trial-documents.yaml |
| PTAB Appeals | 4 | trial-appeal-decisions.yaml |
| PTAB Interferences | 4 | trial-interferences.yaml |

## Setup

```bash
git clone https://github.com/MatoTeziTanka/uspto-doc-mirror.git
cd uspto-doc-mirror
chmod +x bin/uspto-docs-sync.sh
mkdir -p data/uspto logs
export DOC_MIRROR_BASE="$(pwd)"
```

## Usage

### Manual Sync

```bash
export DOC_MIRROR_BASE="$(pwd)"
./bin/uspto-docs-sync.sh
```

### Scheduled Sync (Cron)

```bash
0 7,19 * * * cd /opt/uspto-doc-mirror && DOC_MIRROR_BASE=/opt/uspto-doc-mirror ./bin/uspto-docs-sync.sh >> logs/cron.log 2>&1
```

### View Changes

```bash
cat data/uspto/CHANGELOG.md
cat data/uspto/hashes.manifest
```

## Directory Structure

```
uspto-doc-mirror/
├── bin/
│   └── uspto-docs-sync.sh     # Main sync script
├── config/
│   └── sources.yaml            # Source configuration
├── data/
│   └── uspto/
│       ├── *.yaml              # Mirrored OpenAPI specs (gitignored)
│       ├── *.pdf               # Mirrored USPTO docs (gitignored)
│       ├── hashes.manifest     # SHA-256 hashes (gitignored)
│       └── CHANGELOG.md        # Auto-generated change log (tracked)
├── logs/                       # Sync logs (gitignored)
├── .gitignore
├── LICENSE
└── README.md
```

## API Authentication

USPTO ODP requires an API key passed as `X-API-KEY` header. Get yours at [data.uspto.gov/apis/getting-started](https://data.uspto.gov/apis/getting-started).

```bash
curl -H "X-API-KEY: your-key" "https://api.uspto.gov/api/v1/patent/applications/search?q=robot"
```

## Requirements

- Bash 4.0+
- curl
- sha256sum (coreutils)

## License

MIT License

---

*Developed by [Mato Tezi Tanka](https://github.com/MatoTeziTanka)*
