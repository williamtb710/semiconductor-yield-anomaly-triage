# Data directory

`raw/` and `processed/` are intentionally ignored by Git.

The audit notebook downloads `secom.zip` directly from the official UCI archive, verifies the expected SHA-256 hash, and extracts:

- `secom.data`
- `secom_labels.data`
- `secom.names`

Do not commit mirrors, manually edited copies, or derived split files. The modeling notebook reproduces derived tables and figures from the official raw files and documented split configuration.
