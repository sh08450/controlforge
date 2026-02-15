# TrustStack AI GRC — **AI GRC Workbench**
![TrustStack AI GRC Hero](docs/images/hero.png)

**Tagline:** Config-driven packs → controls → evidence → audit-ready.  
**Control panel:** TrustStack AI Assurance Hub  
**Repo:** `controlforge-ai-grc`

TrustStack AI GRC turns AI regulations and security frameworks into a **practical, trackable checklist** for a specific AI use case — and helps you **store evidence** and produce an **audit-ready report**.

## Application Screenshot
![TrustStack AI Assurance Hub screenshot](docs/images/application-screenshot.svg)

## What you tell it
- Industry + segment + use case (config-driven taxonomy)
- Scoping answers (questionnaire defined by the use case)
- Which packs to apply (security / safety / governance)

## What it does
1. Generates a deterministic checklist of required controls  
2. Explains *why* each control applies (rule + triggered context)  
3. Suggests implementation patterns/tools (config-driven)  
4. Tracks status/owners/notes  
5. Stores evidence with hashes + an immutable audit log  
6. Exports an audit-ready report (HTML/JSON/CSV; PDF scaffold included)

> **Not legal advice.** Packs provide structured obligations/checklists but do not replace legal counsel.

---

## Monorepo layout (simple + folder-driven)

- `registry/`
  - `taxonomy/` → industries/segments/use-cases (discovered by folder conventions)
  - `packs/` → versioned packs (discovered by folder conventions)
  - `suggestions/` → patterns/tools catalog (optional)
- `workspaces/` → file-based projects (no DB)
- `apps/api/` → FastAPI service (pack loader, mapping engine, reporting, file storage)
- `apps/web/` → Next.js UI scaffold (“TrustStack AI Assurance Hub”)
- `docs/` → architecture + authoring guides
- `schemas/` → JSON Schemas for packs, taxonomy, projects

---

## Quickstart (dev)

### 1) API (FastAPI)
#### macOS / Linux
```bash
cd apps/api
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
export TRUSTSTACK_CONFIG_ROOT=../../registry
export TRUSTSTACK_WORKSPACE_ROOT=../../workspaces
uvicorn truststack_grc.main:app --reload --port 8000
```

#### Windows (PowerShell)
```powershell
cd apps/api
py -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
$env:TRUSTSTACK_CONFIG_ROOT = "../../registry"
$env:TRUSTSTACK_WORKSPACE_ROOT = "../../workspaces"
uvicorn truststack_grc.main:app --reload --port 8000
```

### 2) Web (Next.js)
#### macOS / Linux
```bash
cd apps/web
npm install
npm run dev
```

#### Windows (PowerShell)
```powershell
cd apps/web
npm install
npm run dev
```
Open:
- API: http://localhost:8000/docs
- Web: http://localhost:3000

### 3) Try it
1) Create a project in the web UI (or via API)  
2) Generate the checklist  
3) Mark controls complete + upload evidence  
4) Export a report

---

## Extending by folder conventions (no code changes)

### Add a new industry / segment / use case
Create a new folder and YAML file under `registry/taxonomy/industries/…`:

```
registry/taxonomy/industries/<industry_id>/industry.yaml
registry/taxonomy/industries/<industry_id>/segments/<segment_id>/segment.yaml
registry/taxonomy/industries/<industry_id>/segments/<segment_id>/use-cases/<use_case_id>/use_case.yaml
```

### Add a new pack (standard/framework)
Drop a new pack folder under `registry/packs/<domain>/<pack_id>/<version>/`:

```
registry/packs/governance/eu-ai-act/2024-1689/pack.yaml
registry/packs/governance/eu-ai-act/2024-1689/controls/*.yaml
```

The pack registry discovers it automatically at runtime.

---

## Key concepts
- **Taxonomy**: Industry → Segment → Use Case (all config)
- **Packs**: versioned catalog(s) of controls with applicability rules
- **Context**: normalized object derived from scoping answers
- **Checklist**: generated control instances stored in a project workspace folder
- **Evidence**: file uploads with SHA-256 hashes + metadata
- **Audit log**: append-only NDJSON trail of state changes

---

## License
Apache-2.0 (see `LICENSE`).
