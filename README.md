# Location Resolution Pipeline

A cascading four-stage pipeline for resolving messy location data against a standardized world reference dataset on Databricks.

## Pipeline Stages

### 1. Exact Match
SQL-based exact join on (city, state, country) fields against the reference dataset. Handles clean, standardized location records.

### 2. Fuzzy Match
Uses Levenshtein distance to handle typos, diacritics, and minor spelling variations. Applies weighted distance calculation with configurable confidence thresholds (e.g., 85%+) to auto-accept high-quality matches.

### 3. Vector Search
Leverages Databricks Mosaic AI Vector Search with hybrid search and reranking to handle semantic variations, abbreviations (e.g., "NYC" → "New York City"), and airport codes (e.g., "BLR" → "Bangalore").

### 4. LLM Validation
Uses large language models via `ai_query` batch inference to resolve remaining ambiguous cases, nicknames (e.g., "Silicon Valley"), and records requiring contextual reasoning.

## Architecture Benefits

- **Cost-optimized**: Simple cases handled by SQL/fuzzy logic, expensive LLM calls reserved for complex cases
- **Performance**: Cascading design processes 90-95% of records in early stages
- **Accuracy**: Achieves 85%+ average confidence across all resolution stages
- **Traceable**: Each record tagged with resolution stage and confidence score

## Requirements

- Databricks workspace with Unity Catalog
- Mosaic AI Vector Search endpoint
- LLM endpoint (e.g., `databricks-gpt-5-mini`)
- Reference dataset: world cities table with standardized location data
