# Project 4 Commands

Use these commands to gather evidence. Paste your own outputs into the required
deliverables.

## Storage Audit

```sh
chmod +x code/storage_audit.sh
./code/storage_audit.sh
```

## pgvector

Start Postgres with pgvector using the environment from your course materials,
then run the starter SQL:

```sh
psql "$DATABASE_URL" -f code/pgvector_setup.sql
```

After loading test embeddings and building the HNSW index, run the size query
from the starter SQL and record the actual output in `SIZING.txt` and
`REPORT.docx`.

## YAML Check

```sh
python3 - <<'PY'
import yaml
with open("data_classification.yaml", "r", encoding="utf-8") as f:
    yaml.safe_load(f)
print("YAML parses")
PY
```

If PyYAML is not installed, use your editor or another YAML validator.
