
# Local Data Lake Pipeline

Pipeline ETL local qui ingère un fichier CSV, le transforme, le stocke en Parquet partitionné, et permet des requêtes SQL via DuckDB.

## Objectif
Démontrer les concepts d'un data lake moderne (stockage optimisé, partitionnement, requêtage SQL) sans dépendre d'un service cloud.

## Technologies
- Python
- Pandas (manipulation de données)
- PyArrow (écriture Parquet)
- DuckDB (requêtes SQL sur fichiers Parquet)

## Architecture
1. Extraction : lecture du CSV brut (`data/raw/orders.csv`)
2. Transformation : conversion des types, ajout d'une colonne de partition (`year_month`)
3. Chargement : écriture en Parquet partitionné dans `data/processed/`
4. Interrogation : DuckDB exécute une requête agrégée sur les fichiers Parquet

```mermaid
flowchart LR
    A[CSV brut] --> B[Script Python]
    B --> C[Parquet partitionné]
    C --> D[DuckDB SQL]
