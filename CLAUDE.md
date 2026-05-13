# centuria-python-config — shared lint + type defaults

## Ce e
Repo public cu configurări Ruff + mypy folosite de toate proiectele Python din ecosistemul Centuria. Single source of truth pentru linting/typing — pin pe tag, nu pe `main`.

Remote: `git@github.com:centuriasolutions/centuria-python-config.git`

## Conținut

- **`ruff.toml`** — `target-version = py311`, `line-length = 100`, format single-quote/lf.
  - Selecție mare de rules: E, F, W, I, B, UP, SIM, RUF, PERF, FURB, PL, S, TID, N, ANN, ASYNC.
  - Ignored: `ANN101/102` (obsolet în 3.11+), `PLR0913` magic, `PLR2004` magic values, `PLC0415` (lazy imports pentru torch/whisper/transformers).
  - `extend-immutable-calls` cu FastAPI (`Depends`, `Query`, `Path`, `Body`, etc.) + Typer (`Argument`, `Option`) — fără asta orice handler FastAPI trip-uia B008.
  - Per-file overrides: `tests/**` (S101 + ANN ok), `__init__.py` (F401 re-exports).
- **`mypy.ini`** — `python_version = 3.11`, `strict = True`, plus `warn_unreachable`, `disallow_untyped_defs`, `no_implicit_optional`. Tests excluse de la `disallow_untyped_defs`.

## Reminder important
**Public repo** — fără infra/credentials/IPs interne. Vezi feedback `public_repos` din MEMORY.md.

## Adoption

| Proiect | Folosește |
|---------|-----------|
| augur | da (`pyproject.toml`) |
| restul Python (egg-eyes, egg-voice, rag-service, rag-torrent-curator, scripts crypto-scan/nvr-transcribe) | nu încă — candidate de standardizare |

Verificare rapidă: `grep -l centuriasolutions/centuria-python-config ~/Work/bogdan/*/pyproject.toml`.

## Cum se consumă

```toml
# pyproject.toml — recomandat (pin pe tag)
[tool.ruff]
extend = "https://raw.githubusercontent.com/centuriasolutions/centuria-python-config/v0.1.0/ruff.toml"
```

**Caveat Ruff <0.16**: nu rezolvă HTTPS în `extend`. Workaround: vendor (`curl` fișierul în repo) și refresh manual când iese tag nou. Nu folosi `main` în URL — un commit aici sparge CI peste tot.

Pentru mypy: copiază `mypy.ini` în root sau echivalent în `[tool.mypy]` din `pyproject.toml` (mypy nu suportă `extend` din URL).

## Versionare
Tags semver. Bump pe orice schimbare de rules. Consumatorii pin explicit la tag.
